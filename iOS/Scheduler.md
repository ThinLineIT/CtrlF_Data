# Scheduler
<!--Table of Contents-->

    
<!-- 어떤 질문을 대답할 수 있어야 하는지-->
## You can answer
- RxSwfit 의 스케줄링에 대해 설명해주세요.

<br>

<!--Contents-->

---
<br>

## RxSwift 의 스케줄러에 대해 알아보자!
<br>

<img width="756" alt="스크린샷 2021-06-27 오전 1 10 26" src="https://user-images.githubusercontent.com/70083982/123519135-76443080-d6e4-11eb-8291-2c2af9eb7f89.png">

<br>

**iOS 앱을 만들 때 본 적있는 멀티스레딩 처리를 할 때 쓰는 Main 큐!  Rx 에서도 비슷한 용어로 불린다.** 

하지만 여기서 차이점은

- 스케쥴러는 추상화된 Context이기 때문에 쓰레드와 1:1로 매칭되지 않는다. 하나의 쓰레드에 두 개 이상의 개별 스케쥴러가 존재하거나, 하나의 스케쥴러가 두 개의 쓰레드에 걸쳐있는 경우도 있다.

하지만 큰 틀에서 보면 매우 유사하다!

- 예를 들어 UI update와 관련된 코드는 Main 쓰레드에서 실행해야하는데, 이를 GCD에서는 Main Queue에서 실행하고, RxSwift에서는 Main Scheduler에서 실행한다.
- 그리고 Network request나 파일 처리 같은 작업을 메인 쓰레드에서 실행하면 Blocking이 발생한다. 그래서 GCD에서는 이러한 작업을 Global Queue에서 실행하는데, RxSwift에서는 Background Scheduler에서 실행!

<br>
<br>

우선 곰튀김님이 하셨던 명강의 내용부터 복습해보자. 

```swift
Observable.just("800x600")
            .map { $0.replacingOccurrences(of: "x", with: "/") }
            .map { "https://picsum.photos/\($0)/?random" }
            .map { URL(string: $0) }
            .filter { $0 != nil }
            .map { $0! }
            .map { try Data(contentsOf: $0) }
            .map { UIImage(data: $0) }
            .subscribe(onNext: { image in
                self.imageView.image = image
            })
            .disposed(by: disposeBag)
```

사진을 가져와 띄우는 이 예제에서!


버튼을 누르고 (이미지를 로딩) 스크롤을 해보면 안먹히는 것을 볼 수 있다! 

= async 하게 처리되고 있지 않다는 뜻! 

코드를 보면 모든 동작 just, map, dispose 등 모든 동작을 Main thread 에서 하는 것을 볼 수 있다!

그러면 얘네들을 Main thread 가 아닌 Concurrency thread 로 옮겨줘야 한다는 뜻이겠다 ~

이렇게 스레드 사이를 이동하게하는 operator 에는 딱 두가지! 뿐 !

1. **ObserveOn( 스케줄러 이름 )**
2. **SubscribeOn( 스케줄러 이름 )**

<br>
<br>


### 1. ObserveOn

자 그럼 위 예제를 다시보자.

우리는 이미지를 Main Thread 가 아닌 곳에서 가져와야 하잖아?!

그러니까 **다른 Thread 에서 실행**해야 하는데! 이럴 때 ObserveOn 을 쓰는 것!

ObserveOn 을 수행하면 그 다음줄이 실행되는 스케줄러를 변경한다.

<img width="768" alt="스크린샷 2021-06-27 오전 1 12 43" src="https://user-images.githubusercontent.com/70083982/123519199-c8855180-d6e4-11eb-8abc-5a022061add1.png">


빨간 줄 쳐놓은 곳을 보면 

1. 이미지를 가져오는 부분을 **ConcurrentDispatchQueueScheduler** 라는 스케줄러에서 실행해!
2. 이미지를 보여주는 부분 (UI) 는 **MainScheduler** 에서 실행해!

이렇게 실행하면 정상적으로 이미지 로딩과 스크롤을 동시에 실행하는 것을 볼 수 있어!

<br>
<br>



예시를 하나 더 볼까?

```swift
sequence1
  .observeOn(backgroundScheduler)
  .map { n in
      print("This is performed on the background scheduler")
  }
  .observeOn(MainScheduler.instance)
  .map { n in
      print("This is performed on the main scheduler")
  }
```

읽어보고 어떻게 될지 생각해보자!

⇒ observeOn 다음 줄의 map 이 작업할 스케줄러를 바꿔준다. 첫번째 map 은 background 위에서 작업, 두번째는 main 위에서 작업!

<br>
<br>



### 2. SubscribeOn

우선 공식문서에는

If you want to start sequence generation (subscribe method) and call dispose on a specific scheduler, use subscribeOn(scheduler).

⇒ 시퀀스 생성을 하고 dispose call 을 특정 스케줄러 위에서 하고 싶다면, **subscribeOn** 을 쓰라고 하고 있다!

그런데 !! 두둥탁 !!

시퀀스는 언제 생성되나 !

저번에 Observable 내용 remind ☀️

- subscribe가 호출되기 전까지 observable은 선언만 된 상태이기 때문에 어떤 event도 일어나지 않는다.
- subscribe가 호출되어야만 observable이 생성되는 것이다.

즉, **subscribeOn 은 시퀀스가 생성될 때 (= subscribe() 가 호출될 때) 의 스케줄러를 지정하는 것이다!**

<br>
<br>

