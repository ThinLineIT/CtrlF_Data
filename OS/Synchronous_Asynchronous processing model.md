# 동기(Asynchronous)와 비동기처리(Asynchronous processing model)

<!--Table of Contents-->
- 동기 처리 모델

- 비동기 처리 모델

- 두 모델의 장단점

- 블럭과 넌블록

<!-- 어떤 질문을 대답할 수 있어야 하는지-->
## You can answer
- 동기, 비동기 차이
- 블록, 넌블록 상태 예시

<!--Contents-->

---
## 동기 처리 모델(Synchronous processing model)
동기는 말 그대로 동시에 일어난다는 뜻으로 요청과 그 결과가 동시에 일어납니다. 바로 요청을 하면 시간이 얼마가 걸리던지 요청한 자리에서 결과가 주어져야 합니다.  
그렇기 때문에 동기식 처리 모델(Synchronous processing model)은 직렬적으로 태스크(task)를 수행하는데 이는 순차적으로 실행되며 어떤 작업이 수행 중이면 다음 작업은 대기하게 됩니다.  
예를 들어 서버에서 데이터를 가져와서 화면에 표시하는 작업을 수행할 때, 서버에 데이터를 요청하고 데이터가 응답될 때까지 이후 태스크들은 블로킹(blocking, 작업 중단)됩니다.

![image](https://user-images.githubusercontent.com/22022393/128608962-0ed8e3ec-b84c-447d-8b68-924ee6f08ac8.png)


## 비동기 처리 모델(Asynchronous processing model)
비동기는 동기와 반대로 동시에 일어나지 않는다를 의미합니다. 요청과 결과가 동시에 일어나지 않습니다.  
비동기식 처리 모델(Asynchronous processing model)은 병렬적으로 태스크를 수행하는데 이는 태스크가 종료되지 않은 상태라 하더라도 대기하지 않고 다음 태스크를 실행하게 됩니다.
예를 들어 서버에서 데이터를 가져와서 화면에 표시하는 태스크를 수행할 때, 서버에 데이터를 요청한 이후 서버로부터 데이터가 응답될 때까지 대기하지 않고(Non-Blocking) 즉시 다음 태스크를 수행하고 이후 서버로부터 데이터가 응답되면 이벤트가 발생하고 이벤트 핸들러가 데이터를 가지고 수행할 태스크를 계속해 수행합니다.

![image](https://user-images.githubusercontent.com/22022393/128609118-a2767ccf-5dc8-4983-924a-002c0c6afd1b.png)


## 두 모델의 장단점

### 동기 처리 모델(Synchronous processing model)

- 장점 : 설계가 매우 간단하고 직관적입니다.  
- 단점 : 요청에 따른 결과가 반환되기 전까지 아무것도 못하고 대기해야합니다.  

### 비동기 처리 모델(Asynchronous processing model)

- 장점 : 요청에 따른 결과가 반환되는 시간 동안 다른 작업을 수행할 수 있어 자원을 효율적으로 사용할 수 있습니다.  
- 단점 : 동기보다 설계가 복잡하고 논증적입니다.  
---
## Reference
- [공부해서 남 주자 - 동기와 비동기의 개념과 차이](https://private.tistory.com/24)
- [동기와 비동기 방식(Asynchronous processing model)](https://webclub.tistory.com/605)
- [글 쓰는 개발자의 (동기와 비동기에 대해서)](https://juyeop.tistory.com/22)
