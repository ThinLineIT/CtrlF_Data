
# Observable 정리 1탄
### RxSwift 공식 문서를 읽고 정리한 글이다. by 이지원</br></br>
## You can answer
- Observable 개념
- Observable 생성 방법
- RxSwift Observable의 라이프 사이클
<br/>
<br/>
## 1. Observable 이란 ?

---
<br/>
모든 것은 .. sequence 다 ..
<br/><br/>
<img width="414" alt="_2021-05-04__2 25 39" src="https://user-images.githubusercontent.com/70083982/119272691-2506d400-bc42-11eb-95a9-fff78ad48db9.png">
<br/><br/>

> Observable 은 그냥 특별한 힘을 가진 시퀀스다!

여기서 말하는 특별한 힘에는 여러가지가 있는데 그 중 가장 중요한 건! **비동기**. 

- Observable 은 이벤트를 생성하는데, 이를 emitting 이라고 한다. 이벤트는 숫자나 instance 같은 값을 가지고 있을 수도 있고, 제스처를 인식할 수도 있다.
- observer 는 Observable 을 subscribe 하고, 그러면 observer 는 Observable 이 emitting 하는 시퀀스 / 이벤트에 반응하게 되는 것이다.

<br/><br/>
## 2. Observable 의 Life Cycle

---

<br/>
Observable 을 잘 이해하는데에는 Marble Diagram 만 한게 없다고 한다?

Observable 의 라이프 사이클 (= 이벤트)은 단순하다. 3가지!<br/><br/>

1. __next 이벤트. observable 이 emit 뿜뿜하는중__ <br/><br/>
<img width="430" alt="_2021-05-04__2 47 41" src="https://user-images.githubusercontent.com/70083982/119272836-f9d0b480-bc42-11eb-8fa6-9b43d5553b0b.png">

<br/>

2. __completed 이벤트. 마지막에 세로줄? 저 표시다 !__ <br/><br/>
<img width="448" alt="_2021-05-04__2 48 03" src="https://user-images.githubusercontent.com/70083982/119272944-8f6c4400-bc43-11eb-83d7-5e1e9c6c9e7a.png">

<br/>

3. __error 이벤트. 엑스 표시 !__ <br/><br/>
<img width="427" alt="_2021-05-04__2 48 51" src="https://user-images.githubusercontent.com/70083982/119272954-9a26d900-bc43-11eb-8a57-f55177c401cf.png">

<br/><br/>
이번엔 더 큰 marble diagram 을 보자.<br/><br/>
<img width="755" alt="_2021-05-04__2 36 28" src="https://user-images.githubusercontent.com/70083982/119272963-a6ab3180-bc43-11eb-9ce5-8e5b441b317a.png">
<br/>

**정리해보자면,**

- 시간은 좌에서 우로 흐른다
- 윗 부분의 도형들이 Observable 이 emitting 하는 아이템들이다
- 중간 박스는 transformation 을 뜻한다. 함수 개념 처럼 생각하면 됨!
- 아래 나온 도형들은 transformation 의 결과들. 얘네들도 Observable 이라는 사실. 뭐다? 모든 것은 .. 시퀀스다..
- 세로 줄 띡 그어놓았다는 것은 Observable이 completed (정상종료) 되었다는 뜻
- 엑스표시는 Observable 이 error (비정상종료) 났다는 뜻

<br/><br/>

## 3. Observable 생성하기

---
<br/>

크게 3가지가 있다! **of, just, from.** 
<br/><br/>

### 1. of

**.of 연산자는 주어진 값들의 타입추론을 통해 Observable** **sequence를 생성한다.**<br/><br/>

예시를 보자.

```swift
let observable2 = Observable.of(one, two, three) 
//one = 1, two = 2, three = 3
```

여기서 observable2 는 무슨 타입의 sequence가 될까 ?

<img width="576" alt="_2021-05-04__3 17 45" src="https://user-images.githubusercontent.com/70083982/119273009-e83bdc80-bc43-11eb-8fae-08208af5098d.png">

⇒ Observable<Int> 형!

정수 여러개가 들어갔으니 array 가 나오지 않을까 생각하겠지만 아니다!

```swift
let observable3 = Observable.of([one, two, three])
```

그럼 이거는 ?

⇒ Observable[Int] 형을 반환한다!

결론은 타입 추론을 통해 Observable Seqence를 반환한다는 것 ~~

<br/><br/>

### 2. just<br/><br/>

<img width="761" alt="_2021-05-04__3 10 28" src="https://user-images.githubusercontent.com/70083982/119273059-16b9b780-bc44-11eb-8781-36af42bd107d.png"><br/>

**Just 연산자는 아이템을 그 아이템을 emit 하는 Observable 로 변환한다.**

<br/>
from 과 비슷하다고 생각할 수 있지만, from 은 iterable 속에 들어가서 아이템들을 꺼내와 emit 하는 반면, Just 는 말그대로 just single 아이템을 날 것 그대로 반환한다! 

- Just 에 null 을 주면, null 을 emit 하는 Observable 을 리턴한다. empty Observable 을 반환한다고 생각하여 실수하는 경우가 많으니 주의. (Empty operator가 하는 일)
<br/><br/>

### 3. from<br/><br/>
<img width="755" alt="_2021-05-04__3 10 56" src="https://user-images.githubusercontent.com/70083982/119273104-4d8fcd80-bc44-11eb-8c58-613b7a5739d7.png">

**from 연산자는 일반적인 array 각각 요소들을 하나씩 방출한다.**

```swift
let observable4 = Observable.from([one, two, three])
```

어떻게 나올까?

⇒ 1  2  3 이 차례대로 나온다! 배열 요소들을 하나씩 떼어준다고 생각하면 쉽다.

- from 연산자는 ***오직 array 만*** 취한다.
