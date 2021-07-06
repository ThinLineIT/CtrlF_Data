# Coroutine Basic

<!--Table of Contents-->

- first Coroutine
- Refactoring Extract function
- Scope builder
- Scope builder and concurrency
- An explicit job
- Coroutines are light-weight
- Structured Concurrency

<!-- 어떤 질문을 대답할 수 있어야 하는지-->

## You can answer

- 코루틴의 기본적인 생성법
- 코루틴의 기본적인 동작
- 코루틴의 특징
- 코루틴 Structured Concurrency 란?

<!--Contents-->

---

## Coroutine Basic

코루틴은 중단 가능한 계산의 개념 (suspendable computation)

- 개념적으로 스레드와 비슷하지만 코루틴은 특정 스레드에 바인딩 되지 않고  
  한 스레드에서 실행을 중단했다가 다른 스레드에서 재개할 수 있음

### first Coroutine

```kotlin
fun main() = runBlocking {
    launch {
        delay(1000L)
        println("World!")
    }
    println("Hello")
} 
```

```kotlin
Hello
World!
```

launch : coroutine Builder  
→ 코드의 나머지 부분과 동시에 새로운 코루틴을 시작하고, 독립적으로 작동함.  
→ hello가 먼저 출력되는 이유

runBlocking : coroutine Builder  
→ 해당 스레드를 호출하는 동안 runBlocking{ } 내부의 모든 코루틴이 실행이 완료될 때까지 차단됨을 의미  
→ 코루틴 안에서 runBlocking{ } 사용은 권장되지 않음  
일반적인 함수 코드 블록에서 중단 함수를 호출할 수 있도록 하기 위해 존재하는 장치.

delay : suspending function  
→ 지정한 시간 동안 코루틴을 중단.  
→ 코루틴 안에서만 호출 가능

### Refactoring Extract function

launch{} 내부의 코드를 별도의 함수로 변환해보자.  
suspend modifier로 새로운 함수 → *suspending function*

```kotlin
fun main() = runBlocking { // this: CoroutineScope
    launch { doWorld() }
    println("Hello")
}

// suspending function
suspend fun doWorld() {
    delay(1000L)
    println("World!")
}
```

suspending 함수는 일반적인 함수와 같이 코루틴 내에서만 사용가능함.  
그러나 추가적인 기능으로, 다른 일시 중단 함수를 사용하여 코루틴 실행을 중단 시킬 수 있음. ex) dealy()

### Scope builder

사용자 정의 스코프가 필요한 경우가 있다면 coroutineScope 빌더를 사용할 수 있음.  
runBlocking과 scope builder는 부모 코루틴과 자식코루틴 모두가 완료하기를  
기다리기때문에 비슷해보이지만 주된 차이점이 있음.  
runBlocking 은 스레드를 차단 하지만  
coroutineScope는 자식들의 종료를 기다리는 동안 현재 스레드를 차단하지 않고 일시중단 한다는 점.

```kotlin
fun main() = runBlocking {
    launch {
        delay(200L)
        println("runBlocking Task")
    }

    coroutineScope {  // this: CoroutineScope
        launch {
            delay(500L)
            println("Launch Task")
        }
        delay(100L)
        println("Coroutine scope Task")
    }
    println("Coroutine scope is over")
}
```
```
coroutine scope Task  
runBlocking Task
Launch Task
Coroutine scope is over
```

### Scope builder and concurrency

coroutineScope builder는 모든 suspendable function에서 여러 동시 작업을 수행할 수 있음.

다음 예시에서 doWorld() 에서 두 개의 동시 루틴을 실행해보자!
```kotlin
// Sequentially executes doWorld followed by "Done"
fun main() = runBlocking {
        doWorld()
        println("Done")
    }

// Concurrently executes both sections
suspend fun doWorld() = coroutineScope { // this: CoroutineScope
    launch {
        delay(2000L)
        println("World 2")
    }
    launch {
        delay(1000L)
        println("World 1")
    }
    println("Hello")
}
```
```
Hello
World 1
World 2
Done
```
두 개의 launch{ } 내부의 코드들이 동시에 실행되서 world1 먼저 출력되고 1초 뒤에 world2 출력.  
doWorld() 코루틴 스코프는 내부의 코드들이 모두 완료된 뒤에 return 되고  "Done"을 출력하도록 함.

### An explicit job

launch coroutine builder는 시작된 코루틴에 대한 핸들로서 Job 개체가 완료되기를 명시적으로 기다리는 데 사용할 수 있음. 예를 들어 자식 코루틴이 완료될 때까지 기다린 다음 "Done" 출력 할
수 있음.
```kotlin
val job = launch {
    delay(1000L)
    println("World!")
}
println("Hello")
job.join() // wait until child coroutine completes
println("Done")
```
```kotlin
Hello
World!
Done
```

### Coroutines are light-weight
다음을 실행해보자!
```kotlin
//sampleStart
fun main() = runBlocking {
    repeat(100_000) { // launch a lot of coroutines
        launch {
            delay(5000L)
            print(".")
        }
    }
}
//sampleEnd
```
실행해보면 빠른시간 내에 .이 출력되는 것을 볼 수 있음. 
위의 코드를 아래와 같이 Thread로 실행시켜본다면 더 많은 시간이 걸린다는 것을 알 수 있음.
```kotlin
fun main() = runBlocking {
    repeat(100_000) { // launch a lot of coroutines
        thread {
            Thread.sleep(5000L)
            print(".")
        }
    }
}
```

### Structured Concurrency
```kotlin
fun main() = runBlocking {
    val job = GlobalScope.launch {
        delay(1000L)
        println("World!")
    }
    val job1 = GlobalScope.launch {
        delay(1000L)
        println("World!")
    }
    println("Hello")
    job.join()
    job1.join()
} 
```
위의 코드는 job object가 부모 코루틴스코프와 다르게 
GlobalScope에서 launch 되고 있으므로 join() 함수를 이용하여 
결과를 출력할 수 있음.   
job 오브젝트가 여러 개 필요하다면 매번 join()을 추가해주어야하므로 
이를 structured concurrency 로 코드를 바꿔보면 다음과 같음. 
```kotlin
fun main() = runBlocking {
    launch {
        delay(1000L)
        println("World!")
    }
    launch {
        delay(1000L)
        println("World!")
    }
    println("Hello")
} 
```
runBlocking의 코루틴 스코프의 자식 코루틴 스코프 내에서 launch{ }가 실행 될 수 있도록
한다면 join()을 사용하지 않아도 World!가 찍히게 됨.

// TODO : globalScope에서 join을 사용하는 경우 ? 
---
## Reference
- [코루틴 공식 가이드](https://kotlinlang.org/docs/coroutines-basics.html)
