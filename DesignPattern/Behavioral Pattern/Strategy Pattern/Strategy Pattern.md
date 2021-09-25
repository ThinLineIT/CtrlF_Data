# 전략 패턴(Strategy Pattern)

<!--Table of Contents-->
- 전략 패턴이란?
- 전략 패턴의 장단점
- 전략 패턴의 구현예시

## You Can Answer
- 전략 패턴이란 무엇인가요?
- 전략 패턴은 언제 사용하나요?
- 전략 패턴을 사용함으로써 이점이 무엇이 있나요?

## 전략 패턴 이란?
- 알고리즘 대체를 위해 동일한 목적을 지닌 알고리즘군을 인터페이스로 정의하고 캡슐화하여 서로 대체가 가능하게 하는 패턴이다.
- 상황에 따라 사용해야할 알고리즘을 클라이언트가 정해야하거나 동일 계열의 알고리즘들을 인터페이스로 통일하여 제공해야 할 때 사용된다.

### 구조
![img_Strategy](./img/img_Strategy.png)

- Strategy(전략) : 전략 사용을 위한 인터페이스 정의한다.
- implementationOne,Two : Strategy 인터페이스에서 명시한 알고리즘을 실제로 구현하는 클래스이다.
- Context : 전략 패턴을 이용하는 역할을 수행한다.



## 장단점
- 장점
  - 코드 중복을 방지할 수 있다.
  - 새로운 클래스 추가와 알고리즘 변경에 용이하다.
- 단점
  - 알고리즘별로 클래스를 생성하여 프로그램 내의 객체의 수가 증가한다.
  - 적합한 알고리즘을 선택하기 위해 사용되는 다양한 전략에 대해 미리 인지하고 있어야한다.

## 구현 예시
운송수단들이 두가지 방법(선로 or 도로)으로 움직일 수 있을 때, 이 두가지 방법으로 움직일 수 있는 여러 탈것들이 개발될 수 있다는 가정하에 전략패턴을 적용하여 구현한다.

![diagram_Strategy](./img/diagram_Strategy.png)

1. 전략 정의하기
```java
public interface MovableStrategy {
    public void move();
}
public class LoadStrategy implements MovableStrategy {
    @Override
    public void move() {
        System.out.println("도로를 통해 이동");
    }
}
public class RailLoadStrategy implements MovableStrategy {
    @Override
    public void move() {
        System.out.println("선로를 통해 이동");
    }
}
```
&nbsp;&nbsp; 움직이는 두 방식에 대해 Strategy 클래스를 생성하고, 각각 move() 메소드를 구현하여 어떤 방식으로 움직이는지에 대해 구현한다.
두 전략 클래스를 캡슐화 하기 위해 MovableStrategy 인터페이스를 생성한다.

2. 운송수단 선언
```java
public class Moving {
    private MovableStrategy movableStrategy;

    public void move() {
        movableStrategy.move();
    }

    public void setMovableStrategy(MovableStrategy movableStrategy) {
        this.movableStrategy = movableStrategy;
    }
}
public class Bus extends Moving {

}
public class Train extends Moving {
}
```
&nbsp;&nbsp; 기차와 버스 같은 운송수단의 움직이는 방식을 직접 메소드로 구현하지 않고, 어떻게 움직일 것인지에 대한 전략을 설정하는 setMovableStrategy() 메소드를 구현한다.

3. Main
```java
public class Client {
    public static void main(String[] args) {
        Moving train = new Train();
        Moving bus = new Bus();

        train.setMovableStrategy(new RailLoadStrategy());
        bus.setMovableStrategy(new LoadStrategy());

        train.move();
        bus.move();

        /** rail load bus development*/
        bus.setMovableStrategy(new RailLoadStrategy());
        bus.move();
    }
}
```
&nbsp;&nbsp; 운송수단(버스, 기차) 각각 객체를 생성하고, 어떻게 운송수단을 움직일 것인지 전략을 setMovableStrategy() 메소드로 설정한다.


## Reference
- [[Design Pattern] Strategy Pattern](https://beomseok95.tistory.com/270)
- [전략 패턴](https://ko.wikipedia.org/wiki/%EC%A0%84%EB%9E%B5_%ED%8C%A8%ED%84%B4)
