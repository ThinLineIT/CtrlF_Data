# Coroutine Exceptions Handling

<!--Table of Contents-->

- 코루틴 예외 전파
- CoroutineExceptionHandler
- Cancellation and Exceptions
- 예외 통합 (Exception aggregation)
- Supervision

<!-- 어떤 질문을 대답할 수 있어야 하는지-->

## You can answer

- 코루틴의 예외를 핸들링 하는 방법

<!--Contents-->

---

이미 취소된 코루틴은 중단점에서 cancellation을 던짐. 그러나 취소 동작 도중에 예외가 발생하거나 2개 이상의 자식 코루틴들에서 동시에 예외가 발생한다면 ?
### 예외 전파
코루틴 빌더들은 예외처리 방식에 따라 두 가지 타입으로 나눈다.
1) 예외를 자동으로 전파 - launch / actor  
   : Java의 Thread.uncaughtExceptionHanlder와 유사하게 처리되지 않은 예외로 간주

2) 사용자에게 노출하여 예외처리를 맡김 - async / produce  
   : 마지막으로 예외를  처리하는 예외 처리 핸들러에게 예외 처리를 맡기는 방식 (상위에 떠넘기기)

  ```kotlin
fun main(args: Array<String>) = runBlocking<Unit> {
    val job = GlobalScope.launch {
        println("Throwing exception from launch")
        throw IndexOutOfBoundsException() // Will be printed to the console by Thread.defaultUncaughtExceptionHandler
    }

    job.join()
    println("Joined failed job")
    val deferred = GlobalScope.async {
        println("Throwing exception from async")
        throw ArithmeticException() // Nothing is printed, relying on user to call await
    }

    try {
        deferred.await()
        println("Unreached")
    } catch (e: ArithmeticException) {
        println("Caught ArithmeticException")
    }
}
```
```
Throwing exception from launch
Exception in thread "DefaultDispatcher-worker-1" java.lang.IndexOutOfBoundsException
	at d.PropagationKt$main$1$job$1.invokeSuspend(propagation.kt:11)
	at kotlin.coroutines.jvm.internal.BaseContinuationImpl.resumeWith(ContinuationImpl.kt:33)
	at kotlinx.coroutines.DispatchedTask.run(DispatchedTask.kt:56)
	at kotlinx.coroutines.scheduling.CoroutineScheduler.runSafely(CoroutineScheduler.kt:571)
	at kotlinx.coroutines.scheduling.CoroutineScheduler$Worker.executeTask(CoroutineScheduler.kt:738)
	at kotlinx.coroutines.scheduling.CoroutineScheduler$Worker.runWorker(CoroutineScheduler.kt:678)
	at kotlinx.coroutines.scheduling.CoroutineScheduler$Worker.run(CoroutineScheduler.kt:665)
Joined failed job
Throwing exception from async
Caught ArithmeticException
```

### CoroutineExceptionHandler (코루틴 예외 핸들러)
![imgs](./img/coroutineContext.png)  
코루틴에서는 root coroutine(최상위 코루틴)에 CoroutineExceptionHandler 컨텍스트 요소를 설정하면 이 핸들러는 범용적으로 catch 블록과 같이 사용된다.  
예외 핸들러가 호출된 코루틴은 이미 발생한 예외로 인해 종료되고 복구될 수 없다.  
일반적으로 예외 핸들러는 예외를 로깅하거나 관련 예외 메시지를 개발자에게 보여주고 애플리케이션을 종료하거나 재시작하기 위해 사용된다.  
이 코루틴 예외 핸들러는 개발자에 의해 처리되지 않은 예외에 한해서만 호출된다.  
특히, 모든 자식 코루틴들은 예외 처리를 그들의 부모 코루틴에게 위임하고, 그 부모는 또 그 부모의 코루틴에게 위임하며, 결국 루트 코루틴에 도달할 때까지 예외처리를 위임한다.  
따라서, 자식 코루틴들에게 설치된 coroutineExceptionHandler는 절대 사용되지 않는다.  
추가적으로 async 코루틴 빌더는 항상 모든 예외를 잡아 (catch) 결과 객체인 deffered 객체를 통해 노출한다. 그래서 coroutineExceptionHandler가 효과가 없다.


