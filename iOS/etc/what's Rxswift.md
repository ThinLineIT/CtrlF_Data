
# RxSwift 란?
### RxSwift 공식 문서를 읽고 정리한 글이다. by 이지원</br></br>
## You can answer
- RxSwift 가 뭔가요?
- 비동기 프로그래밍이란?
- RxSwift 사용의 장점?

<br/>

## 용어의 어원
- Rx is a generic abstraction of computation expressed through Observable<Element> interface, which lets you broadcast and subscribe to values and other events from an Observable stream.
- RxSwift는 Reactive extension + Swift의 합성어로, 비동기 프로그래밍을 관찰 가능한 흐름으로 지원해주는 API 이다.
<br/>
<br/>

## 비동기란
    비동기는 Asynchronous, 사전적으로 '동시에 발생하지 않는'이라는 뜻을 갖고 있다. 즉, 한 가지 일을 처리하는 동시에 다른 일도 함께 처리하는 것을 의미한다.
<br/>
- 예를 들어보자.
1. 웹 url을 통해 이미지를 다운로드 하고 불러오는 작업
2. 시간 초를 세는 작업
두 가지의 작업을 동시에 진행할 때, 동기로 작업을 하게 되면 이미지를 불러오는 작업을 하는 동안 시간 초를 세는 작업은 일시적으로 멈추게 된다.

하지만, 비동기로 작업을 하게 되면 이미지를 불러오는 동시에, 시간 초를 세는 작업도 함께 진행할 수 있다.
<br/><br/>
- 이외에도, 비동기 프로그래밍을 할 수 있는 일의 예를 들면,
<br/>

1. 버튼을 눌렀을 때의 이벤트 반응
2. 텍스트필드에 포커스가 잡힌 경우
3. 인터넷에서 크기가 큰 이미지 파일을 받는 경우
4. 디스크에 데이터를 저장하는 경우
5. 오디오를 실행하는 경우
이런 작업들이 있다.

<br/>
 - RxSwift를 배우기 전에도 UIKit을 통해 비동기를 경험했던 순간들이 있다. 예를 들면

NotificationCenter : 백그라운드 진입 후 몇초 있다가 메시지 알림
The delegate pattern : tableView의 didSelectRowAt과 같은 메소드 closure
이런 작업들이 있고, RxSwift를 사용하지 않아도 비동기 처리를 할 수 있는 방법은 많다.
<br/><br/>
## 근데 굳이 RxSwift를 사용하는 이유는 뭘까?

<br/>

### 코드의 일관성
- 우선 Rxswift를 사용하면 일관성 있는 하나의 비동기 코드로 비동기 처리가 가능하다.
-  실제 작업에서는 여러 쓰레드를 넘나 들고 Completion 메소드를 작성하며 처리 해야하기 때문에 가독성도 좋지 않고 골치 아픈 경우가 많지만, RxSwift는 그럴수록 Observable이라는 래퍼런스 타입 하나만으로 간결하고 깔끔하게 가독성 좋은 모습으로 처리할 수 있다.

<br/>

### 아키텍처의 확장
- MVVM 아키텍처는 데이터 바인딩을 제공하는 플랫폼에서 만들어진 이벤트 중심 프로그램을 위해 특별히 개발되었기 때문에, RxSwift와 함께 사용했을 때 아주 간편하게 MVVM 디자인 패턴을 구현할 수 있다.
- 또한 Rx로 설계가 된 아키텍처 영역(ReactorKit, RxFlow 등)과 서비스 영역(MoyaSugar, RxBus, RxStarscream 등)의 기능도 활용할 수 있다.

<br/><br/>

## RxSwift의 세 가지 구성요소
<br/>

1. Observables
2. operators
3. schedulers

