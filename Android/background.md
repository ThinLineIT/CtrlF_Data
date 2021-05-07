# Handler & Looper 
<!--Table of Contents-->
- 백그라운드 스레드
- Handler 
- Looper

<!-- 어떤 질문을 대답할 수 있어야 하는지-->
## You can answer
- Handler의 역할 
- Looper의 역할 

<!--Contents-->

---
### Background Thread 에서 작업 실행
![handler](https://blog.kakaocdn.net/dn/b1lTv3/btqzI39CSrz/WBVGD1tRhAhkSm5ThjLq7k/img.png)
* 안드로이드 시스템에서 UI 관련 작업은 Main Thread에서 처리해야한다.   
  그 외의 네트워크, 데이터베이스 작업들은 별도의 Thread에서 수행한다.  
  이 때, 서로 다른 Thread 간의 통신 작업을 할 때 필요한 것이 Handler와 Looper 다.  


## Handler
- Handler는 Main Thread가 가진 Looper로부터 받은 Message나 Runnable을 처리하는 스레드 간의 통신장치이다.
- Handler를 사용하여 다른 Thread에서 실행될 작업을 큐에 추가할 수 있다.
- 새로운 Handler를 만들 경우 그 Handler를 만든 Thread에 연결된다. 
- 다른 Thread에서 특정 handler를 통해 message를 전달하는 경우에는  
  message를 전달한 handler와 연결된 Thread의 MessageQueue에 들어가 동작한다.

### 예시
```kotlin
var mHandler = Handler(Handler.Callback{
    msg->
    //doing
    false 
})

mHandler.sendMessage(msg)

var mRunnable = Runnable{
    //doing
}

var mHandler2 = Handler()
mHandler2.post(mRunnable)
```
-> message의 경우 Handler에서 MessageQueue에 전달 후 전달한 Handler의 handleMessage에서 받아서 동작한다. 
Runnable의 경우도 handler를 통해 messageQueue에 전달 후 차례가 되면 Runnable의 run()에서 동작한다.


## Looper와 MessageQueue
- Looper는 Thread에서 무한 루프로 돌며 자신이 속한 Thread의 Messagequeue에 message나 runnable이 들어오면 
선입선출로 Handler에 전달하는 역할을 한다.
- 새로 생성한 Thread는 Looper를 생성해야한다.
Main Thread는 자동으로 Looper가 생성되지만, 새로 생성한 Thread의 경우 Looper가 없고 run만 존재하므로,
run의 동작이 끝날 경우 Thread는 종료가 된다.  
 
- Looper의 종료는 quit()나 quitSafely()를 통해 종료할 수 있다.

---
## Reference
- [Example Site1](www.google.com)
- [Example Site2](www.google.com)