```kotlin
fun main(args: Array<String>) = runBlocking<Unit> {
    val handler = CoroutineExceptionHandler { _, exception ->
        println("Caught $exception")
    }
    val job = GlobalScope.launch(handler) {
        throw AssertionError()
    }
    val deferred = GlobalScope.async(handler) {
        throw ArithmeticException() // Nothing will be printed, relying on user to call deferred.await()
    }

    joinAll(job, deferred)
}
```
```
Caught java.lang.AssertionError
```
launch 블록에 대해서만 예외처리가 이루어짐.

### 취소와 예외 (Cancellation and exceptions)
코루틴은 cancellation을 위해 cancellationException을 사용하며, 이 예외들은 모든 핸들러들에게 무시되므로 부가적인 디버깅 정보 획득용으로만 사용하는 것이 좋음.   
코루틴이 Job.cancel()로 인해 취소되면 자신의 실행을 종료하지만 부모 코루틴에게 취소를 요청하지는 않음.
```kotlin
fun main(args: Array<String>) = runBlocking<Unit> {
    val job = launch {
        val child = launch {
            try {
                delay(Long.MAX_VALUE)
            } finally {
                println("Child is cancelled")
            }
        }
        yield()
        println("Cancelling child")
        child.cancel()
        child.join()
        yield()
        println("Parent is not cancelled")
    }
    job.join()
}
```
```
Cancelling child
Child is cancelled
Parent is not cancelled
```
최초 발생한 예외는 모든 자식 코루틴이 종료된 후에야 부모코루틴에 의해 처리됨.
```kotlin
fun main(args: Array<String>) = runBlocking<Unit> {
    val handler = CoroutineExceptionHandler { _, exception ->
        println("Caught $exception")
    }
    val job = GlobalScope.launch(handler) {
        launch { // the first child
            try {
                delay(Long.MAX_VALUE)
            } finally {
                withContext(NonCancellable) {
                    println("Children are cancelled, but exception is not handled until all children terminate")
                    delay(100)
                    println("The first child finished its non cancellable block")
                }
            }
        }
        launch { // the second child
            delay(10)
            println("Second child throws an exception")
            throw ArithmeticException()
        }
    }
    job.join()
}
```
```
Second child throws an exception
Children are cancelled, but exception is not handled until all children terminate
The first child finished its non cancellable block
Caught java.lang.ArithmeticException
```
### 예외 통합 ( Exception aggregation)
만약 두 개 이상의 연관된 자식 코루틴들에서 예외가 발생한다면?
기본적으로는 처음 발생한 예외가 우선이며,
처음 발생한 예외가 예외처리메커니즘에 따라 처리되고 그 이후에 발생하는 모든 예외들은 첫 번째 예외에 숨겨져 추가됨.
```kotlin
fun main(args: Array<String>) = runBlocking<Unit> {
    val handler = CoroutineExceptionHandler { _, exception ->
        println("Caught $exception with suppressed ${exception.suppressed.contentToString()}")
    }
    val job = GlobalScope.launch(handler) {
        launch {
            try {
                delay(Long.MAX_VALUE)
            } finally {
                throw ArithmeticException()
            }
        }
        launch {
            delay(100)
            throw IOException()
        }
        delay(Long.MAX_VALUE)
    }
    job.join()
}
```
```
Caught java.io.IOException with suppressed [java.lang.ArithmeticException]
```
cancellation exception은 예외 전파에 있어서 기본적으로 unwrapping 됨.

