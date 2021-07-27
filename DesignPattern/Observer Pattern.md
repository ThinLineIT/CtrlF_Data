# Observer Pattern
<!--Table of Contents-->
- 주체객체 & 관찰객체
- 예시
- 구현
    - in java
    - in kotlin

<!-- 어떤 질문을 대답할 수 있어야 하는지-->
## You can answer
- 옵저버 패턴의 특징
- 옵저버 패턴의 구현

<!--Contents-->

---
## Observer Pattern
* 상태를 가지고 있는 주체 객체와 상태의 변경을 알아야하는 관찰 객체로
1 대 1  혹은 1 대 N의 관계를 가짐.
* 디자인 패턴의 행동(behavior) 패턴으로 분류됨.
* 서로의 정보를 주고받는 과정에서 정보의 단위가 클수록, 객체들의 규모가 클수록 복잡성이 증가하게 됨. 이때 가이드 라인을 제시해줄 수 있는 것이 '옵저버 패턴'
### 주체 객체 & 관찰 객체 
    잡지사 : 구독자 혹은 우유배달업체 : 고객 과 같은 관계를 가짐 
구독자, 고객들은 정보를 얻거나 받아야하는 주체와 관계를 형성하며, 관계를 해제할 수도 있음

### 예시 
 
다음은 매일 새로운 뉴스를 제공하는 뉴스머신과 뉴스를 구독하는 구독자들로 이루어져있을 때의 관계를 나타낸 다이어그램.
옵저버 패턴에서는 인터페이스를 이용하여 객체 간의 느슨한 결합을 권장함.
![observer_diagram](./img/diagram_observer.jpg)

### 구현

1. Observer 인터페이스  
이 메서드는 주체 객체에서 변화가 발생할 때마다 실행됨. 
```java
public interface Observer { 
    public void update(String title, String news); 
}
```

2. Publisher 인터페이스  
   Observer들을 관리하는 메소드.
```java
public interface Publisher {
    public void add(Observer observer); 
    public void delete(Observer observer); 
    public void notifyObserver(); 
}
```
3. NewsMachine 클래스  
publisher를 구현한 클래스로 정보를 제공함.
```java
public class NewsMachine implements Publisher { 
    private ArrayList<Observer> observers; 
    private String title; 
    private String news;
    
    public NewsMachine() { 
        observers = new ArrayList<>(); 
    } 
    
    @Override 
    public void add(Observer observer) { 
        observers.add(observer); 
    } 
    
    @Override 
    public void delete(Observer observer) {
        int index = observers.indexOf(observer); 
        observers.remove(index); 
    }
    
    @Override 
    public void notifyObserver() { 
        for(Observer observer : observers) { 
            observer.update(title, news); 
        } 
    } 
    
    public void setNewsInfo(String title, String news) { 
        this.title = title; 
        this.news = news; 
        notifyObserver(); 
    } 
    
    public String getTitle() { 
        return title; 
    } 
    
    public String getNews() { 
        return news; 
    } 
}
```
4. AnnualSubscriber 클래스  
Observer를 구현한 클래스로, notifyObserver()를 호출하면서 알려줄 때 마다 Update 호출됨
```java
public class AnnualSubscriber implements Observer { 
    private String newsString; 
    private Publisher publisher; 
    public AnnualSubscriber(Publisher publisher) { 
        this.publisher = publisher; 
        publisher.add(this); 
    } 
    
    @Override 
    public void update(String title, String news) { 
        this.newsString = title + " \n -------- \n " + news; display(); 
    } private void display() { 
        System.out.println("\n\n오늘의 뉴스\n============================\n\n" + newsString); 
    } 
}
```

**Implementation in kotlin**

코틀린에서는 코틀린에 내장된 위임 속성(Delegated Properties)을 사용하여 다른 방법으로 구현 할 수 있음.
위임된 속성은 클래스 필드에서 백업되는 것이 아니라 지정된 속성을 읽거나 쓸 때 실행되는 콜백 함수를 반환함.
위임된 기능들을 추상화해서 비슷한 여러 속성들간에 공유를 할 수 있음. 

```kotlin
class Newsletter : publisher {
    override val observers: ArrayList<Observer> = ArrayList()
    var newestArticleUrl = ""
        set(value) {
            field = value
            notifyObserver()
        }
}
```
-> setter를 재정의해서 업데이트 하는 방법 
```kotlin
class BaeldungNewsletter {
    val newestArticleObservers = mutableListOf<(String) -> Unit>()

    var newestArticleUrl: String by Delegates.observable("") { _, _, newValue ->
        newestArticleObservers.forEach { it(newValue) }
    }
}
```
-> observer가 lambda 함수의 list로 저장됨. 문서 Url이 delegate에 할당됨.

### 정리 
* 옵저버 패턴은 주체 객체의 상태가 바뀌면 그 객체에 의존하는 관찰 객체들에게  자동으로 정보가 갱신되는 1:N 의 관계를 정의한다.
* 연결은 인터페이스를 이용하여 느슨한 결합성을 유지한다. 주체, 옵저버 인터페이스를 적용한다.
* Swing, Android 등에서 UI관련된 곳에서 이 옵저버 패턴이 많이 사용된다. (물론 이보다 더 많다)



---
## Reference
- [Observer Pattern](https://flowarc.tistory.com/entry/%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4-%EC%98%B5%EC%A0%80%EB%B2%84-%ED%8C%A8%ED%84%B4Observer-Pattern)
- [Observer Patter in Kotlin](https://www.baeldung.com/kotlin/observer-pattern)
- [Delegated Properties](https://www.baeldung.com/kotlin/delegated-properties)