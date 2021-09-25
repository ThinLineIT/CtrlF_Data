# Composing suspending functions (중단 함수들의 조합)

<!--Table of Contents-->

- Sequential by default
- Concurrent using async
- Async-style functions
- Structured concurrency with async

<!-- 어떤 질문을 대답할 수 있어야 하는지-->

## You can answer
- 코루틴에서 중단함수의 특징

<!--Contents-->
---
## Suspending Functions
### Sequential by default
원격 서비스 호출이나 계산과 같은 기능을 수행하는 두 가지 중단 함수들이 정의 되어 있다고 가정해보자!
```kotlin
val time = measureTimeMillis {
    val one = doSomethingUsefulOne()
    val two = doSomethingUsefulTwo()
    println("The answer is ${one + two}")
}
println("Completed in $time ms")

suspend fun doSomethingUsefulOne(): Int {
    delay(1000L) // pretend we are doing something useful here
    return 13
}

suspend fun doSomethingUsefulTwo(): Int {
    delay(1000L) // pretend we are doing something useful here, too
    return 29
}
```
```
doSomethingUsefulOne
doSomethingUsefulTwo
The answer is 42
Completed in 2028 ms
```

코루틴에서 일반코드와 같이 작성하게 되면 비동기로 호출할지라도 순차적으로 실행되는 것이 기본임.  
첫 번째 중단 함수의 결과를 사용해서 두 번째 함수의 호출 여부를 결정하거나 호출 방법을 결정하는 경우에 수행함

### Concurrent using async
두 중단 함수 사이에 연관이 없는 경우 두 작업을 동시에 수행하려면 ?
```kotlin
val time = measureTimeMillis {
    val one = async { doSomethingUsefulOne() }
    val two = async { doSomethingUsefulTwo() }
    println("The answer is ${one.await() + two.await()}")
}
println("Completed in $time ms")

suspend fun doSomethingUsefulOne(): Int {
    delay(1000L) // pretend we are doing something useful here
    return 13
}

suspend fun doSomethingUsefulTwo(): Int {
    delay(1000L) // pretend we are doing something useful here, too
    return 29
}
```
```
doSomethingUsefulOne
doSomethingUsefulTwo
The answer is 42
Completed in 1056 ms
```
순차적으로 실행되는 것이 기본이므로 이를 다시 concurrency하게 수행하려면 명시적으로 나타내어야함. → async { }

async는 launch와 개념적으로 비슷하지만 launch는 Job을 리턴하고 어떠한 결과값도 제공하지 않음.  
async는 Deferred( non-blocking future)를 리턴함.  
deferred value는 await()를 이용해 결과를 얻을 수 있음.


### Async-style functions

위에서 작성한 async { } 부분을 아예 한 함수로 묶어서 async-style의 함수를 만들어보겠다!
```kotlin
fun main() {
    val time = measureTimeMillis {
        // we can initiate async actions outside of a coroutine
        val one = somethingUsefulOneAsync()
        val two = somethingUsefulTwoAsync()
        // but waiting for a result must involve either suspending or blocking.
        // here we use `runBlocking { ... }` to block the main thread while waiting for the result
        runBlocking {
            println("The answer is ${one.await() + two.await()}")
        }
    }
    println("Completed in $time ms")
}


fun somethingUsefulOneAsync() = GlobalScope.async {
    doSomethingUsefulOne()
}

fun somethingUsefulTwoAsync() = GlobalScope.async {
    doSomethingUsefulTwo()
}

suspend fun doSomethingUsefulOne(): Int {
    delay(1000L) // pretend we are doing something useful here
    return 13
}

suspend fun doSomethingUsefulTwo(): Int {
    delay(1000L) // pretend we are doing something useful here, too
    return 29
}
```
```
The answer is 42
Completed in 1205 ms
```
```
비동기 기능이 있는 이 프로그래밍 스타일은 다른 프로그래밍 언어에서 인기 있는 스타일이기 때문에 여기서는 예시용으로만 제공됩니다. 
코틀린 코루틴과 함께 매번 Global Scope를 만들어서 각자의 코루틴에서 작동하게끔 사용하는 스타일을 사용하는 것은 아래에 설명된 이유 때문에 강력히 권장되지 않습니다.
```

일반 함수처럼 만들어서 어디서든 사용할 수 있도록 하겠다! 라는 생각에서 짤 수 있는 코드지만   
이렇게 하는 것이 위험한 이유는 다음의 코드를 실행시켜보면 알 수 있음.

```kotlin
fun main() {
    try {
        val time = measureTimeMillis {
            // we can initiate async actions outside of a coroutine
            val one = somethingUsefulOneAsync()
            val two = somethingUsefulTwoAsync()

            print("exception")
            throw Exception("exceptions")

            runBlocking {
                println("The answer is ${one.await() + two.await()}")
            }
        }
        println("Completed in $time ms")
    } catch (e: Exception) {
    }

    runBlocking {
        delay(100000)
    }
}
fun somethingUsefulOneAsync() = GlobalScope.async {
    println("start, somethingUsefulOneAsync")
    doSomethingUsefulOne()
    println("end, somethingUsefulOneAsync")
}

fun somethingUsefulTwoAsync() = GlobalScope.async {
    println("start, somethingUsefulTwoAsync")
    doSomethingUsefulTwo()
    println("end, somethingUsefulTwoAsync")
}

suspend fun doSomethingUsefulOne(): Int {
    delay(3000L) // pretend we are doing something useful here
    return 13
}

suspend fun doSomethingUsefulTwo(): Int {
    delay(3000L) // pretend we are doing something useful here, too
    return 29
}
```
```
exception
start, somethingUsefulTwoAsync
start, somethingUsefulOneAsync
end, somethingUsefulTwoAsync
end, somethingUsefulOneAsyn
```
예외가 발생했을 때 어떻게 처리하는가 ?  
-> GlobalScope에서 실행되고 있기 때문에 예외가 발생하여도 취소가 되지 않고  
그대로 실행됨.

이를 해결하기 위해 Structured Concurrency를 사용
### Structured concurrency with async
```kotlin
fun main() = runBlocking<Unit> {
    val time = measureTimeMillis {
        println("The answer is ${concurrentSum()}")
    }
    println("Completed in $time ms")
}

suspend fun concurrentSum(): Int = coroutineScope {
    val one = async { doSomethingUsefulOne() }
    val two = async { doSomethingUsefulTwo() }
    one.await() + two.await()
}

suspend fun doSomethingUsefulOne(): Int {
    delay(1000L) // pretend we are doing something useful here
    return 13
}

suspend fun doSomethingUsefulTwo(): Int {
    delay(1000L) // pretend we are doing something useful here, too
    return 29
}
```
```
The answer is 42
Completed in 1049 ms
```
이러한 방식을 사용한다면 예외가 발생할 경우에 해당 scope에서 시작된 모든 코루틴이 취소될 것임.
따라서 이 방식을 권장함!
---

## Reference
- [코루틴 공식가이드](https://kotlinlang.org/docs/composing-suspending-functions.html)