다음 예제와 같이 취소 이외의 예외가 발생하여 코루틴이 취소 될 경우 coroutineExceptionHandler가 예외를 처리할 때는 취소 예외는 바로 unwrapping 되고 최초 발생한 예외가 핸들러의 exception 파라미터로 전달됨.
```kotlin
fun main(args: Array<String>) = runBlocking<Unit> {
    val handler = CoroutineExceptionHandler { _, exception ->
        println("Caught original $exception")
    }
    val job = GlobalScope.launch(handler) {
        val inner = launch {
            launch {
                launch {
                    println("Before the first Exception")
                    throw IOException()
                }
            }
        }
        try {
            inner.join()
        } catch (e: CancellationException) {
            println("Rethrowing CancellationException with original cause")
            throw e
        }
    }
    job.join()
}
```
```kotlin
Before the first Exception
Rethrowing CancellationException with original cause
Caught original java.io.IOException
```
### Supervision (감독)
단방향 취소  :  좋은 예로는 동일 스코프에 정의된 Job을 공유하는 UI 컴포넌트가 적절함.   
만일 UI 자식 task 중 하나가 실패했더라도 전체 UI를 취소할 필요는 없음. 그러나 UI 컴포넌트가 종료되면 그 Job도 취소되고 모든 자식들의 결과물도 필요없게 되므로 자식들의 Job도 취소되어야함. 
또 다른 예는 여러 개의 자식 task를 갖는 서버 프로세스에서 자식 task들의 실행과 실패를 감독하고 실패한 자식 task에 대해서는 다시 시작될 수 있도록 해야함

- Supervision Job  
  일반적인 Job과 유사하지만 예외로 인한 취소가 아래 방향으로만 전파됨 ( 부모 → 자식)

```kotlin
fun main(args: Array<String>) = runBlocking<Unit> {
    val supervisor = SupervisorJob()
    with(CoroutineScope(coroutineContext + supervisor)) {
        // launch the first child -- its exception is ignored for this example (don't do this in practice!)
        val firstChild = launch(CoroutineExceptionHandler { _, _ ->  }) {
            println("First child is failing")
            throw AssertionError("First child is cancelled")
        }
        // launch the second child
        val secondChild = launch {
            firstChild.join()
            // Cancellation of the first child is not propagated to the second child
            println("First child is cancelled: ${firstChild.isCancelled}, but second one is still active")
            try {
                delay(Long.MAX_VALUE)
            } finally {
                // But cancellation of the supervisor is propagated
                println("Second child is cancelled because supervisor is cancelled")
            }
        }
        // wait until the first child fails & completes
        firstChild.join()
        println("Cancelling supervisor")
        supervisor.cancel()
        secondChild.join()
    }
}
```
```
First child is failing
First child is cancelled: true, but second one is still active
Cancelling supervisor
Second child is cancelled because supervisor is cancelled
```
- SupervisionScope  
  범위를 지정하여 scoped concurrency(동시성 제어)를 하기 위해서는 coroutineScope를 사용하는데 이러한 스코프에서 단방향으로 전파되는 취소 메커니즘을 사용하려면 supervisionScope를 사용하면 됨. 
  오직 자신이 실패했을 때만 모든 자식을 취소하며, coroutineScope와 동일하게 모든 자식들의 종료를 기다림.

```kotlin
fun main(args: Array<String>) = runBlocking<Unit> {
    try {
        supervisorScope {
            val child = launch {
                try {
                    println("Child is sleeping")
                    delay(Long.MAX_VALUE)
                } finally {
                    println("Child is cancelled")
                }
            }
            // Give our child a chance to execute and print using yield
            yield()
            println("Throwing exception from scope")
            throw AssertionError()
        }
    } catch(e: AssertionError) {
        println("Caught assertion error")
    }
}
```
```
Child is sleeping
Throwing exception from scope
Child is cancelled
Caught assertion error
```
- 감독 중인 코루틴들에서의 예외  
  
    일반적인 Job과 Supervisor Job의 중요한 차이점은 예외처리방식! 모든 자식은 예외 처리 메커니즘을 통해서 자신의 예외를 처리해야함. 이러한 차이점은 자식의 실패가 부모로 전파되지 않는다는 사실로부터 발생함.

    즉, supervisorScope { } 안에서 실행 된 코루틴 들은 그들의 스코프에 설정된 coroutineExceptionHandler를 사용한다는 것을 의미함.
```kotlin
fun main(args: Array<String>) = runBlocking<Unit> {
    val handler = CoroutineExceptionHandler { _, exception ->
        println("Caught $exception")
    }
    supervisorScope {
        val child = launch(handler) {
            println("Child throws an exception")
            throw AssertionError()
        }
        println("Scope is completing")
    }
    println("Scope is completed")
}
```
```
Scope is completing
Child throws an exception
Caught java.lang.AssertionError
Scope is completed
```


---

## Reference
- [코루틴 공식가이드](https://kotlinlang.org/docs/exception-handling.html)
