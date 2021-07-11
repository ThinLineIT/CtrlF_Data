# 템플릿 메소드 패턴(Template Method Pattern)

<!--Table of Contents-->
- 템플릿 메소드 패턴이란?
- 템플릿 메소드 패턴의 장단점
- 템플릿 메소드 패턴의 구현예시

## You Can Answer
- 템플릿 메소드 패턴이란 무엇인가요?
- 템플릿 메소드 패턴은 언제 사용하나요?
- 템플릿 패턴을 사용함으로써 이점이 무엇이 있나요?

## 템플릿 메소드 패턴 이란?
- 하위 클래스에서 공통으로 사용하는 메서드를 상위 클래스에서 정의하고, 다르게 구현해야 하는 메소드를 하위 클래스에서 오버라이드하여 구현하는 패턴을 말한다.
- 상위 클래스에서 정의하는 메소드를 Template Method라 하고, 하위 클래스마다 다르게 구현되어야 하는 메소드들을 Hook이라고 한다.
- 전체적으로는 동일하지만 부분적으로 다른 코드로 구성된 메서드의 코드 중복을 최소화 할 때 유용하다.

## 장단점
- 장점
  - 중복 코드를 줄일 수 있다.
  - 하위 클래스의 역할을 줄여 핵심 로직의 관리가 용이하다.
  - 일정한 템플릿이 있기 때문에 추상 클래스를 상속받아 쉽게 하위 클래스의 생성 및 추가가 가능하다.
- 단점
  - 상위 클래스 수정하게 되면 모든 하위 클래스를 수정해야 한다.
  - 추상 메소드가 많아지면 클래스 관리가 복잡해진다.

## 구현 예시
제작 과정이 거의 비슷한 커피와 홍차 만드는 법을 템플릿 메소드로 구현한다.
1. 커피
   1) 물을 끓인다.
   2) 끓는 물에 커피를 우려낸다.
   3) 커피를 컵에 따른다.
   4) 설탕과 우유를 추가한다.

 2. 홍차
    1) 물을 끓인다.
    2) 끓는 물에 홍차를 우려낸다.
    3) 홍차를 컵에 따른다.
    4) 레몬을 추가한다.

1) 공통으로 사용되는 메소드를 정의하고 다르게 구현해야하는 메소드는 추상 메소드로 구현한 추상 클래스 생성
```java
public abstract class CaffeineBeverage {

  public final void prepareRecipe() {
    boilWater();
    brew();
    pourInCup();
    addcondiments();
  }

  public abstract void brew();
  public abstract void addcondiments();

  private void boilWater() {
    System.out.println("물 끓이는 중");
  }

  private void pourInCup() {
    System.out.println("컵에 따르는 중");
  }
}
```
&nbsp;&nbsp; 커피와 홍차 각각의 제작 과정에서 공통으로 사용되는 boilWater와 pourInCup 메소드를 구현하고, 다르게 구현해야 하는 메소드는 추상화한다.

2) 추상 클래스를 상속받아 추상 메소드를 구현한 Coffe 클래스
```java
public class Coffee extends CaffeineBeverage {

  @Override
  public void brew() {
    System.out.println("필터를 통해 커피를 우려내는 중");
  }

  @Override
  public void addCondiments() {
    System.out.println("설탕과 우유를 추가하는 중");
  }
}
```
&nbsp;&nbsp; 추상클래스인 CaffeineBeverage를 상속받아 추상 클래스인 brew와 addCondiments 메소드를 오버라이드하여 Coffe만의 메소드로 구현한다.

3) 추상 클래스를 상속받아 추상 메소드를 구현한 Tea 클래스
```java
public class Tea extends CaffeineBeverage {

  @Override
  public void brew() {
    System.out.println("차를 우려내는 중");
  }

  @Override
  public void addCondiments() {
    System.out.println("레몬을 추가하는 중");
  }
}
```
&nbsp;&nbsp; 추상클래스인 CaffeineBeverage를 상속받아 추상 클래스인 brew와 addCondiments 메소드를 오버라이드하여 Tea만의 메소드로 구현한다.

## Reference
- [디자인패턴 - 템플릿 메소드 패턴 (template method pattern)](https://jusungpark.tistory.com/24)
