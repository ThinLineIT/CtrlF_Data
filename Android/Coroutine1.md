# Coroutine - overview
<!--Table of Contents-->
- Coroutine 
    - What is a Coroutine?
    - Why we use Coroutine?
    - Coroutine 비동기 처리
  
<!-- 어떤 질문을 대답할 수 있어야 하는지-->
## You can answer
- 코루틴이란 무엇이고 왜 사용하는가 ?

<!--Contents-->
---
## Coroutine
### What is a Coroutine?
    비동기적으로 실행되는 코드를 간소화하기 위해 Android에서 사용할 수 있는
    동시 실행 설계 패턴 (= Concurrency design pattern)
    → 기본스레드를 block 하여 앱이 응답하지 않게 만들 수도 있는
    long-running  작업을 관리하는 데에 도움을 줌.
### Why we use Coroutine ?
    1)  Lightweight : suspension(코루틴을 실행 중인 스레드를 Block하지 않는 기능) 을 지원하므로 하나의 스레드에서 많은 코루틴을 실행할 수 있음. 
    이는 쓰레드를 Block하며 Context switch 하는 것 보다 효율적임
    → 왜 가볍다고 할까? 어떻게 동작할까? -> 아래 내용과 coroutine basic.md 참고!

    2) 메모리 누수 감소  : structured concurrency(구조화된 동시 실행)를 사용하여 범위 내에서 작업을 실행함
    
    3) cancellation  : 실행 중인 코루틴 계층 구조를 통해 자동으로 cancellation이 전달됨
    코루틴에서 실행되는 모든 suspending 함수들은 취소 요청에 응답가능하도록 구현되어야함.
    kotlinx.coroutines 라이브러리의 모든 suspending함수는 이런 취소요청에 대응하도록 구현되어있음.

    * Structured concurrency 
      코루틴은 구조화된 동시성 원칙을 따름. 
      즉, 수명을 제한하는 특정 코루틴 범위에서만 새 코루틴을 시작 할 수 있음.
      이러한 구조화된 동시성은 데이터 손실 및 누출을 방지하고, 모든 subroutine이 완료될 때까지
      외부 범위를 완료할 수 없음. 또, 코드의 오류가 올바르게 보고되고, 손실되지 않는 것을 보장함.

### Coroutine 비동기 처리 
* 코루틴은 scope를 통해 제어범위 및 실행범위를 지정할 수 있음
    1) GlobalScope : 프로그램 어디서나 제어 동작이 가능한 기본 범위
    2) CoroutineScope : 특정한 목적의 Dispatcher를 지정하여 제어 및 동작이 가능한 범위
* Dispatcher  
  Dispatcher는 코루틴의 실행을 특정 스레드로 한정짓거나, 특정 스레드 풀로 전달하거나, 스레드의 제한 없이 실행되도록 할 수 있음.
    1) Dispatcher.Default : 기본적인 백그라운드 동작
    2) Dispatcher.IO :  I/O에 최적화 된 동작
    3) Dispatcher.Main : UI 스레드 동작  
    ![imgs](./img/Coroutinenonblocking.jpg)
* coroutine builder 
    1) launch{} : 반환값이 없는 Job 객체
    2) async{} : 반환값이 있는 Deffered 객체 반환
    ```kotlin
    val scope = CoroutineScope(Dispatcher.Default)
    val coroutineA = scope.lauch{}
    val coroutineB = scope.async{}
    ```
* ex) 
    ```kotlin
    import kotlinx.coroutines.*
    
    fun main() {
        runBlocking {
         val a = launch {
         for(i in 1..5)
        {
        println(i)
        delay(1000L)
        }
    }
    
        val b = async {
            "async 종료"
        }
    
        println("async 대기")
        println(b.await())
    
        println("launch 대기")
        a.join()
        println("launch 종료")
        }
    }
    ```
    -> 결과

        async 대기
        1  
        async 종료
        launch 대기
        2
        3
        4
        5
        launch 종료
 
* 코루틴이 Lightweight Thread인 이유? 
![images](./img/CoroutinelightWeight.png)
코루틴은 task의 단위가 coroutine object로 task들은 각 coroutine에 할당되며 JVM의 Heap 영역을 차지함.  
  코틀린의 코루틴을 Stackless Coroutine이라고도 하는데, 스택이 없고 특정 스레드에 종속되는 것도 아니기 때문임.  
  따라서 프로세서에서 Context Switching이 필요하지 않기때문에 수천개의 thread 생성보다 coroutine을 생성하는 것이  
  더 빠르고, 자원을 적게 사용함.  
여러 개의 코루틴을 하나의 스레드에서 실행할 수 있고 스레드 내에서 실행되는 코루틴은  
중단지점에 도달하면 다른 코루틴을 선택할 수 있음.  
  -> 스레드와 메모리 사용량이 줄어 많은 동시성 작업을 수행할 수 있음.  
  *예시는 coroutine basic.md 를 참고해주세요:)*
---
## Reference
- [코루틴 공식 문서](https://kotlinlang.org/docs/async-programming.html)
