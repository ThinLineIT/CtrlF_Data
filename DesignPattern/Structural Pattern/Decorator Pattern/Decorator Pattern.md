# 데코레이터 패턴(Decorator Pattern)

<!--Table of Contents-->
- 데코레이터 패턴이란?
- 데코레이터 패턴의 장단점
- 데코레이터 패턴의 구현예시

## You Can Answer
- 데코레이터 패턴이란 무엇인가요?
- 데코레이터 패턴은 언제 사용하나요?
- 데코레이터 패턴을 사용함으로써 이점이 무엇이 있나요?

## 데코레이터 패턴 이란?
- 주어진 상황 및 용도에 따라 객체의 추가적인 기능 등을 동적으로 추가하는 패턴이다.
- 클래스의 요소들을 계속해서 수정하면서 사용하는 구조가 필요한 경우와 여러 요소들을 조합해서 사용하는 클래스 구조인 경우 사용된다.

## 장단점
- 장점
  - 기존 코드를 수정하지 않고도 행동을 확장시킬 수 있다.
  - 객체에 동적인 기능 추가가 간단하다.
- 단점
  - 데코레이터 클래스들이 계속 추가되어 클래스가 많아지기 때문에 객체의 정체를 파악하기 힘들고 복잡하다.

## 구현 예시
컴포지트 패턴을 적용하여 기본 빵 객체를 대상으로 여러가지 재료를 조합하여 동적으로 햄버거를 만드는 것을 구현한다.

![diagram_Decorator](./img/diagram_Decorator.png)

[Hamburger]
```java
public abstract class Hamburger {
    public abstract String get_Current_Ingredient();
}
```
&nbsp;&nbsp; 장식 할 대상이 가져야 할 공통 인터페이스를 정의하는 추상 클래스이다.

[Bread]
```java
public class Bread extends Hamburger{
    public String Current_Ingredient = "기본 빵";

    public String get_Current_Ingredient()
    {
        return Current_Ingredient;
    }
}
```
&nbsp;&nbsp; 장식 할 대상인 Bread 클래스이다. 아무런 장식도 되어 있지 않은 빵만 가지고 있는 상태이다.

[Ingredient]
```java
public abstract class Ingredient extends Hamburger {
    public Hamburger hamburger;

    public Ingredient(Hamburger hamburger)
    {
        this.hamburger = hamburger;
    }
}
```
&nbsp;&nbsp; 장식자가 가져야 할 인터페이스를 정의한다. 장식자이면서 장식당하는 대상이기도 하기 때문에 Bread의 상위 클래스인 Hamburger 클래스를 참조한다.

[Bulgogi_Patty 와 shrimp_Patty]
```java
public class Bulgogi_Patty extends Ingredient{

    public Bulgogi_Patty(Hamburger hamburger)
    {
        super(hamburger);
    }
    public String get_Current_Ingredient()
    {
        return hamburger.get_Current_Ingredient() + "+" +"불고기 패티";
    }
}

public class Shrimps_Patty extends Ingredient{

    public Shrimps_Patty(Hamburger hamburger)
    {
        super(hamburger);
    }

    public String get_Current_Ingredient()
    {
        return hamburger.get_Current_Ingredient() + "+" + "새우 패티";
    }
}
```
&nbsp;&nbsp; 장식자들로 기본 빵에 불고기패티와 새우패티를 추가하는 부분을 구현한다.

[Main]
```java
public class Main {

    public static void main(String argsp[])
    {    
        Hamburger hamburger = new Bread();
        System.out.println(hamburger.get_Current_Ingredient());    //아무런 장식이 더해지지 않은 빵만 있는 형태

        Hamburger hamburger2 = new Bulgogi_Patty(hamburger);
        System.out.println(hamburger2.get_Current_Ingredient()); //기본빵에 불고기패티를 추가하는 형태

        Hamburger hamburger3 = new Bulgogi_Patty(hamburger2);
        System.out.println(hamburger3.get_Current_Ingredient()); //기본빵+불고기패티에 불고기 패티를 추가

        Hamburger hamburger4 = new Shrimps_Patty(hamburger3);
        System.out.println(hamburger4.get_Current_Ingredient()); //기본빵+불고기패티+불고기패티에 새우패티 추가
    }
}
```
&nbsp;&nbsp; 기본 빵을 시작으로 동적으로 새로운 재료 등을 추가가 가능하다.

## Reference
- [데코레이터 패턴 (Decorator Pattern))](https://lktprogrammer.tistory.com/61)
