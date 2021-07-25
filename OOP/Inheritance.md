# Inheritance(상속)
## 목차
- Inheritance 개념.
- Inheritance 사용방법 및 예제.

## You can answer
- 상속이란 무엇입니까?
- 상속을 하는 이유는 무엇입니까?
- 상속의 장점은 무엇입니까?
- 상속을 어떻게 활용합니까?

---
## Inheritance 개념
### Inheritance 란?
    상속이란 상위 클래스의 모든 멤버를 하위 클래스가 물려 받는 것.

### 상속 관련 용어
- (부모||기반||상위) 클래스 - 기존에 정의 되어있는 클래스.
- (자식||파생||하위) 클래스 - 부모||기반||상위 클래스를 선택하여 상속을 통해 새롭게 작성되는 클래스.
<img src = "img/Parent&Child_Class.png" />

### 상속의 이유 & 장점 & 주의 사항
[이유]
- 객체지향의 목적 중 하나는 유기적인 상호작용이고, 객체들간의 유기적인 관계를 형상하게 해주는것이 바로 상속이다.

- 또한 단순한 기능의 확장측면으로 사용하는것이 아닌 객체들을 분류하는 수단이기 때문!!

예를들어 나이와 이름의 데이터를 갖는 사람, 강아지 객체가 있다고 하자!
사람 객체를 상속받는 객체는 사람이라는 특성을 알 수 있게 되고, 강아지 객체를 상속받는 객체는 강아지라는 특성을 알 수 있게 되어 상속을 통해 객체를 분류 할 수있게 된다!

[장점(효과)]
- 이미 정의 되어있는 클래스를 재사용하여 만들기 때문에 효율적이다.
- 이미 정의 된 코드를 재사용하여 개발 시간을 줄여준다!!

[주의사항]
- 부모 클래스의 private 접근 제한을 갖는 필드, 메소드는 자식이 물려받을 수 없기 때문에 클래스의 접근권한을 잘 살펴보자!!
- 부모와 자식 클래스가 서로 다른 패키지에 있다면, 부모의 default 접근 제한을 갖는 필드 및 메소드도 자식이 물려받을 수 없다.(default 접근 제한은 '같은 패키지에 있는 클래스'만 접근 가능.)
- Java 같은 경우 다중상속을 허용하지 않으므로 하나의 부모클래스에서만 상속받을 수 있다.
## Inheritance 사용방법 및 예제
[사용방법]
- 상속받고자 하는 자식 클래스명 옆에 __extends__ 키워드를 붙이고, 상속할 __부모 클래스명__ 을 적는다.
- 부모클래스의 변수나 메소드, 생성자에 접근할 땐 super라는 키워드를 사용한다.

[예제]
``` java
//부모클래스
class ParentClass {
	int age = 60;
	String name = "Parent";

	public ParentClass() {
		System.out.println("Parent Default Constructor");
	}

	public ParentClass(int age, String name) {
		//매개변수를 클래스변수에 할당.
		showInfo();
		this.age = age;
		this.name = name;
		System.out.println("Parent Constructor");
	}

	public void showInfo() {
		System.out.println("I'm Parent! My Age : " + age + " Name : "+name);
	}

}

//정의 되어있는 ParentClass를 상속받음.
class ChildClass extends ParentClass{

	//자식클래스의 디폴트 생성자.
	public ChildClass() {
		System.out.println("Child Default Constructor");
	}

	public ChildClass(int age, String name) {
		//super 키워드를 사용하여 부모생성자를 호출.
		super(age,name);
		System.out.println("Child Constrcutor");
		System.out.println("I'm Child! My Age : " + age + " Name : "+name);
	}

	//부모클래스의 메소드를 자식클래스에서 재정의.
	public void showInfo() {
		System.out.println("Child Override showInfo!!");
		super.showInfo();
	}

}


public class Inheritance {

	public static void main(String[] args) {
		ChildClass child = new ChildClass(25,"HONG");
		child.showInfo();
	}
}

```

[예제결과]

<img src = "img/Inheritance_Result.png" width = "60%"/>

---
## Reference
- [상속 개념](https://chanhuiseok.github.io/posts/java-1/)
- [상속 예제](https://reakwon.tistory.com/27)
