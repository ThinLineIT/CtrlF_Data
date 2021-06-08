
# Observable 정리 2탄

### RxSwift 공식 문서를 읽고 정리한 글이다. by 이지원</br></br>



## You can answer
- Observable 구독?
- Observable 생성 연산자들
- Observable의 dispose와 종료에 대해서 설명해보세요.
- Single, maybe, Completable 은 어떤 상황에서 어떻게 사용되나?
- RxSwift Observable 사용의 전체적인 흐름, 구조
<br/>
<br/>


## 4. Observable 구독하기

---
<br/>
말그대로 구독 ! 

NotificationCenter 를 써본 경험이 있다면 addObserver() 를 subscribe() 로만 바꾸면 된다! 

NotificationCenter의 차이점은, 전자는 .default 로 singleton instance 를 사용하지만 Rx는 아니다.

또한 가장 중요한 점은, **observable 은 구독자가 없으면 어떤 이벤트를 내보내지 않는다. 그냥 일을 안한다!**<br/><br/>

```swift
example(of: "subscribe") {
     let one = 1
     let two = 2
     let three = 3
     
     let observable = Observable.of(one, two, three)
     observable.subscribe({ (event) in
    	 print(event)
 	})
 	
 	/* Prints:
 	 next(1)
 	 next(2)
 	 next(3)
 	 completed
 	*/
 }
```

위 코드를 보면, observable 을 subscribe 하고, 이벤트를 출력한다! 

보면 next(...) 이벤트를 출력하다가 마지막에 completed 를 출력하는 하는 것을 볼 수 있다.

저 안에 있는 숫자를 빼내오기 위한 축약형이 있다. 바로 **.subscribe(onNext:)**<br/><br/>

```swift
observable.subscribe(onNext: { (element) in
 	print(element)
 })
/*
  1
  2
  3
 */
```

onNext 만 붙여주면 뚝딱 !<br/><br/><br/>

## 5. 다양한 Observable 연산자들

---
<br/>

### 1. **.empty()**

앞에서는 하나 또는 여러개의 요소를 가진 observable 를 생성하는 법을 알았다. 그렇다면 아무 요소도 없는 observable 은 ??  .empty() 는 아무 요소도 갖지 않는 observable sequence 를 생성한다. 얘는 completed 이벤트만 emit 한다!<br/><br/>

예시를 보면

```swift
example(of: "empty") {
     let observable = Observable<Void>.empty()
     
     observable.subscribe(
         
         // 1
         onNext: { (element) in
             print(element)
     },
         
         // 2
         onCompleted: {
             print("Completed")
     }
     )
 }
 
 /* Prints:
  Completed
 */
```

아무것도 없으므로 Completed 만 출력하고 끝이난다.<br/><br/>

그렇다면 이 observable 의 용도는 무엇이냐 ??

- 즉시 종료할 수 있는 Observable을 리턴하고 싶을 때
- 의도적으로 0개의 값을 가지는 Observable을 리턴하고 싶을 때

<br/><br/>

### 2. .never()

```swift
example(of: "never") {
     let observable = Observable<Any>.never()
     
     observable
         .subscribe(
             onNext: { (element) in
                 print(element)
         },
             onCompleted: {
                 print("Completed")
         }
     )
 }
```

- 아무것도 출력되지 않는다. Completed 도 안나온다.
- empty 는 item 을 emit하지 않으나 종료하지만, never 는 종료하지 않는다! 주로 production에 쓰이지 않고 테스팅에 쓰인다고 한다.

<br/><br/>


<br/><br/>

### 3. .range()

지금까지 우리는 specific 한 요소나 값을 가지는 observable 들만 봤다.

하지만, 특정 range 의 값들을 가지는 observable 을 생성하는 것이 가능하다~!<br/><br/>

```swift
example(of: "range") {
     
     //1
     let observable = Observable<Int>.range(start: 1, count: 10)
     
     observable
         .subscribe(onNext: { (i) in
             
             //2
             let n = Double(i)
             let mul = n * n
             print(mul)
         })
 }
/*
1
4
9
16
25
36
49
..
..
*/
```

