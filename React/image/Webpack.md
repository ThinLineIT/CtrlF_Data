# Webpack

- 웹팩의 개념
- 웹팩에서의 모듈
- 웹팩이 필요한 이유
- 웹팩으로 해결하려는 문제

## You can answer
- 웹팩의 개념을 알 수 있다.
- 웹팩의 등장 배경과 이를 통해 어떤 문제점을 해결하는지 알 수 있다.

---
### Webpack이란?
    - 모듈 번들러(Module Bundler)이라고도 한다.
    - 웹 애플리케이션을 구성하는 자원(JS,CSS,HTML,Images 등)을 모두 각각 모듈로 보고 이를 조합해 병합된 하나의 결과물을 만드는 도구이다.

### 모듈(Module)
    프로그래밍 관점에서 특정 기능을 갖는 작은 코드 단위이다.

- 예시)
    ```javascript
    // Math.jsd
    function plus(a,b){return a+b;}
    function minus(a,b){return a-b;}
    const pi = 3.14;
    export {plus, minus, pi}
    ```
    Math.js는 아래와 같은 기능을 가지는 모듈이다.   
      1. 두 수의 합을 구하는 add()   
      2. 두 수의 차를 구하는 minus()   
      3. 원주율 값을 갖는 pi 상수

### 웹팩에서의 모듈
    위에서 javascript에만 국한되지 않고 HTML, CSS, Javascript, Images, Font 등 웹 어플리케이션을 구성하는 파일 각각을 의미한다.
<br/>

### 웹팩이 필요한 이유
- 파일 단위의 자바스크립트 모듈 관리의 필요성   
  ```
  아래와 같이 2개의 js파일을 로딩하여 스크립트를 실행하면 '20'의 결과를 얻는다.    
  즉, 복잡한 애플리케이션을 개발할 때, 변수의 이름을 모두 기억하지 않는 이상 변수를 중복 선언하거나 의도치 않는 값을 할당할 수 있는 현상을 초래한다.
  ```


  ```html
  <!-- index.html -->
  <html>
      <head></head>
      <body>
          <script src="./app.js"></script>
          <script src="./main.js"></script>
          <script>getNum();</script>
      </body>
  </html>
  ```

  ```javascript
    // app.js
    var num = 10;
    
    function getNum(){
        console.log(num); // 10
    }
  ```
  ```javascript
    // main.js
    var num = 20;
    
    function getNum(){
        console.log(num); // 20
    }
  ```


- 웹 개발 작업 자동화 도구(Web Task Manager)
    ```
  과거, 웹 개발 업무를 하면서 가장 많이 반복하는 작업은 텍스트편집기에서 코드를 수정/저장하고 브라우저에서 새로고침을 누르는 것이었고 그래야지만 화면에 내용을 볼수 있었다.  
    ```
- 웹 애플리케이션의 빠른 속도와 높은 성능
  ```
  웹 사이트의 로딩 속도를 높이기 위해 브라우저에서 서버로 요청하는 파일 숫자를 줄이는 방법이 대표적이다.
  또한, Lazy Loading이 등장하면서 나중에 필요한 자원들은 나중에 요청하는 개념이 확산되었다. 
  ```

<br/>

### 웹팩으로 해결하려는 문제
- Javascript 변수 유효 범위 문제

        ES6의 Modules의 문법, 웹팩 모듈 번들링으로 해결할 수 있게됨.
- 브라우저별 HTTP 요청 숫자의 제약

        TCP스펙에 따라 브라우저에서 한번에 서버로 보낼 수 있는 HTTP요청 숫자는 제약이 있다.
        따라서, Webpack을 이용하면 여러 개의 파일을 하나로 합치고 HTTP 요청 숫자 제약을 피할 수 있다.
- Dynamic Loading / Lazy Loading 미지원

        Webpack의 코드분할 기능을 이용해 모듈을 원하는 타이밍에 로딩할 수 있다.

<br/>

# References
- [javascript 문제점 예제](https://codesandbox.io/s/dreamy-beaver-jv33j?file=/index.html)   
- [ES6의 Modules문법](https://babeljs.io/docs/en/learn#introduction)
- [브라우저 HTTP 요청 숫자 제약](https://medium.com/@syalot005006/%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80-http-%EC%B5%9C%EB%8C%80-%EC%97%B0%EA%B2%B0%EC%88%98-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0-3f7aa1453bc2)