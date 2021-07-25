# Cancellation and Timeouts

<!--Table of Contents-->

- Cancelling coroutine execution
- Cancellation is cooperative
- Making computation code cancellable
- Closing resources with finally
- Run non-cancellable block
- Timeout

<!-- 어떤 질문을 대답할 수 있어야 하는지-->

## You can answer

- 코루틴을 취소하는 방법 ?

<!--Contents-->

---

### Cancelling coroutine execution

기본적인 cancel의 사용법.  
job객체에 cancel()을 걸어주면 코루틴이 취소됨을 보여줌.

  ```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking {
    val job = launch {
        repeat(1000) { i ->
            println("job: I'm sleeping $i ...")
            delay(500L)
        }
    }
    delay(1300L) // delay a bit
    println("main: I'm tired of waiting!")
    job.cancel() // cancels the job
    job.join() // waits for job's completion 
    println("main: Now I can quit.")
}
```

```
job: I'm sleeping 0 ...
job: I'm sleeping 1 ...
job: I'm sleeping 2 ...
main: I'm tired of waiting!
main: Now I can quit.
```

### Cancellation is cooperative

코루틴이 취소되도록 하기 위한 두 가지 방법

1. 취소 여부를 확인하는 suspend 함수를 주기적으로 호출
2. 취소 상태를 명시적으로 확인

*1. suspend 함수 호출*

코루틴 코드가 취소에 협조적이여야 함.  
중단함수는 코루틴의 취소를 확인하고 CancellationException을 던짐.

```kotlin
import kotlinx.coroutines . *

fun main() = runBlocking {
    val startTime = currentTimeMillis()
    val job = launch(Dispatchers.Default) {
        var nextPrintTime = startTime
        var i = 0
        while (i < 5) { // computation loop, just wastes CPU
            // print a message twice a second
            if (currentTimeMillis() >= nextPrintTime) {
                println("job: I'm sleeping ${i++} ...")
                nextPrintTime += 500L
            }
        }
    }
    delay(1300L) // delay a bit
    println("main: I'm tired of waiting!")
    job.cancelAndJoin() // cancels the job and waits for its completion
    println("main: Now I can quit.")
}
```

```
job: I'm sleeping 0 ...
job: I'm sleeping 1 ...
job: I'm sleeping 2 ...
main: I'm tired of waiting!
job: I'm sleeping 3 ...
job: I'm sleeping 4 ...
main: Now I can quit.
```

이 예제에서는 launch 아래 코드가 취소에 협조적이지 않았음. 단순 연산만 존재하고 어떠한 중단함수도 있지않음. 따라서 중단 함수를 사용하여 취소가 가능하도록 해야함.
공식가이드에서는 다음과 같은 상황에 사용할 수 있는 중단 함수로 yield()를 추천해주고있음.

```kotlin
import kotlinx.coroutines.*
import java.lang.Exception

fun main() = runBlocking {
    val startTime = System.currentTimeMillis()
    val job = launch(Dispatchers.Default) {
        try {
            var nextPrintTime = startTime
            var i = 0
            while (i < 5)  // computation loop, just wastes CPU
            // print a message twice a second
                if (System.currentTimeMillis() >= nextPrintTime) {
                    yield()
                    println("job: I'm sleeping ${i++} ...")
                    nextPrintTime += 500L
                }
        } catch (e: Exception) {
            kotlin.io.println("Exception {$e}")
        }
    }
    delay(1300L) // delay a bit
    println("main: I'm tired of waiting!")
    job.cancelAndJoin() // cancels the job and waits for its completion
    println("main: Now I can quit.")
}
```
```
job: I'm sleeping 0 ...
job: I'm sleeping 1 ...
job: I'm sleeping 2 ...
main: I'm tired of waiting!
Exception {kotlinx.coroutines.JobCancellationException: StandaloneCoroutine was cancelled; job=StandaloneCoroutine{Cancelling}@4353fbf5}
main: Now I can quit.
```
yield() 중단 함수에 의해 코루틴이 중단 되었다가 다시 재개되는 시점에 exception 처리가 됨.

*2. 취소 상태 명시적으로 확인*

