# MVC vs MVP vs MVVM
<!--Table of Contents-->
- MVC,MVP,MVVM 이란?
- MVC, MVP, MVVM의 차이점
- 결론

<!-- 어떤 질문을 대답할 수 있어야 하는지-->
## You can answer
- MVC, MVP, MVVM 의 차이점이 무엇인가요?

<!--Contents-->
---
## MVC,MVP,MVVM 이란?
- 정의
  * Android Architecture Pattern 들로
  MVC 는 Model, View, Controller로 구성, MVP 는 Model과 View과 Presenter로 구성, MVVM은 Model과 View, ViewModel로 구성되어 있다.

- MVC
  * MVC는 안드로이드와 관계없이 프로그래밍 시 가장 널리 사용되는 구조 중 하나이다.
  * 사용자의 입력은 모두 Controller에서 발생하고, 관리하게 되는 구조이다.

- MVP
  * MVC와는 다르게 UI(View)와 비즈니스 로직(Model)을 분리되어 있다.
  * Controller와 다르게 Presenter는 View의 인터페이스를 통해 처리할 내용을 전달한다.

- MVVM
  * 하나의 소프트웨어를 최대한 기능적으로 작은 단위로 나눠 테스트가 쉽고 큰 프로젝트도 상대적으로 관리하기가 좋은 구조이다.
  * 모든 입력은 View로 전달되며, ViewModel은 입력에 해당하는 로직을 처리하여 View에 데이터를 전달한다.

## MVC, MVP, MVVM의 차이점
||MVC|MVP|MVVM|
|---|---|---|---|
|입력|Controller에서 처리|View에서 Presenter로 처리|ViewModle에서 처리|
|구조|Controller가 Model을 이용해 데이터를 처리하고, 어떤 View에게 해당 데이터를 적용할 지 결정한다.|Presenter가 Model을 이용해 데이터를 처리하고, View에게 데이터를 interface를 통해 전달한다.|ViewModel이 Model을 이용해 데이터를 처리하고 관리하면, View에서 해당 데이터를 관찰하다 업데이트한다.|
|장점|구현하기가 가장 쉽고 단순하여 개발기간이 짧아진다.|MVC의 단점인 Model과 View 사이의 의존성이 없다.|Model과 View 사이, ViewModel과 View 사이의 의존성이 없고, 중복되는 코드를 모듈화 할 수 있다.|
|단점|Model과 View 사이에 의존성이 발생하여 앱 자체가 커지거나 로직이 복잡해질수록 유지보수가 힘들어진다. |View와 Presenter 사이의 의존성이 존재하고 1:1관계이기 때문에 View가 늘어날 때 마다 추가되어야하는 인터페이스와 클래스가 많아진다.|ViewModel의 초기 설계가 쉽지 않고, View에 대한 처리 내용이 복잡해지면 ViewModel도 거대해진다.|

## 결론
1) MVC, MVP, MVVM 각각 장단점이 있기 때문에 특정한 패턴이 더 좋다고 할 수 없다.
2) 하지만 MVVM 패턴이 현 개발 환경에서 가장 적합하다.
  * Model과 View의 동기화 보장
  * 중복 코드 최소화
  * 다른 라이브러리와의 호환

---
## Reference
- [안드로이드 아키텍처 패턴 - MVC가 뭘까?](https://velog.io/@jojo_devstory/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-%EC%95%84%ED%82%A4%ED%85%8D%EC%B3%90-%ED%8C%A8%ED%84%B4-MVC%EA%B0%80-%EB%AD%98%EA%B9%8C)
- [안드로이드 아키텍처 패턴 - MVP가 뭘까?](https://velog.io/@jojo_devstory/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98-%ED%8C%A8%ED%84%B4-MVP%EA%B0%80-%EB%AD%98%EA%B9%8C)
- [안드로이드 아키텍처 패턴 - MVVM가 뭘까?](https://velog.io/@jojo_devstory/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98-%ED%8C%A8%ED%84%B4-MVVM%EC%9D%B4-%EB%AD%98%EA%B9%8C)
