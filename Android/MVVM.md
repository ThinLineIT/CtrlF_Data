# MVVM
<!--Table of Contents-->
- MVVM 란?
- MVVM 처리과정
- MVVM 특징

<!-- 어떤 질문을 대답할 수 있어야 하는지-->
## You can answer
- MVVM 란 무엇인가요 ?

<!--Contents-->

---
## MVVM 란?
- 정의
  * Android Architecture Pattern 중 하나로 Model, View, ViewModel 세 가지의 구성요소로 나뉜다.

- Model
  * 어플리케이션에서 사용되는 데이터와 그 데이터를 처리하는 비즈니스 로직을 갖고 있는 부분이다.
  * View나 ViewModel에 독립적이기 때문에 재사용이 가능하다.

- View
  * 사용자에게 보여지는 UI부분으로 안드로이드에서는 Activity나 Fragment에 해당한다.
  * ViewModel을 관찰하고 있다가 데이터가 전달되면 사용자에게 보여준다.

- ViewModel
  * Model과 View 사이의 매개체라는 점에서 MVP의 Presenter와 유사하지만 MVP와 다르게 View와 ViewModel은 1:n의 관계를 가질 수 있으며, 여러개의 Fragment가 하나의 ViewModel을 가질 수 있다.
  * Model을 이용해 View에게 전달하기 좋은 데이터를 가공한다.

## MVVM 처리과정
  ![MVVMProcess](./img/MVVMProcess.PNG)
  ### 처리 순서
  1) View로 사용자의 입력이 들어온다.
  2) View는 Command 패턴으로 View Model에 이벤트를 전달한다.
  3) View Model은 필요한 데이터를 Model에 요청하고, Model은 요청받은 데이터를 View Model에게 응답한다.
  4) View Model은 응답받은 데이터를 가공하여 저장하고, View는 View Model과 Data Binding하여 화면을 나타낸다.


## MVVM 특징
  - 장점
    * Model과 View 사이, ViewModel과 View 사이의 의존성이 없으므로 유닛테스트가 더 쉬워진다.
    * 각각의 부분은 독립적이기 때문에 중복되는 코드를 모듈화 할 수 있다.


  - 단점
    * ViewModel의 초기 설계가 쉽지 않다.
    * View에 대한 처리 내용이 복잡해질수록 ViewModel도 거대해진다.

---
## Reference
- [안드로이드 아키텍처 패턴 - MVVM가 뭘까?](https://velog.io/@jojo_devstory/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98-%ED%8C%A8%ED%84%B4-MVVM%EC%9D%B4-%EB%AD%98%EA%B9%8C)
