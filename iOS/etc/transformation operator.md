
# 변환연산자 Transformation Operator

### RxSwift 공식 문서를 읽고 정리한 글이다. by 이지원</br></br>



## You can answer
- Observable 구독?
- Observable 생성 연산자들
- Observable의 dispose와 종료에 대해서 설명해보세요.
- Single, maybe, Completable 은 어떤 상황에서 어떻게 사용되나?
- RxSwift Observable 사용의 전체적인 흐름, 구조
<br/>
<br/>

### 1. toArray

- 이름을 봐라. 뭘까? 맞다! array 로 변경해준다.<br/><br/>

<img width="632" alt="_2021-05-04__11 01 49" src="https://user-images.githubusercontent.com/70083982/119273684-f5a69600-bc46-11eb-89a4-4ccb85e93846.png">
<br/><br/>

```swift
example(of: "toArray") {
 	let disposeBag = DisposeBag()
 	
 	Observable.of("A", "B", "C")
 		.toArray()
 		.subscribe(onNext: {
 			print($0)
 		})
 		.disposed(by: disposeBag)
 		
 	/* Prints:
 		["A", "B", "C"]
 	*/
 }

```

observable 을 생성하고 toArray 연산자를 이용해 array로 변경!<br/>
<br/><br/>

### 2. Map

- mapping! 매핑 시켜준다고 생각하면 쉽다.
- 원본 옵저버블이 방출하는 요소를 대상으로 함수를 실행하고 결과를 새로운 옵저버블로 리턴한다.

<img width="629" alt="_2021-05-04__11 05 05" src="https://user-images.githubusercontent.com/70083982/119273694-0c4ced00-bc47-11eb-930f-57df9163890d.png">

```swift
let skills = ["Swift", "SwiftUI", "RxSwift"]

Observable.from(skills)
    .map { $0.count }
    .subscribe{ print($0) }
    .disposed(by: disposeBag)

----- RESULT -----
next(5)
next(7)
next(7)
completed
```

예시를 보면 string 의 count 로 mapping 시켜주는 것이 보인다. 
<br/><br/><br/>

### 3. enumerated

요 아이는 공식사이트에서 나와있지 않고 구글링해도 잘 안쓰는 것 같아서 생략 ..?

보면 요소들의 값과 index를 갖는 tuple을 만들어주는 것 같다. 

<br/><br/><br/>

### 4. flatMap

- 원본 옵져버블이 emit 하는 항목을 새로운 옵져버블로 변환
- 새로운 옵져버블은 항목이 업데이트 될때마다 새로운 항목을 emit
- 최종적으로 하나의 옵져버블로 합쳐지고 모든항목이 옵져버블을 통해 emit
- 네트워크 요청을 구현할 때 활용

<br/><br/>

<img width="620" alt="_2021-05-04__11 18 00" src="https://user-images.githubusercontent.com/70083982/119273732-32728d00-bc47-11eb-8d1b-3f2ecbc1af1a.png">

FlatMap은 Observables의 emit 들을 합치기 때문에, 동시에 일어날 수 있다. <br/><br/>

<img width="414" alt="_2021-05-04__11 20 24" src="https://user-images.githubusercontent.com/70083982/119273737-39010480-bc47-11eb-86ec-9997d73e1a88.png">
<br/><br/>

- 첫 Observable은 `Int`타입의 `value` 값을 가지고 있다. 각각의 고유한 값은 `01` = `1`, `02` = `2`, `03` = `3`을 의미한다.
- `01`부터 시작하여 `flatMap`은 객체를 수신하고 value 속성에 접근하여 10을 곱한다. 그리고 `01`로 부터 변환된 새 값을 새 `Observable` (01의 경우 flatMap 아래 첫 번째 줄)에 투영한다. 이렇게 subscriber(마지막줄)에게 줄 observable까지 내려간다.
- 이 후 `01`의 값 속성이 4로 변경된다. 이 부분은 그림에서 표현하지 않았다. 너무 복잡해지므로. 다만, `01`의 값이 바뀌었다는 증거는 해당 Observable에서 값이 40으로 변형된 것을 보고 확인할 수 있다.
- 첫 째줄의 Observable에서 방출하는 다음 값은 `02`다. 역시 `flatMap`이 받는다. 이 값은 `20`으로 전환되고 역시 새 Observable에 투영된다. 이 후 `02`의 값은 `5`로 바뀔 것이고, 이 값 역시 `50`으로 전환된다.
- 최종적으로 `03`을 `flatMap`이 받아 변환시킨다.