이렇게 되면 range 연산자를 통해 1 ~ 10 만큼의 크기를 갖는 Observable 을 생성하게 되고, 

각각의 mul 값을 계산하고 출력하게 된다.


<br/><br/>

## 6. Disposing 과 terminating

---

정리해보자.

- observable은 subscription 을 받을 때까지 아무것도 안한다고 했다!
- 그렇다면 observable 이 종료될 때까지 새로운 이벤트를 emit 하게 하는, (= 동작하게 하는) trigger 는 subscription 이라는 의미!
- 그렇다면, 구독을 취소하면 observable 은 종료된다.
<br/><br/>

### 1. **.dispose()**
<br/>

예시를 들어보자.

```swift
example(of: "dispose") {
  // 1
  let observable = Observable.of("A", "B", "C")
// 2
  let subscription = observable.subscribe { event in
    // 3
    print(event)
  }
}

subscription.dispose()
```

1. strings 에 대한 observable 를 만든다.
2. observable 을 구독하고, 이번엔 리턴되는 Disposable을 subscription 이라는 local constant 로 저장한다.  **(이 예제에서 우리는 subscribe 하면 리턴되는 값이 Disposable 이라는 것을 알 수 있다! )** 
3. emitted event 를 출력한다. 

- 지금 observable 안에는 요소가 3개만 있으므로 dispose() 를 호출하지 않아도 Completed가 프린트 되지만, 요소가 무한히 있다면 dispose() 를 호출해야 Completed 가 프린트 된다.<br/><br/>

여기서 간단하게, 구독을 취소하려면

```swift
subscription.dispose()
```

하면 된다! observable 은 이벤트 emit 을 스탑하게 된다!
<br/><br/><br/><br/>

### 2. disposeBag()

- 위의 예시처럼 각각의 subscription 을 독립적으로 관리하는 건 힘드니, Rx 는 DisposeBag type을 제공한다.
- disposed(by:) 메소드를 호출하면 dispose bag 에 disposables 를 담을 수 있다!
- bag 에 담아서 한꺼번에 처리할 수 있게 되는 것.<br/><br/>

```swift
example(of: "DisposeBag") {
  // 1
  let disposeBag = DisposeBag()

// 2
  Observable.of("A", "B", "C")
    .subscribe { print($0) } // 3 
    .disposed(by: disposeBag) // 4
}
```

1. dispose bag 을 만든다. 
2. observable 을 만든다. 
3. observable 를 구독하고 print한다.
4. subscribe 로 부터 리턴된 Disposable 를 bag 에 담는다. (disposed by 메소드 이용)

<br/><br/>

    이 패턴이 가장 자주 이용하게 될 패턴 :
    observable 을 생성하고, 구독하고, 바로 subscription 을 dispose bag 에 넣는 패턴

<br/><br/>

**Why bother ? 너무 귀찮은디요 ?**

    메모리 leak 때문이다!


<br/><br/>

### 3. .create(:)


앞서서, 우리는 next 이벤트로 observables 을 만들었다. ( .of, .just ..)

create 연산자로도 observables 를 만들 수 있다!<br/>

```swift
example(of: "create") {
  let disposeBag = DisposeBag()
  Observable<String>.create { observer in
  }
}
```

- create 연산자는 subscribe 라는 변수 하나만을 받는다. subscribe 의 역할은 observable 의 subscribe 를 구현하는 것!
- 다시 말해서, subscribers 에게 emit 될 모든 이벤트를 정의하는 것.<br/><br/>

create 를 눌러보면!

<img width="583" alt="_2021-05-04__5 45 56" src="https://user-images.githubusercontent.com/70083982/119273354-79f81980-bc45-11eb-828e-453ae13bd21f.png">

- subscribe 변수는  AnyObserver 를 받아 Disposable 를 리턴하는 escaping closure 다.<br/><br/>

예시를 보자. 

