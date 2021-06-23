# Abstraction(추상화)

## 목차
- Abstraction의 개념
    - Abstraction이란?
    - Abstraction의 개념 예시
- Abstraction 실제 코드 예시
- Abstarction 주의사항




## You can answer
- Abstraction를(을) 사용하는 이유는 무엇인가요?
- Abstraction를(을) 어떻게 사용하나요?

---
## Abstraction(추상화) 개념
### Abstraction 이란?
    사전적 의미 : 여러가지 사물이나 개념에서 공통되는 특성이나 속성따위를 추출하여 파악하는 적용.

    OOP(객체지향 프로그래밍)에서 객체는 객체가 어떤 데이터를 갖는지에 대한 field와 객체가 무슨일을 하는지에 대한 method를 가지고 있다.

    이때 추상화는 각 객체가 가지고 있는 공통적인 데이터나 method 등 관심있는 특성만을 가지고 재조합하는것을 의미한다.


### Abstraction 간단 예시
앞에 설명만으로 이해를 한다면 그대는 천재임이 분명하니 뒤로 돌아가도 좋다!!ㅎㅎ

예시를 보기 전에 객체,클래스의 개념을 간단,확실하게 정리하고 가보자!
- 객체 : 세상에 존재하는 유일무이한 사물(예시 : 포유류,파충류,조류,이끼식물,양치식물...등등)
- 클래스 : 같은 속성들과 기능들을 가진 객체들을 총칭하는 개념(예시 :동물 ,식물, 미생물...등등)


이제 간단한 예시를 보며 이해를 해보자!!

부모객체(동물) 자식객체(포유류,조류,어류) 가 있다고 가정하자!

동물이 비행이 가능한가?
동물이 알을 낳을 수 있는가?

라는 질문에 대답할 수 있는가? 동물은 포유류,조류,어류를 구분하는 객체이며



## Abstraction 실제 코드 예시

```java

//추상클래스 Animal
public abstract class Animal{
    public void animal(){
        system.out.println("나는 동물입니다!");
    }
    abstract public void Sound(); //동물의 소리를 명확하게 정의 할 수 없지만 동물은 소리를 낸다! 그렇기 때문에 정의만 해놓는다!
    abstract public void Attack(); //동물의 공격방법을 명확하게 정의 할 수 없지만 동물은 자신을 보호하기 위해 공격한다! 그렇기 때문에 정의만 해놓는다.
}

//추상클래스 Animal을 상속받은 Dog클래스(추상클래스를 상속받았기 때문에 반드시 재정의를 해줘야함.)
public class Dog extends Animal{
    @Override
    public void Sound(){
        system.out.println("월월!!");
    }
    public void Attack(){
        system.out.println("물어뜯기");
    }
}

//추상클래스 Animal을 상속받은 Cat클래스(추상클래스를 상속받았기 때문에 반드시 재정의를 해줘야함.)
public class Cat extends Animal{
    @Override
    public void Sound(){
        system.out.println("에옹");
    }
    public void Attack(){
        system.out.println("냥냥펀치");
    }

    private string
}

//각 동물들의 소리와 공격방식을 알아보는 Test클래스
public class Test{
    public static void main(String[] args){

        Animal an1 = new Dog();
        an1.Sound();
        an1.Attack();

        Animal an2 = new Cat();
        an2.Sound();
        an2.Attack();
    }
}


```
### [실행결과]



## Second
### 라마바
    ㅂㅈㄷㅂㅈㄷㅂㅈㄷ

## Thrid
### 사아자
    ㅂㅈㄷㅂㅈㄷㅂㅈㄷ
### 차카타
    ㅂㅈㄷㅂㅈㄷㅂㅈ

---
## Reference
- [Example Site1](www.google.com)
- [Example Site2](www.google.com)
