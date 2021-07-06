# Asynchronous programming (비동기 프로그래밍 기술)
<!--Table of Contents-->
- Asynchronous Programming
    - Threading
    - Callbacks
    - Reactive Extensions
    - Coroutines

<!-- 어떤 질문을 대답할 수 있어야 하는지-->
## You can answer
- 비동기 프로그래밍에 어떤 것들이 있는지 

<!--Contents-->

---
## Asynchronous programming
### 1. Threading
스레드는 가장 잘 알려진 비동기 방법
```kotlin
fun postItem(item: Item) {
    val token = preparePost()
    val post = submitPost(token, item)
    processPost(post)
}

fun preparePost(): Token {
    // makes a request and consequently blocks the main thread
    return token
}
```
preparePost가 long-running 프로세스이므로 결과적으로 ui를 차단한다고 가정을 한다면,  
UI가 차단되는 것을 피하기 위해서는 별도의 thread로 실행하는 것.  
흔한 기술이지만 , 몇 가지 단점이 존재함.

- 비용이 많이 드는 context switching 필요로 함
- 실행 될 수 있는 thread가 한정적이라 서버 측에서는 병목현상이 일어날 수 있음
- thread가 항상 사용 가능한 것이 아님 ( js와 같은 일부 플랫폼은 지원하지 않음).
### 2. Callbacks
```kotlin
fun postItem(item: Item) {
    preparePostAsync { token ->
        submitPostAsync(token, item) { post ->
            processPost(post)
        }
    }
}

fun preparePostAsync(callback: (Token) -> Unit) {
    // make request and return immediately
    // arrange callback to be invoked later
}
```
Callback을 사용하면 한 함수를 다른 함수에 매개변수로 전달해서 프로세스가 완료되면  
이 함수가 호출되도록 할 수 있음.   
이는 더 좋은 솔루션처럼 느껴지지만 몇 가지 문제가 있음.

- 중첩된 콜백이 어려움.  
  일반적으로 콜백으로 사용되는 함수는 자체 콜백이 필요한 경우가 많은데  
  이렇게 되면 중첩된 콜백이 발생하여 코드를 이해할 수 없게 됨
- 오류 처리가 복잡함.
### 3. Reactive Extensions
리액티브 프로그래밍은 비동기에서 처리하기 힘든 에러 처리나 데이터 가공을 쉽게 하도록 도와줌.  
이벤트가 콜백이 아닌 데이터의 모음으로 모델링 되기 때문.  
데이터를 스트림(무한의 데이터 양)으로 생각하고 이런 스트림을 관찰 가능한 스트림으로 이동하도록 하는,  
우리가 데이터에 대해 작동할 수 있는 일련의 확장을 가진 관찰자 패턴임.
![images](https://www.seekpng.com/png/detail/117-1173481_images-filter-rxjava-map.png)
데이터의 흐름을 먼저 정의하고 데이터가 변경되었을 때 업데이트 해서 push하는 방식

### 4. Coroutines
코틀린의 비동기 코드 작업 방식.
일시 중단 가능한 계산의 개념. (suspendable computation)
```kotlin
fun postItem(item: Item) {
    launch {
        val token = preparePost()
        val post = submitPost(token, item)
        processPost(post)
    }
}

suspend fun preparePost(): Token {
    // makes a request and suspends the coroutine
    return suspendCoroutine { /* ... */ }
}
```
이 코드는 main thraed를 차단하지 않고 long running 작업을 시행함.

---
## Reference
- [코루틴 공식 가이드](https://kotlinlang.org/docs/async-programming.html)

