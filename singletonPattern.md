#Singleton Pattern

##싱글턴 패턴이란?
* 전역 변수를 사용하지 않고 인스턴스를 하나만 생성하도록 하며, 생성된 인스턴스를 어디서든지 참조 할 수 있도록 하는 패턴
* 즉, 인스턴스가 사용될 때 기존에 생성했던 동일한 인스턴스가 사용되게끔 함.
  <br><br>
### 1) 싱글톤 패턴을 쓰는 이유?
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

1. static block (클래스 로딩 시점에 실행)
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

2) lazy init (인스턴스 요청할 때 생성됨)
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

3. Thread safe + lazy (synchronized 키워드로 쓰레드에서 동시 접근 해결하지만 synchronized 키워드는 성능 저하 발생 )
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

4. Holder (JVM의 클래스 로더 매커니즘과 클래스의 로드 시점을 이용하여 내부 클래스를 통해 인스턴스를 생성 시킴으로써 스레드 간의 동기화 문제 해결 )
```
public class ExampleClass {

    //private construct
    private ExampleClass() {}

    private static class InnerInstanceClazz() {
        private static final ExampleClass instance = new ExampleClass();
    }

    public static ExampleClass getInstance() {
        return InnerInstanceClazz.instance;
    }
}

```

* 다른 언어에서의 사용

    1. Python : https://wikidocs.net/3693
    2. PHP : https://stackoverflow.com/questions/203336/creating-the-singleton-design-pattern-in-php5/203359#203359
    3. JS :  https://blog.mgechev.com/2014/04/16/singleton-in-javascript/