```swift
Observable<String>.create { observer in
  // 1
  observer.onNext("1")
  // 2
  observer.onCompleted()
// 3
  observer.onNext("?")
// 4
  return Disposables.create()
}
```

1. .next 이벤트를 Observer에 추가한다. `onNext(_:)`는 `on(.next(_:))`를 편리하게 쓰는 용도
2. `.completed` 이벤트를 Observer에 추가한다. `onCompleted` 역시 `on(.completed)`를 간소화한 것
3. 추가로 `.next` 이벤트를 추가한다.
4. disposable을 리턴한다.
<br/><br/>

    Note: 처음에는, Disposable 을 리턴하는게 이상해보일 수도 있다. 여기서 subscribe operators 는 반드시 disposable 를 리턴해야한다는 것을 명심하자. 그러므로  disposable를 만들기 위해 Disposables.create() 를 호출하는 것!

<br/><br/>
위의 예제에서 ? 는 emit 될 일이 있을까? 3초 생각해보자.

```swift
.subscribe(
  onNext: { print($0) },
  onError: { print($0) },
  onCompleted: { print("Completed") },
  onDisposed: { print("Disposed") }
)
.disposed(by: disposeBag)
```

이 코드를 뒤에 붙여 확인해보면

```swift
--- Example of: create ---
1
Completed
Disposed
```

결과다!

결론은 ? 은 안나온다. 두번째 줄에서 이미 completed 되었기 때문. 끝났으니까!

중간에 error 를 찍어봐도 error 가 나오고 끝난다.


</br></br>

### 4. .deferred()

- 앞에서는 observable 을 먼저 생성하고 subscribe 를 기다렸다. 하지만 subscriber 에게 새로운 observable 을 유동적으로 줄 수도 있다.
- .deferred 는 **Observable을 리턴하는 메소드**다.
- Swift 기본 문법에서 lazy var 같은 느낌처럼, subscribe 될 때, .deferred가 실행되어 리턴 값인 Observable이 나오게 된다.
</br></br>

예시를 보자.

```swift
example(of: "deferred") {
     let disposeBag = DisposeBag()
     
     // 1
     var flip = false
     
     // 2
     let factory: Observable<Int> = Observable.deferred{
         
         // 3
         flip = !flip
         
         // 4
         if flip {
             return Observable.of(1,2,3)
         } else {
             return Observable.of(4,5,6)
         }
     }
     
     for _ in 0...3 {
         factory.subscribe(onNext: {
             print($0, terminator: "")
         })
             .disposed(by: disposeBag)
         
         print()
     }
 }
 /* Prints:
 123
 456
 123
 456
 */
```

1. Observable이 리턴할 `Bool`값을 생성한다.
2. `deferred` 연산자를 이용해서 `Int` factory Observable을 생성한다.
3. `factory`가 구독할 `flip`을 전환한다 (false > true)
4. `flip`의 값에 따라 다른 Observable을 리턴하도록 한다.

⇒ 결과를 보자.  번갈아 가면서 Observable 을 다르게 주고 싶다던지 할 때 유용하다!

</br></br>

## 7. Traits 이용하기

---


- Traits 는 일반 observables 보다는 좁은 범위의 기능을 제공하는 observable 이다.
- 일반 observable 사용하는 곳 어디든 쓸 수 있다.
- 목적? 가독성과 직관성, 전달력을 높이기 위해!
- 종류는 3가지다. Single, Maybe and Completable. 이름으로 기능 추측해보자 ~~

</br></br>
### 1. Single

- Single은 Obvservable의 한 형태이며, 한 가지의 값 또는 에러를 emit 한다. 그렇기에 Single을 구독시 success, error 두 개의 이벤트에 처리를 한다.
- `.success(value)` 또는 `.error` 이벤트를 방출한다.
- `.success(value)` = `.next` + `.completed`
- 사용: 성공 / 실패로 확인될 수 있는 1회성 프로세스 (예. 데이터 다운로드, 디스크에서 데이터 로딩)
</br></br>

예제를 보자.