<br/><br/>

```swift
example(of: "flatMap") {
     let disposeBag = DisposeBag()
     
     // 1
     let ryan = Student(score: BehaviorSubject(value: 80))
     let charlotte = Student(score: BehaviorSubject(value: 90))
     
     // 2
     let student = PublishSubject<Student>()
     
     // 3
     student
         .flatMap{
             $0.score
         }
         // 4
         .subscribe(onNext: {
             print($0)
         })
         .disposed(by: disposeBag)
     
     // 5
     student.onNext(ryan)    // Printed: 80
     
     // 6
     ryan.score.onNext(85)   // Printed: 80 85
     
     // 7
     student.onNext(charlotte)   // Printed: 80 85 90
     
     // 8
     ryan.score.onNext(95)   // Printed: 80 85 90 95
     
     // 9
     charlotte.score.onNext(100) // Printed: 80 85 90 95 100
 }
```

```swift
let a = BehaviorSubject(value: 1)
let b = BehaviorSubject(value: 2)

let subject = PublishSubject<BehaviorSubject<Int>>()

subject
    .flatMap{ $0.asObserver() }
    .subscribe{ print($0) }
    .disposed(by: disposeBag)

subject.onNext(a)
subject.onNext(b)

a.onNext(11)
b.onNext(22)

----- RESULT -----
next(1)
next(2)
next(11)
next(22)
```

예제는 나중에 이해해도 될 것 같고, 결론은 flatMap 을 하면 observable의 모든 변화를 지켜본다는 것이다!

변경할 때 마다 출력되는 거 보이지?!
<br/><br/><br/>

### 5. flatMapLatest
<br/>

- 위의 flatMap 에서 최신 것만 확인하고 싶을 때 사용!
- 가장 최근의 항목을 방출한 옵저버블의 요소만 방출
- 자동적으로 이전 observable을 구독해지한다.
- 자매품으로 flatMapFirst 도 있음
<br/><br/>
<img width="411" alt="_2021-05-04__11 26 47" src="https://user-images.githubusercontent.com/70083982/119273776-66e64900-bc47-11eb-9e93-7b9cc7dfd4b2.png">
<br/><br/>

```swift
let a = BehaviorSubject(value: 1)
let b = BehaviorSubject(value: 2)

let subject = PublishSubject<BehaviorSubject<Int>>()

subject
   .flatMapLatest { $0.asObservable() }
   .subscribe { print($0) }
   .disposed(by: disposeBag)

subject.onNext(a)
a.onNext(11)

subject.onNext(b)
b.onNext(22)

a.onNext(11)

----- RESULT -----
next(1)
next(11)
next(2)
next(22)
```

- 가장 나중에 등록된 애의 변화만 추적하는 거라고 생각하면 된다. a 변경해도 무시한다!

<br/><br/>

### 언제 사용할까?

- flatMapLatest 는 네트워킹에서 가장 흔하게 쓰인다.
- 사전으로 단어를 찾는 것을 생각해보자. 사용자가 각 문자 s, w, i, f, t를 입력하면 새 검색을 실행하고, 이전 검색 결과 (s, sw, swi, swif로 검색한 값)는 무시해야할 때 사용할 수 있을 것이다.

<br/><br/><br/>

### 6. Buffer

- periodically gather items emitted by an Observable into bundles and emit these bundles rather than emitting the items one at a time
- 말그대로 버퍼. Observable 이 emit 하는 아이템을 한번에 하나씩 emit 을 그대로 해주는 것이 아니라 묶어서 ! 모아서 하나의 배열로 리턴하는 연산자.
- 파라미터는 최대시간, 최대갯수 (하나만 충족하면 방출)
- 수집된 배열을 방출하는 **옵저버블을 리턴**<br/><br/>

<img width="622" alt="_2021-05-04__11 34 15" src="https://user-images.githubusercontent.com/70083982/119273795-8a10f880-bc47-11eb-8fcc-d2d1f18627d4.png">

<br/>

```swift
Observable<Int>.interval(.seconds(1), scheduler: MainScheduler.instance)
    .buffer(timeSpan: .seconds(2), count: 3, scheduler: MainScheduler.instance)
    .take(5)
    .subscribe{ print($0) }
    .disposed(by: disposeBag)

----- RESULT -----
next([0])
next([1, 2, 3])
next([4, 5])
next([6, 7])
next([8, 9])
completed
```
<br/>