예시를 보자.

```swift
Observable<Int>.create { observer in
    observer.onNext(1)
    observer.onNext(2)
												
    print("Hi \(Thread.isMainThread)")

    observer.onCompleted()
    return Disposables.create()
}
.observeOn(MainScheduler.instance)
.subscribeOn(ConcurrentDispatchQueueScheduler(qos: .background))
.subscribe(onNext: { el in	    
    print("onNext \(el) \(Thread.isMainThread)") 
}, onDisposed: {(
    print("dispose \(Thread.isMainThread)")
)})

/*
	onNext 1 true
	onNext 2 true
	Hi false
	dispose true
*/
```


위 코드에서는 

**.create 클로저가 subscribeOn의 영향을 받고** 

**subscribe는 observeOn의 영향을 받는다.** 

이름 때문에 헷갈릴 수도 있지만 subscribeOn이 subscribe 자체에 영향을 주는 게 아니라는 것을 명심!

1. `.observeOn()` 로 그 다음에 오는 subscribe 안의 스케줄러를 main 스케줄러로 지정해줌.
2. `.subscribeOn()` 로 observable이 생성되는 스케줄러를 background로 지정해줌.

3. `.subscribe()`가 호출되었을 때 onNext 이벤트가 발생하고 이 이벤트를 subscribe에서 받아서 출력하므로 true 출력
4. Hi 는 Observable 이 생성되는 곳에 있으므로 false 출력!

<br>
<br>


**실전 예시**

```swift

// 1. observable을 선언. 아직 subscribe가 되지 않아 생성되진 않은 상태.
let todoObservable: Observable<[String]> = NetworkService.loadTodoList()

todoObservable
  // 2. observable을 background에서 생성.
  .subscribeOn(ConcurrentDispatchQueueScheduler(qos: .default))
  // 3. 이후부터는 main에서 처리하도록 함.
  .observeOn(MainScheduler.instance)
  .subscribe(onNext: { todoList in
      print("Todo: \(todoList)")
  })
  .disposed(by: disposeBag)
```

네트워킹으로 받아온 데이터로 background에서 observable을 생성하고 observable이 방출하는 element들은 main에서 처리하는 코드.

<br>
<br>


이제 여기서!! Marble 을 보면 완. 벽. 이. 해

![image](https://user-images.githubusercontent.com/70083982/123519276-1ef29000-d6e5-11eb-9429-df69a23ffa1e.png)
![image](https://user-images.githubusercontent.com/70083982/123519287-303b9c80-d6e5-11eb-83d5-8a1117abb4ce.png)


위에서 부터 내려오면서 색깔이랑 유의해서 읽어보자! 

<br>
<br>
<br>



### 결론

## **observeOn**

operator(map, filter, etc)와 subscribe 작업을 다른 스케줄러에서 사용하고 싶을 때 사용.

## **subscribeOn**

observable을 특정 스케줄러에서 생성하고 싶을 때 사용.

<br>

## 스케줄러 Scheduler

---

### 1. 주로 사용되는 스케줄러는 두가지

- `MainScheduler.instance` 메인 스레드! 얘는 맨날 사용하니까 그냥 외우기
- `ConcurrentDispatchQueueScheduler` 얘 작업을 수행하려고 GCD 를 사용한다고 한다

### 2. Built In Scheduler

<br>
<br>

## **CurrentThreadScheduler (Serial scheduler)**

- 기본으로 제공되는 디폴트 스케줄러라고! 별도로 스케쥴러를 별도로 지정하지 않는다면, 이 스케쥴러가 사용된다.
- 어떤 스레드에서 `CurrentThreadScheduler.instance.schedule(state) { }`가 호출되면, 작업들이 임시로 저장되는 숨겨진 큐가 생성된다!

<br>

## **MainScheduler (Serial scheduler)**

- MainThread 에서 수행되어야 하는 작업들이 여기서 사용!
- 이 스케줄러는 보통 UI 작업에 쓰임!

<br>

## **SerialDispatchQueueScheduler (Serial scheduler)**

- `dispatch_queue_t` 에서 수행되어야 하는 작업들이 여기서 사용! ~~저거뭔지모름~~
- 이거는 concurrent dispatch 큐가 전달되어도 serial dispatch 큐로 변환된다.
- 메인 스케줄러는 `SerialDispatchQueueScheduler` 의 인스턴스 중 하나라고!

<br>

## **ConcurrentDispatchQueueScheduler (Concurrent scheduler)**

- 이 스케줄러는 작업이 백그라운드에서 수행되어야할 때 적합하다.
- 우리가 아까 앞에서 이미지 불러오는 작업을 이 스케줄러 한테 맡겼었지 !

<br>

## **OperationQueueScheduler (Concurrent scheduler)**

- `NSOperationQueue` 에서 수행되어야 하는 작업이 여기서 수행
- 이 스케줄러는 백그라운드에서 수행되어야 하는 어떤 큰 덩어리의 작업이 있고 `maxConcurrentOperationCount`를 이용해서 비동기 처리 작업을 미세하게 조정하고 싶을 때 적합하다.

이번 개념을 배우면서 Observing 과 ObserveOn , subscribe 와 subscribeOn 을 혼동하지 않는 것이 중요할 것 같아!

잘 설명해놓은 블로그가 있어서 링크 첨부할게 !!

[http://rx-marin.com/post/observeon-vs-subscribeon/](http://rx-marin.com/post/observeon-vs-subscribeon/)