# REST API
<!--Table of Contents-->
- REST API란?
- REST API 설계 규칙
- RESTful이란?

## You Can Answer
- REST API란 무엇인가?
- RESTful이란 무엇인가?
---

## REST API란?
REST 아키텍쳐 스타일을 따르는 API로 웹상에서 사용되는 여러 자원을 HTTP URI로 표현하고, 해당 자원에 대한 행위를 HTTP 메소드로 정의하는 방식을 말한다.

## REST API 설계 규칙

### REST API 설계 기본 규칙
REST API 설계 시 가장 기본적으로 지켜야 하는 항목은 다음과 같이 2가지가 있다.
1. URI는 정보의 자원(Resource)을 표현해야 한다.
    - 자원의 이름은 동사보다는 명사를, 대문자보다는 소문자를 사용한다.
      ```Json
        GET /Members/1 (x)
        GET /members/1 (o)
      ```
2. 자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE 등)로 표현한다.
    - URI에 HTTP 메소드가 들어가면 안된다.
      ```Json
        GET /members/delete/1 (x)
        DELETE /members/1 (o)
      ```
    - URI에 행위헤 대한 동사 표현이 들어가면 안된다.
      ```Json
        GET /members/show/1 (x)
        GET /members/1 (o)
      ```
    - 경로 부분 중 변하는 부분은 고유 값으로 대체한다.
      * ex) id=12인 student를 삭제하는 route: DELETE /students/12

### URI 설계시 주의할 점
1. 슬래시 구분자(/)는 계층 관계를 나타내는 데 사용한다.
  ```
    http://restapi.example.com/houses/apartments
    http://restapi.example.com/animals/mammals/whales
  ```
2. URI 마지막 문자로 슬래시(/)를 포함하지 않는다.
    - URI에 포함되는 모든 글자는 리소스의 유일한 식별자로 사용되어야 하며 URI가 다르다는 것은 리소스가 다르다는 것이고, 역으로 리소스가 다르면 URI도 달라져야 한다.
    REST API는 분명한 URI를 만들어 통신을 해야 하기 때문에 혼동을 주지 않도록 URI 경로의 마지막에는 슬래시(/)를 사용하지 않는다.
  ```
    http://restapi.example.com/houses/apartments/ (X)
    http://restapi.example.com/houses/apartments  (0)
  ```

3. 하이픈(-)은 URI 가독성을 높이는데 사용한다.
    - URI를 쉽게 읽고 해석하기 위해, 불가피하게 긴 URI경로를 사용하게 된다면 하이픈을 사용해 가독성을 높일 수 있다.

4. 밑줄(_)은 URI에 사용하지 않는다.
    - 글꼴에 따라 다르긴 하지만 밑줄은 보기 어렵거나 밑줄 때문에 문자가 가려지기도 한다. 이러한 문제를 피하기 위해 밑줄 대신 하이픈(-)을 사용하는 것이 좋다.

5. URI 경로에는 소문자가 적합하다.
    - URI 경로에 대문자 사용은 피해야한다. 대소문자에 따라 다른 리소스로 인식하게 되기 때문이다.
    RFC 3986(URI 문법 형식)은 URI 스키마와 호스트를 제외하고는 대소문자를 구별하도록 규정하기 때문이다.

6. 파일 확장자는 URI에 포함시키지 않는다.
    - REST API에서는 메시지 바디 내용의 포맷을 나타내기 위한 파일 확장자를 URI 안에 포함시키지 않는다.
    Accept header를 사용한다.
    ```
    http://test.com/test.jpg (X)

    http://test.com/test (o)
    GET /test HTTP/1.1
    Host: testapi.com
    Accept: image/jpg
    ```

### 리소스 간의 관계 표현 방법
REST 리소스 간에는 연관 관계가 있을 수 있고, 이런 경우 다음과 같은 표현방법으로 사용한다.
```
  /리소스명/리소스 ID/관계가 있는 다른 리소스명

  GET : /users/{userid}/devices (일반적으로 소유 ‘has’의 관계를 표현할 때)
```
관계명이 복잡하다면 이를 서브 리소스에 명시적으로 표현하는 방법이 있다. 예를 들어 사용자가 ‘좋아하는’ 디바이스 목록을 표현해야 할 경우 다음과 같은 형태로 사용될 수 있다.
```
   GET : /users/{userid}/likes/devices (관계명이 애매하거나 구체적 표현이 필요할 때)
```

### 자원을 표현하는 Collection과 Document
Document : 객체와 유사한 개념의 리소스이다.
Collection : Document들의 집합으로 서버에서 관리하는 디렉토리 리소스이다.
```
http:// restapi.example.com/sports/soccer/players/13
```
위 URI는 sports, players 컬렉션과 soccer,13(13번 선수)를 의미하는 도큐먼트로 이루어져 있다.
여기서 중요한 점은 컬렉션은 복수로 사용되고 있다.
컬렉션과 도큐먼트 사용 시 단수, 복수 규칙도 지켜준다면 조금 더 직관적인 REST API 설계가 가능하다.

### HTTP 응답 상태 코드
|상태코드|설명|
|---|---|
|200|클라이언트의 요청을 정상적으로 수행함|
|201|클라이언트가 어떠한 리소스 생성을 요청, 해당 리소스가 성공적으로 생성됨(POST)|

|상태코드|설명|
|---|---|
|400|클라이언트의 요청이 부적절 할 경우 사용하는 응답 코드|
|401|클라이언트가 인증되지 않은 상태에서 보호된 리소스를 요청했을 때 사용하는 응답 코드|
|403|유저 인증상태와 관계 없이 응답하고 싶지 않은 리소스를 클라이언트가 요청했을 때 사용하는 응답 코드|
|404|클라이언트가 요청한 리소스에서는 사용 불가능한 Method를 이용했을 경우 사용하는 응답 코드|

|상태코드|설명|
|---|---|
|301|클라이언트가 요청한 리소스에 대한 URI가 변경 되었을 때 사용하는 응답 코드|
|500|서버에 문제가 있을 경우 사용하는 응답 코드|

## RESTful이란?
  - RESTful은 일반적으로 REST 아키텍처 스타일을 따르는 웹 서비스를 나타내기 위해 사용되는 용어이다.
  - REST 아키텍처 스타일을 따르는 서비스는 RESTful이란 용어로 지칭된다.

---
## Reference
[REST란? REST API란? RESTful이란?](https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html)
[REST API 제대로 알고 사용하기](https://meetup.toast.com/posts/92)
