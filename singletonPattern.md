# 싱글턴 패턴 (Singleton Pattern)

<!-- 어떤 질문을 대답할 수 있어야 하는지-->
## You can answer
- 싱글톤 패턴은 무엇인가요?
- 싱글톤 패턴을 왜 사용해야나요? 
- 싱글톤 패턴의 문제점은 무엇이고, 어떻게 해결할 수 있나요?

---

## 싱글턴 패턴이란?
* 전역 변수를 사용하지 않고 인스턴스를 하나만 생성하도록 하며, 생성된 인스턴스를 어디서든지 참조 할 수 있도록 하는 패턴
* 즉, 인스턴스가 사용될 때 기존에 생성했던 동일한 인스턴스가 사용되게끔 함.
  <br><br>
### 1) 싱글톤 패턴을 사용하는 이유?
* 고정된 메모리 영역과 한번 생성된 인스턴스를 사용함으로써 메모리 낭비를 방지
* 싱글톤으로 생성된 인스턴스는 전역성을 띄기 때문에 다른 인스턴스들이 데이터를 공유하기애 용이
* 인스턴스가 절대적으로 한 개만 존재하는 것을 보증하고 싶을 경우 사용하며,  
  두번째부터는 객체 로딩 시간이 현저하게 줄어 성능이 좋아짐.
  <br><br>
### 2) 싱글톤 패턴의 문제점
* 전역성을 띄면서 다른 인스턴스와 공유하는 경우는 몇 가지 케이스에서만 효율적이고  
  싱글톤으로 만든 인스턴스의 역할이 복잡한 경우라면 해당 싱글톤 객체를 사용하는  
  다른 객체 간의 결함도가 높아져서
  객체지향 설계 원칙 (개방 - 폐쇄 원칙) 에 어긋나게 됨  
  &nbsp;&nbsp; -> &nbsp;&nbsp;수정이 어려워지고 테스트하기 어려움
* 멀티쓰레드 환경에서 동기화 처리를 하지 않으면 인스턴스가 두 개가 생성되는 등의 경우가 발생할 수 있음

### 3) 싱글톤의 구현  (java)
* JAVA에서는 외부에서 인스턴스를 생성할 수 없도록 생성자를 private으로 선언하고,  
  getInstance() 메서드를 static으로 정의하여 미리 생성된 자신을 반환할 수 있도록 함.  
  getInstance() 메서드 호출 시, 생성자 변수에 객체가 할당되지 않아 null이라면 새로운 객체를 생성.  
  이미 있다면 그것을 반환함. <br>

1. static block 
```
public class ExampleClass {
    //Instance
    private static ExampleClass instance;

    //private construct
    private ExampleClass() {}

    static {
        try { instance = new ExampleClass();}
        catch(Exception e) { throw new RuntimeException("Create instace fail. error msg = " + e.getMessage() ); }
    }

    public static ExampleClass getInstance() {
        return instance;
    }
}
```
&nbsp;&nbsp;  클래스 로딩 시점에 최초 한번 실행하며, 초기화 블럭을 이용하여 logic을 담을 수 있기 때문에  
&nbsp;&nbsp;  초기변수 셋팅, 에러처리 구문을 넣을 수 있음  
&nbsp;&nbsp;  => 인스턴스가 사용되는 시점에 생성되는 것은 아님 
<br>

2. lazy init 
```
public class ExampleClass {
    //Instance
    private static ExampleClass instance;

    //private construct
    private ExampleClass() {}

    public static ExampleClass getInstance() {
        if (instance == null) { instance = new ExampleClass();}
        return instance;
    }
}
```
&nbsp;&nbsp;  인스턴스 요청할 때 생성되며 인스턴스가 null 일 경우에만 new 를 사용해 객체를 생성함  
&nbsp;&nbsp;  => 멀티 스레드 방식이라면 동일 시점에 getInstance() 호출되어 두 번 생길 위험 존재

3. Thread safe + lazy 
```
public class ExampleClass {
    //Instance
    private static ExampleClass instance;

    //private construct
    private ExampleClass() {}

    public static synchronized ExampleClass getInstance() {
        if (instance == null) { instance = new ExampleClass();}
        return instance;
    }
}

```
&nbsp;&nbsp;  synchronized 키워드로 쓰레드에서 동시 접근 해결  
&nbsp;&nbsp;  => 수 많은 스레드들이 getInstance() 메서드를 호출하게 되면 높은 비용으로 인해 프로그램 성능 저하 발생<br>

4. Holder 
```
public class ExampleClass {

    //private construct
    private ExampleClass() {}

    private static class InnerInstanceClass() {
        private static final ExampleClass instance = new ExampleClass();
    }

    public static ExampleClass getInstance() {
        return InnerInstanceClazz.instance;
    }
}
```

&nbsp;&nbsp;  JVM의 클래스 로더 매커니즘과 클래스의 로드 시점을 이용하여 내부 클래스를 통해 인스턴스를 생성 시킴으로써 스레드 간의 동기화 문제 해결  

5. Enum initialization
```
public enum ExampleClass {

   INSTANCE;
   static String test = "";

    public static ExampleClass getInstance() {
        test = "test";
        return INSTANCE;
    }
}
```
&nbsp;&nbsp;  enum을 singleton pattern으로 사용할 수 있는 이유 :  
&nbsp;&nbsp;  &nbsp;&nbsp;  INSTANCE가 생성될 때, 멀티스레드로 부터 안전하고 단 한번의 인스턴스 생성을 보장  
&nbsp;&nbsp;  &nbsp;&nbsp;  또한, 사용이 간편하며 enum value는 전역에서 접근 가능

###4) 다른 언어에서의 구현

    1. Python : https://wikidocs.net/3693
    2. PHP : https://stackoverflow.com/questions/203336/creating-the-singleton-design-pattern-in-php5/203359#203359
    3. JS :  https://blog.mgechev.com/2014/04/16/singleton-in-javascript/


---
## Reference
- [JAVA 싱글톤 패턴](https://blog.seotory.com/post/2016/03/java-singleton-pattern)
- [[디자인패턴] 싱글톤 패턴(Singleton Pattern)](https://victorydntmd.tistory.com/293)