*isActive는 CoroutineScope 개체를 통해 코루틴 내에서 사용할 수 있는 확장 속성
```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking {
    val startTime = System.currentTimeMillis()
    val job = launch(Dispatchers.Default) {
        try {
            var nextPrintTime = startTime
            var i = 0
            kotlin.io.println("isActive $isActive")
            while (isActive) { // computation loop, just wastes CPU
                // print a message twice a second
                if (System.currentTimeMillis() >= nextPrintTime) {
                    println("job: I'm sleeping ${i++} ...")
                    nextPrintTime += 500L
                }
            }
            kotlin.io.println("isActive $isActive")
        } catch (e: Exception) {
            kotlin.io.println("Exception {$e}")
        }

    }
    delay(1300L) // delay a bit
    println("main: I'm tired of waiting!")
    job.cancelAndJoin() // cancels the job and waits for its completion
    println("main: Now I can quit.")
}
```
```
isActive true
job: I'm sleeping 0 ...
job: I'm sleeping 1 ...
job: I'm sleeping 2 ...
main: I'm tired of waiting!
isActive false
main: Now I can quit.
```
try-catch로 확인해보면, 이 방법은 exception을 던지지 않는 것을 알 수 있음.

### Closing resources with finally
코루틴이 종료 될때 resource는 어떻게 해제하는가?   
resource를 해체하는 것은 중요함! -> 메모리 낭비를 막기 위해
```kotlin
val job = launch {
    try {
        repeat(1000) { i ->
            println("job: I'm sleeping $i ...")
            delay(500L)
        }
    } finally {
        println("job: I'm running finally")
    }
}
delay(1300L) // delay a bit
println("main: I'm tired of waiting!")
job.cancelAndJoin() // cancels the job and waits for its completion
println("main: Now I can quit.")
```
```
job: I'm sleeping 0 ...
job: I'm sleeping 1 ...
job: I'm sleeping 2 ...
main: I'm tired of waiting!
job: I'm running finally
main: Now I can quit.
```
finally에서 resource가 해제됨을 알 수 있음.

### Run non-cancellable block
취소 불가능한 코드 블록의 실행 : 이미 CancellableException이 발생한 코루틴의 finally 블록에서
중단 함수를 호출하면 현재 코루틴은 이미 취소된 상태이므로 CancellationException이 발생하게 되는데
이러한 상황에 동기적으로 중단 함수를 호출해야한다면 withContext(NonCancellable)을 이용해 context를 전달하여
처리할 수 있음. -> rare한 케이스임 
```kotlin
val job = launch {
    try {
        repeat(1000) { i ->
            println("job: I'm sleeping $i ...")
            delay(500L)
        }
    } finally {
        withContext(NonCancellable) {
            println("job: I'm running finally")
            delay(1000L)
            println("job: And I've just delayed for 1 sec because I'm non-cancellable")
        }
    }
}
delay(1300L) // delay a bit
println("main: I'm tired of waiting!")
job.cancelAndJoin() // cancels the job and waits for its completion
println("main: Now I can quit.")
```
```
job: I'm sleeping 0 ...
job: I'm sleeping 1 ...
job: I'm sleeping 2 ...
main: I'm tired of waiting!
job: I'm running finally
main: Now I can quit.
```

### Timeout
어떤 코루틴의 수행시간이 너무 길어져 허용할 수 있는 시간을 넘어섰을 경우 코루틴의 실행을 취소해야하는데,
이떄 timeout을 지정해주어 해당 시간이 넘을 경우 취소하도록 구현할 수 있음.
```kotlin
withTimeout(1300L) {
    repeat(1000) { i ->
        println("I'm sleeping $i ...")
        delay(500L)
    }
}
```
```
I'm sleeping 0 ...
I'm sleeping 1 ...
I'm sleeping 2 ...
Exception in thread "main" kotlinx.coroutines.TimeoutCancellationException: Timed out waiting for 1300 ms
	at
```
Exception이 발생한 이유는 runBlocking에서 바로 실행했기 떄문임. 
이를 해결하기 위해서는 withTimeoutOrNull()을 사용.

```kotlin
val result = withTimeoutOrNull(1300L) {
    repeat(1000) { i ->
        println("I'm sleeping $i ...")
        delay(500L)
    }
    "Done" // will get cancelled before it produces this result
}
println("Result is $result")
```
```
I'm sleeping 0 ...
I'm sleeping 1 ...
I'm sleeping 2 ...
Result is null
```
결과 값이 null로 떨어지는 것을 볼 수 있음.

---

## Reference
- [코루틴 공식가이드](https://kotlinlang.org/docs/cancellation-and-timeouts.html)
