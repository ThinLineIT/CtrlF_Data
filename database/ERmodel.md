# Entity Relation Model
<!--Table of Contents-->
- Outline of the ER model
    - Explanation
- Entity Sets
    - Entity and Entity Sets
- Relationship Sets
    - Relation and Relationship Sets
- Attributes
    - Composite attributes
    - Multivalued attributes
    - Derived attributes
    
<!-- 어떤 질문을 대답할 수 있어야 하는지-->
## You can answer
- You can answer structure of ER model

<!--Contents-->

---
## Outline of the ER model
### Explanation
    데이터를 구조화해 Attributes, Entity, Relation으로 표현한 데이터 모델이다
    일반적으로, 데이터베이스를 디자인할 때 사용되고, 설계도라고 생각할 수 있다

## Entity Sets
### Entity and Entity Sets
![](https://user-images.githubusercontent.com/70050038/117546228-57220e80-b064-11eb-93f0-10ca82ed9d9c.png)

    Entity(개체)는 특정한 학생, 교수, 회사 등을 가리킨다
    위 사진의 예시에서, 개체(instructor)는 ID, name, salary등의 정보를 가지고 있다
    여기에서, ID, name, salary는 attribute이고 밑줄이 그어진 ID는 key attribute이다

    이러한 개체들의 집합을 Entity Sets이라고 한다

## Relationship Sets
### Relation and Relationship Sets
![](https://user-images.githubusercontent.com/70050038/117546350-f6df9c80-b064-11eb-8b49-4b13a3b786db.png)

    Relation(관계)은 개체들 간에 관계를 나타낸다
    위 사진의 예시에서, instructor의 특정한 개체와 student의 특정한 개체가 맺은 advisor가 Relation이 될 수 있다

![](https://user-images.githubusercontent.com/70050038/117546419-4a51ea80-b065-11eb-8dff-911b3868f8e4.png)

    위 사진에서 Crick과 Tanaka가 맺은 관계가 Relation이 되는 것이고, 
    이러한 관계들의 집합이 Relationship Sets이다


## Attributes
- Attributes(속성) : 개체를 구성하는 요소들
### Composite attributes
![](https://user-images.githubusercontent.com/70050038/117546695-9fdac700-b066-11eb-85ad-73ac4ed63c48.png)

    위 사진을 보면, name은 first_name, middle_initial, last_name 세가지로 나뉘는 데,
    이러한 속성을 Composite attributes라고 한다
    
### Multivalued attributes
    두개 이상으로 구성될 수 있는 속성
    예를 들어, 어떤 사이트에 회원가입을 할 때, 전화번호를 두개 이상 입력할 수 있으면
    전화번호는 Multivalued sttributes가 된다
### Derived attributes
    다른 속성으로 부터 계산 될 수 있는 속성
    예를 들어, 나이라는 속성이 생일이라는 속성으로부터 계산 될 수 있으면
    나이는 Derived attributes가 된다

---
## Reference
- [www.db-book.com](https://www.db-book.com/)

