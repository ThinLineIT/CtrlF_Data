# 옵셔널이란?


- 어떤 질문을 대답할 수 있어야 하는지
    - 옵셔널이 뭔가요?
    - 옵셔널을 왜 사용할까요?

</br>

# Optional

Swift가 갖는 Optional이라는 개념은 변수의 값이 nil일 수 있다는 것을 표현하는 것인데, 반대로 Optional이 아니라면(non-optional) 해당 값은 nil이 될 수 없음을 의미한다. Objective-C를 사용해왔다면 Optional이라는 표현이 너무나도 당연해보일 수 있지만, Swift에서는 Optional은 말 그대로 옵션(선택적) 이며 기본값은 non-Optional 이다.

</br>

---

</br>

## 선언
Optional 변수의 선언은 ? 키워드를 사용한다

```swift
var name: String?
```

</br>

Optional의 디폴트 값은 nil로 name은 nil을 갖게 된다

```swift
var name: String // 컴파일에러
var name = nil // 컴파일에러
```

</br>

만약 Optional 키워드를 사용하지 않았다면 값을 입력하라는 에러가 발생하고, 그 이후에라도 nil을 넣으려하면 컴파일에러가 발생한다.
여기에서 Swift가 기본적으로 non-optional을 지원하면서 갖는 엄청난 장점을 느낄 수 있다.
nil에 대한 컴파일 에러를 통해 개발자는 nil에 대해 명확한 예외처리가 강제되며, 런타임에 nil로 인한 문제를 컴파일 단계에서 예방할 수 있다.

그래서 Swift가 잠재적 오류에 대해 안전하다고 표현하는 글도 많이 있다.

</br>
</br>

## Optional 변수의 이용

Optional변수는 nil을 가질 수 있는 특별한 변수입니다. Optional 변수를 이용해서 작업할 때 Optional을 해제하는 과정이 추가적으로 필요하다.


```swift
var number1:Int? = 20
var number2:Int = 100
number1 + number2
```


Int(optional)와 Int(non-optional)의 연산을 시도하면

 `Value of optional type 'Int?' not unwrapped; did you mean to use '!' or '?'?`

위 컴파일 에러가 발생한다.
number1은 Optional로 nil값을 가질 가능성이 있기 때문에 컴파일 단계에서 에러를 발생시킨다. 연산을 수행하기 위해서는 unwrapping 또는 binding 과정이 필요하다.

</br>
</br>


## Optional Unwrapping (옵셔널 해제)
Optional Unwrapping이란 Optional 변수에서 Optional 껍데기를 벗겨내는 작업이다. 

```swift
var number1:Int? = 20
var number2:Int = 100

if number1 {
    let sum = number1! + number1!
}
```


if 로 optional 변수의 값이 nil이 아닌지 판별 후 ! unwrapping 키워드를 통해 강제로 값을 꺼내온다. 만약 if로 nil 체크를 하지 않고 !을 사용한다면 런타임 오류가 발생할 수 있다.

</br>
</br>

## Optional Binding (옵셔널 바인딩)

```swift
var number1:Int? = 20
var number2:Int = 100

if let nonOptionalNumber1 = number1 {
    let sum = nonOptionalNumber1 + number2
}
```    

Optional Unwrapping과 비슷하지만 Optional 값을 새로운 상수로 받고, 그 이후로 non-optional 값을 사용한다는 차이점이 있다.
새로 선언된 nonOptionalNumber1은 이미 non-optional로 연산에 사용할 때 ! 키워드를 사용할 필요가 없다.

</br>
</br>

## Optional Chaining (옵셔널 체이닝)

여러 객체를 혼합해서 사용하다보면 Optional끼리의 연산이 필요한 경우가 있다. 이 경우에 객체마다 옵셔널 바인딩을 사용하게 되면, if문이 복잡해진다.
Optional Chaining을 통해서 좀 더 간단하게 Optional 예외처리를 할 수 있다.

swift에서 . 을 통해 클래스의 프로퍼티에 접근이 가능합다는 점을 이용한다.


```swift
class A {
    var b:B?
}

class B {
    var c:String?
}
```
​
이 경우 A의 프로퍼티 b의 c에 접근할 때, b에대한 옵셔널, c에대한 옵셔널, 2번의 바인딩이 필요하다.
이 경우에 옵셔널 체이닝을 통해 한줄로 옵셔널 바인딩이 가능하다.

</br>


```swift
var a:A = A()

if a.b?.c {

}
```
​
a.b?.c 를 통해 b의 옵셔널, c의 옵셔널 바인딩을 모두 처리가능하다.

</br>
</br>


## Optional이 점점 불편해 보이는데 왜 사용할까?
Objective-C에는 아직 nil타입이 존재하며 하나의 프로젝트에서 obj-c와 swift를 혼용해서 사용할 수 있다. 따라서 obj-c와의 상호운용성을 위해 사용한다.