- 2초, count 3 이 만족되면 리턴<br/>

    window 연산자는 buffer 와 비슷하지만 아이템을 모아 어떤 자료구조에 넣는 것이 아니라 분리된 observable 로 emit 한다는 점이 다르다. 

The Window operator is similar to Buffer but collects items into separate Observables rather than into data structures before reemitting them. <br/>
~~맞게 해석한건지 모르겠다....~~

<br/><br/><br/>

### 7. window

- 최대시간, 최대갯수를 지정해 원본 옵저버블이 방출하는 항목들을 작은 단위의 옵저버블로 분해, 리턴
- 옵저버블을 방출하는 옵저버블을 리턴(Inner Observable)
<br/><br/>


    <img width="624" alt="_2021-05-04__11 39 08" src="https://user-images.githubusercontent.com/70083982/119273829-b0cf2f00-bc47-11eb-8b1f-9402875945ef.png">

<br/>

```swift
Observable<Int>.interval(.seconds(1), scheduler: MainScheduler.instance)
    .window(timeSpan: .seconds(2), count: 3, scheduler: MainScheduler.instance)
    .take(5)
    .subscribe{
        print($0)
        
        if let observable = $0.element {
            observable.subscribe { print("inner : \($0)") }
        }
}
    .disposed(by: disposeBag)

----- RESULT -----
next(RxSwift.AddRef<Swift.Int>)
inner : next(0)
inner : completed
next(RxSwift.AddRef<Swift.Int>)
inner : next(1)
inner : next(2)
inner : next(3)
inner : completed
next(RxSwift.AddRef<Swift.Int>)
inner : next(4)
inner : next(5)
inner : completed
next(RxSwift.AddRef<Swift.Int>)
inner : next(6)
inner : next(7)
inner : completed
next(RxSwift.AddRef<Swift.Int>)
completed
inner : next(8)
inner : next(9)
inner : completed
```


<br/><br/><br/>

### 8. groupBy

- 방출되는 요소를 조건에 따라 그룹핑
- flatMap, toArray 를 활용해 최종 결과를 하나의 배열로 방출할 수 있음<br/><br/>

    <img width="621" alt="_2021-05-04__11 49 44" src="https://user-images.githubusercontent.com/70083982/119273877-ea079f00-bc47-11eb-9544-9741709f2634.png"><br/>

```swift
let words = ["Apple","Banana","Orange","Book","City","Axe"]

// 첫번째 문자로 그룹핑
Observable.from(words)
    .groupBy { $0.first }
    .flatMap{ $0.toArray() }
    .subscribe{ print($0) }
    .disposed(by: disposeBag)

----- RESULT -----
next(["City"])
next(["Orange"])
next(["Banana", "Book"])
next(["Apple", "Axe"])
completed
```

- word 들의 첫번째 글자로 묶은 뒤 flatMap 을 이용해 배열로 만든뒤 출력하는 모습

```swift
Observable.range(start: 1, count: 10)
    .groupBy{ $0.isMultiple(of: 2) }
    .flatMap{ $0.toArray() }
    .subscribe{ print($0) }
    .disposed(by: disposeBag)

----- RESULT -----
next([1, 3, 5, 7, 9])
next([2, 4, 6, 8, 10])
completed
```

- 홀수 짝수로 그룹핑하는 모습

<br/><br/><br/>

### 9. Scan

- 기본값으로 연산을 시작
- Observable 이 emit 하는 각각의 아이템에 함수를 적용, 순차적으로, 그리고 연속적으로 각각의 값을 emit
- 원본 옵저버블이 방출하는 항목을 대상으로 변환을 실행한 다음 결과를 방출하는 하나의 옵저버블을 리턴
- 원본이 방출하는 항목의 수 = 구독자로 전달되는 항목의 수<br/>

<img width="626" alt="_2021-05-04__11 51 41" src="https://user-images.githubusercontent.com/70083982/119273884-f25fda00-bc47-11eb-8185-462f005c84da.png">

<br/>

```swift
Observable.range(start: 1, count: 10)
    .scan(0, accumulator: +)
    .subscribe{ print($0) }
    .disposed(by: disposeBag)

----- RESULT -----
next(1)
next(3)
next(6)
next(10)
next(15)
next(21)
next(28)
next(36)
next(45)
next(55)
completed
```

1 ~ 10 의 합을 출력하는 모습