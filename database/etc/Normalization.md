# 데이터 베이스 정규화
<!--Table of Contents-->

<!-- 어떤 질문을 대답할 수 있어야 하는지-->
## You can answer
- 데이터 베이스 정규화란?
- 1NF, 2NF, 3NF, BCNF 를 설명해주세요.

<!--Contents-->

---
<br>

## 정규화란?

<br>

    정규화(Normalization)의 기본 목표는 테이블 간에 중복된 데이타를 허용하지 않는다는 것이다. 중복된 데이터를 허용하지 않음으로써 무결성(Integrity)를 유지할 수 있으며, DB의 저장 용량 역시 줄일 수 있다.

이러한 테이블을 분해하는 정규화 단계가 정의되어 있는데, 여기서 테이블을 어떻게 분해되는지에 따라 정규화 단계가 달라지는데, 각각의 정규화 단계에 대해 자세히 알아보도록 하자. 

<br>
<br>

## [제 1 정규화]
    제1 정규화란 테이블의 컬럼이 원자값(Atomic Value, 하나의 값)을 갖도록 테이블을 분해하는 것이다. 

예를 들어 아래와 같은 고객 취미 테이블이 존재한다고 하자. 

<br>
<br>

![image](https://user-images.githubusercontent.com/70083982/123519711-f1f3ac80-d6e7-11eb-8960-394b428c66cd.png)


위의 테이블에서 추신수와 박세리는 여러 개의 취미를 가지고 있기 때문에 제1 정규형을 만족하지 못하고 있다. 그렇기 때문에 이를 제1 정규화하여 분해할 수 있다. 제1 정규화를 진행한 테이블은 아래와 같다.

<img width="252" alt="스크린샷 2021-06-27 오전 1 36 41" src="https://user-images.githubusercontent.com/70083982/123519749-210a1e00-d6e8-11eb-8ec4-f9431544c8e1.png">

<br>
<br>
<br>
<br>

## [제 2 정규화]
    제2 정규화란 제1 정규화를 진행한 테이블에 대해 완전 함수 종속을 만족하도록 테이블을 분해하는 것이다. 여기서 완전 함수 종속이라는 것은 기본키의 부분집합이 결정자가 되어선 안된다는 것을 의미한다.

예를 들어 아래와 같은 수강 강좌 테이블을 살펴보자. 

<img width="836" alt="스크린샷 2021-06-27 오전 1 37 51" src="https://user-images.githubusercontent.com/70083982/123519786-4b5bdb80-d6e8-11eb-99a7-973f8deae28c.png">

<br>
<br>

이 테이블에서 기본키는 (학생번호, 강좌이름)으로 복합키이다. 그리고 (학생번호, 강좌이름)인 기본키는 강의실을 결정하고 있다.  (학생번호, 강좌이름) --> (강의실)
그런데 여기서 강의실이라는 컬럼은 기본키의 부분집합인 강좌이름에 의해 결정될 수 있다. (강좌이름) --> (강의실)

또한 성적이라는 컬럼은 기본키의 부분집합인 학생번호에 의해 결정될 수 있다. (학생번호) --> (성적)
그렇기 때문에 위의 테이블의 경우 다음과 같이 기존의 테이블에서 강의실을 분해하여 별도의 테이블로 관리하여 제2 정규형을 만족시킬 수 있다.

<br>
<br>


<img width="830" alt="스크린샷 2021-06-27 오전 1 38 39" src="https://user-images.githubusercontent.com/70083982/123519805-67f81380-d6e8-11eb-9d19-9f8beb65a0cb.png">

<br>
<br>
<br>
<br>

## [제 3 정규화]
    제3 정규화란 제2 정규화를 진행한 테이블에 대해 이행적 종속을 없애도록 테이블을 분해하는 것이다. 여기서 이행적 종속이라는 것은 A -> B, B -> C가 성립할 때 A -> C가 성립되는 것을 의미한다.
예를 들어 아래와 같은 계절 학기 테이블을 살펴보자. 


<img width="831" alt="스크린샷 2021-06-27 오전 1 40 30" src="https://user-images.githubusercontent.com/70083982/123519847-aa215500-d6e8-11eb-8150-4173f5959afc.png">

기존의 테이블에서 학생 번호는 강좌 이름을 결정하고 있고, 강좌 이름은 수강료를 결정하고 있다. 그렇기 때문에 이를 (학생 번호, 강좌 이름) 테이블과 (강좌 이름, 수강료) 테이블로 분해해야 한다. 

이행적 종속을 제거하는 이유는 비교적 간단하다. 예를 들어 501번 학생이 수강하는 강좌가 스포츠경영학으로 변경되었다고 하자. 이행적 종속이 존재한다면 501번의 학생은 스포츠경영학이라는 수업을 20000원이라는 수강료로 듣게 된다. 물론 강좌 이름에 맞게 수강료를 다시 변경할 수 있지만, 이러한 번거로움을 해결하기 위해 제3 정규화를 하는 것이다.
즉, 학생 번호를 통해 강좌 이름을 참조하고, 강좌 이름으로 수강료를 참조하도록 테이블을 분해해야 하며 그 결과는 다음의 그림과 같다

<br>
<br>
<br>
<br>



## [BCNF 정규화]

    BCNF 정규화란 제3 정규화를 진행한 테이블에 대해 모든 결정자가 후보키가 되도록 테이블을 분해하는 것이다. 예를 들어 다음과 같은 특강수강 테이블이 존재한다고 하자.

 <img width="832" alt="스크린샷 2021-06-27 오전 1 41 32" src="https://user-images.githubusercontent.com/70083982/123519878-cde49b00-d6e8-11eb-8400-a039f193bb66.png"> 


<br>
<br>

특강수강 테이블에서 기본키는 (학생번호, 특강이름)이다. 그리고 기본키 (학생번호, 특강이름)는 교수를 결정하고 있다. 또한 여기서 교수는 특강이름을 결정하고 있다.
그런데 문제는 교수가 특강이름을 결정하는 결정자이지만, 후보키가 아니라는 점이다. 그렇기 때문에 BCNF 정규화를 만족시키기 위해서 위의 테이블을 분해해야 하는데, 다음과 같이 특강신청 테이블과 특강교수 테이블로 분해할 수 있다.


<img width="737" alt="스크린샷 2021-06-27 오전 1 42 45" src="https://user-images.githubusercontent.com/70083982/123519917-f9678580-d6e8-11eb-9595-dffd265f9d87.png">