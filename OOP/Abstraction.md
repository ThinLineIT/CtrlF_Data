# Abstraction(추상화)

## 목차
- Abstraction의 개념
- Abstraction 실제 코드 예시




## You can answer
- Abstraction를(을) 사용하는 이유는 무엇인가요?
- Abstraction를(을) 어떻게 사용하나요?

---
## Abstraction(추상화) 개념
### Abstraction 이란?
    사전적 의미 : 여러가지 사물이나 개념에서 공통되는 특성이나 속성따위를 추출하여 파악하는 적용.

    OOP(객체지향 프로그래밍)에서 객체는 객체가 어떤 데이터를 갖는지에 대한 field와 객체가 무슨일을 하는지에 대한 method를 가지고 있다.

    이때 추상화는 각 객체가 가지고 있는 공통적인 데이터나 method 등 관심있는 특성만을 가지고 재조합하는것을 의미한다.

- 추상화를 적절히 시키면 코드의 재사용성, 가독성을 높이고, 생산성, 이후 에러의 감소와 같은 요소에 영향을 미치게 된다.
- 추상화를 통해 객체지향의 큰 장점 중 하나인 유지보수에도 용이 하다.

### Abstraction 간단 예시


이제 간단한 예시를 보며 이해를 해보자!!

<img src="img/DogCatTiger.png" width='50%' height='100%'/>

- 개,고양이,호랑이(객체)가 존재한다고 하자!
- 개,고양이,호랑이(자식객체) 는 동물(부모객체)이라는 특성을 포함하고 있다. 따라서 동물이라는 공통속성으로 묶을 것이다!(추상화)
- 동물의 큰 특징은 '움직인다'라는 특성이 있다.

- 동물은 소리를 내고 공격을 한다, 하지만 '동물'이라 함은 명확하게 어떻게 소리를내고 공격을 하는지 하나로 정의할 수 는 없다.
- 따라서 소리,공격이라는 특성만 정의를 해놓고, 동물에 해당하는 개,고양이,호랑이(자식객체들)의 소리,공격방식을 각자의 특성에 맞게 재정의 해준다.
- 동물인 개는 "월월" 소리를 내며, 공격을 할 때에는 이빨을 이용해 공격한다.(메소드 재정의)
- 동물인 고양이는 "에옹' 소리를 내며, 공격을 할 때에는 주먹을 이용해 공격한다.(메소드 재정의)

+ 동물에 해당되는 특징을 각각의 동물에서 표현을 하게 된다면 동물에서 이미 정의된 '움직인다'를 사용하여 표현할 수 있게된다.(코드의 재사용성,상속)




## Abstraction 실제 코드 예시

```java

//추상클래스 Animal
public abstract class Animal{
    public void animal(){
        system.out.println("동물은 움직입니다!");
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

    private string name;
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

    private string name;
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
<img src="img/Abstraction_Code_Result.png" width='50%' height='100%'/>

---
## Reference
- [OOP 추상화 장점](https://shutcoding.tistory.com/16)
- [추상화 예시](https://programmingnote.tistory.com/28)
