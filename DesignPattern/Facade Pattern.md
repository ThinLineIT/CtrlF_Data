# 퍼사드 패턴(Facade Pattern)

<!--Table of Contents-->
- 퍼사드 패턴이란?
- 퍼사드 패턴의 장단점
- 퍼사드 패턴의 구현예시

## You Can Answer
- 퍼사드 패턴이란 무엇인가요?
- 퍼사드 패턴은 언제 사용하나요?
- 퍼사드 패턴을 사용함으로써 이점이 무엇이 있나요?

## 퍼사드 패턴 이란?
- Facade는 "건물의 정면"을 의미하는 단어로 어떤 소프트웨어의 다른 커다란 코드 부분에 대하여 간략화된 인터페이스를 제공해주는 디자인 패턴을 의미한다.
- 퍼사드 객체는 복잡한 소프트웨어 바깥쪽의 코드가 라이브러리의 안쪽 코드에 의존하는 일을 감소시켜 주고, 복잡한 소프트웨어를 사용 할 수 있게 간단한 인터페이스를 제공한다.
- 복잡한 서브 시스템에 대해 간단한 인터페이스를 제공하고자 할 때, 서브시스템을 계층화시킬 때 사용된다.

![img_Facade](img/img_Facade.png)

## 장단점
- 장점
  - 서브 시스템 간의 결합도를 낮출 수 있다.
  - 클라이언트 입장에서 간결하게 코드를 알아볼 수 있다.

- 단점
  - 레거시 코드 등에 퍼사드 패턴을 적용하게 되면 코드가 복잡해질 수 있다.

## 구현 예시
facade pattern을 적용하여 MobileShop 인터페이스를 구현하는 세 클래스(Iphone, Samsung, Blackberry)를 생성하고 퍼사드 클래스인 ShopKeeper를 구현한다.

![Diagram_Facade](img/Diagram_Facade.png)

<br>
[Interface Class]
```java
public interface MobileShop {
    public void modelNo();  
    public void price();  
}
```
<br>
[Model Class]
```java
public class Iphone implements MobileShop {  
    @Override  
    public void modelNo() {  
        System.out.println(" Iphone 6 ");  
    }

    @Override  
    public void price() {  
        System.out.println(" Rs 65000.00 ");  
    }  
}
public class Samsung implements MobileShop {  
    @Override  
    public void modelNo() {  
        System.out.println(" Samsung galaxy tab 3 ");  
    }  

    @Override  
    public void price() {  
        System.out.println(" Rs 45000.00 ");  
    }  
}
public class Blackberry implements MobileShop {  
    @Override  
    public void modelNo() {  
        System.out.println(" Blackberry Z10 ");  
    }  

    @Override  
    public void price() {  
        System.out.println(" Rs 55000.00 ");  
    }  
}
```
<br>
[Facade Class]
```java
public class ShopKeeper {  
    private MobileShop iphone;  
    private MobileShop samsung;  
    private MobileShop blackberry;  

    public ShopKeeper(){  
        iphone= new Iphone();  
        samsung=new Samsung();  
        blackberry=new Blackberry();  
    }

    public void iphoneSale(){  
        iphone.modelNo();  
        iphone.price();  
    }

    public void samsungSale(){  
        samsung.modelNo();  
        samsung.price();  
    }  

    public void blackberrySale(){  
        blackberry.modelNo();  
        blackberry.price();  
    }  
}
```
<br>
[Operation Class]
```java
public class FacadePatternClient {  
    public static void main(String args[]) {  
        ShopKeeper sk=new ShopKeeper();

        sk.iphoneSale();  
        sk.samsungSale();        
        sk.blackberrySale();       
    }  
}
```

## Reference
- [[자바 디자인 패턴] 구조패턴 - 퍼사드 패턴](https://know-one-by-one.tistory.com/43)
- [디자인패턴 - 퍼사드 패턴 (facade pattern)](https://jusungpark.tistory.com/23)
