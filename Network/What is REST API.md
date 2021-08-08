# REST API
- REST 란?
- REST의 장단점
- REST API란?
- REST API의 특징
- REST API 설계 규칙
- RESTful이란?
- RESTful의 목적

## You can answer
- REST가 무엇인가?
- REST API가 무엇인가?
- RESTful란?
-----------------------
## REST 란?
- "Representational State Transfer"의 약자
- 자원을 이름으로 구분하여 해당 자원의 상태(정보)를 주고 받는 모든 것을 의미한다.
- 즉, 자원(Resource)의 표현(representation)에 의한 상태 전달
- HTTP URI를 통해 자원을 명시하고, HTTP Method(POST, GET, PUT, DELETE)를 통해 해당 자원에 대한 CRUD Operation을 적용하는 것을 의미한다.
- CRUD Operation
  - CREATE : 생성(POST)
  - READ : 조회(GET)
  - UPDATE : 수정(PUT)
  - DELETE : 삭제(DELETE)
  - HEAD : header 정보 조회(HEAD)

</br>

## REST의 장단점
- 장점
  - HTTP 프로토콜의 인프라를 그대로 사용하므로 REST API 사용을 위한 별도의 인프라를 구축할 필요가 없다.
  - HTTP 프로토콜의 표준을 최대한 활용하여 여러 추가적인 장점을 함께 자겨갈 수 있게 해준다.
  - HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용이 가능하다.
  - REST API 메시지가 의도하는 바를 명확하게 나타내므로 의도하는 바를 쉽게 파악할 수 있다.
  - 여러가지 서비스 디자인에서 생길 수 있는 문제를 최소화 한다.
  - 서버와 클라이언트의 역활을 명확하게 분리한다.

- 단점
  - 표준이 존재하지 않는다.
  - 사용할 수 있는 메소드가 4가지 밖에 없다.
  - 구형 브라우저가 아직 제대로 지원해주지 못하는 부분이 존재한다.(PUT, DELETE를 사용하지 못하는 점, pushState를 지원하지 않는 점)

</br>

## REST API란?
- API(Application Programming Interface)란?
데이터와 기능의 집합을 제공하여 컴퓨터 프로그램간 상호작용을 촉직하며, 서로 정보를 교환가능 하도록 하는 것
- REST API의 정의
REST 기반으로 서비스 API를 구현한 것. </br>
최근 OpenAPI, 마이크로 서비스 등을 제공하는 업체 대부분은 REST API를 제공한다.

</br>

## REST API의 특징
- 사내 시스템들도 REST 기반으로 시스템을 분산해 확장성과 재사용성을 높여 유지보수 및 운용을 편리하게 할 수 있다.
- REST는 HTTP 표준을 기반으로 구현하므로, HTTP를 지원하는 프로그램 언어로 클라이언트, 서버를 구현할 수 있다.
- 즉, REST API를 제작하면 델파이 클라이언트 뿐 아니라, 자바, C#, 웹 등을 이용해 클라이언트를 제작할 수 있다. 

</br>

## REST API 설계 규칙
1. 슬래시 구분자(/)는 계층 관계를 나타내는데 사용한다.</br>
  Ex) http://restapi.example.com/houses/apartments
2. URI 마지막 문자로 슬래시(/)를 포함하지 않는다.
  - REST API는 분명한 URI를 만들어 통신을 해야 하기 때문에 혼동을 주지 않도록 URI 경로의 마지막에는 슬래시(/)를 사용하지 않는다.</br>
  Ex) http://restapi.example.com/houses/apartments/ (X)
3. 하이픈(-)은 URI 가독성을 높이는데 사용
4. 밑줄(_)은 URI에 사용하지 않는다.
5. URI 경로에는 소문자가 적합하다.
6. 파일 확장자는 URI에 포함하지 않는다.
7. 리소스 간에는 연관 관계가 있는 경우 /리소스명/리소스ID/관계가 있는 다른 리소스명</br>
   Ex) GET : /users/{userid}/devices

</br>

## RESTful이란?
- RESTful은 일반적으로 REST라는 아키텍처를 구현하는 웹 서비스를 나타내기 위해 사용되는 용어이다.
- REST API를 제공하는 웹 서비스를 RESTful하다고 할 수 있다.
- RESTful은 REST를 REST답게 쓰기 위한 방법으로 누군가가 공식적으로 발표한 것이 아니다.
- 즉, REST 원리를 따르는 시스템은 RESTful이란 용어로 지칭된다.

</br>

## RESTful의 목적
- 이해하기 쉽고 사용하기 쉬운 REST API를 만드는 것
- RESTful한 API를 구현하는 근본적인 목적이 성능 향상에 있는 것이 아니라 일관적인 컨벤션을 통한 API의 이해도 및 호환성을 높이는 것이 주 동기이니, 성능이 중요한 상황에서는 굳이 RESTful한 API를 구현할 필요는 없다.

</br>

## Reference
- https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html