```swift
example(of: "Single") {
  // 1
  let disposeBag = DisposeBag()

// 2
  enum FileReadError: Error {
	case fileNotFound, unreadable, encodingFailed
  }

// 3
  func loadText(from name: String) -> Single<String> {
    // 4
    return Single.create { single in
			// 4 - 1
			let disposable = Disposables.create()
			// 4 - 2
			guard let path = Bundle.main.path(forResource: name, ofType:"txt") 
		else {
			  single(.error(FileReadError.fileNotFound))
			  return disposable
		}
			// 4 - 3
			guard let data = FileManager.default.contents(atPath: path) 
				else {
			  single(.error(FileReadError.unreadable))
			  return disposable
			}
			// 4 - 4
			guard let contents = String(data: data, encoding: .utf8) else {
			  single(.error(FileReadError.encodingFailed))
			  return disposable
			}
			// 4 - 5
			single(.success(contents))
			return disposable
	    }
} }
```

1. dispose bag 만든다.
2. 파일 읽기에서 발생할 수 있는 에러 enum 정의한다. 
3. 파일을 읽고 Single 을 리턴하는 함수를 정의한다. 
4. Create 하고 Single 을 리턴한다.

</br></br>
4-1. Disposable 만든다. 리턴해줘야 하니까!

4-2. 파일 path 를 받아오고, 못받아오면 싱글에 에러를 추가하고 Disposable 리턴

4-3. path 로 부터 data 를 받아오고, 못받아오면 싱글에 에러를 추가하고  Disposable 리턴.

4-4. data 를 string 으로 변환하고, 못하면 싱글에 에러를 추가하고 Disposable 리턴. 패턴 보이죠 ?

4-5. 여기까지 왔으면 성공임 !! 싱글 success 와 contents 를 만들고 Disposable 리턴!

</br></br>
```swift
loadText(from: "Copyright")
         .subscribe {
             switch $0 {
             case .success(let string):
                 print(string)
             case .error(let error):
                 print(error)
             }
         }
         .disposed(by: disposeBag)
```

이렇게 실행시켜보자 !

</br></br>

### Single 의 장점과 단점

**장점**

- 두 개의 이벤트만 처리하기 때문에 코드가 줄어든다. 그리고 간단 명료하다.

**단점**

- 하지만 Single은 만능이 아니다. Stream에서 Single을 사용한다면 Single로 시작을 해야한다. Observable로 시작해서 중간에 Single을 엮는다거나 하게 되면 문제가 발생한다.

⇒ 따라서 Single을 사용해야 한다면 반드시 Single로 시작하도록 하고, Observable로 시작한 경우 불필요한 에러를 발행할 수 있기 때문에 가급적 지양하는 것이 좋다.

</br></br>

### 2. Completable

- `.completed` 또는 `.error` 만을 방출하며, 이 외 어떠한 값도 방출하지 않는다.
- 사용: 연산이 제대로 완료되었는지만 확인하고 싶을 때 (예. 파일 쓰기)

```swift
test(success: true)
    .subscribe(onCompleted: { completed in
                //완료되었을 때
    }, onError: { error in
                //에러났을 때
    })
    .disposed(by: disposeBag)
```

다음과 같이 사용하기! 

</br></br>

### 3. Maybe

- `Single`과 `Completable`을 섞어놓은 것
- `success(value)`, `.completed`, `.error`를 모두 방출할 수 있다. 즉 두가지 특징 다 가진다.
- 사용: 프로세스가 성공, 실패 여부와 특정 값도 emit 하고 싶다면 쓰자.
</br></br>

사용 예시
```swift
test()
    .subscribe { maybe in
        switch maybe {
            case .success(let element):
                print("Completed with element \(element)")
            case .completed:
                print("Completed with no element")
            case .error(let error):
                print("Completed with an error \(error.localizedDescription)")
        }
    }
    .disposed(by: disposeBag)
```


# 참조

[민소네블로그](http://minsone.github.io/programming/rxswift-single-traits)

RxSwift 4th.pdf