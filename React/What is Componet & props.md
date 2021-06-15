# Component & Props
- 컴포넌트 정의
- 컴포넌트 종류 및 형태
- props 정의
- 예시
  
# You can answer
- 컴포넌트의 개념과 종류를 알 수 있다.
- prop의 개념과 사용법을 알 수 있다.
- 컴포넌트의 렌더링 방식을 알 수 있다.
---
## Component
- 정의
  - 기능을 단위별로 캡슐화한 것을 의미한다.
  - 재사용과 확장이 가능하다.
  - 대문자로 시작 해야한다.  
- 종류
  - 함수형 컴포넌트
   ```javascript 
   function Welcome(props){
       return <h1> Hello, {props.name}</h1>
   }
   ```
    - 클래스형 컴포넌트
    ``` javascript
    class Welcome extends React.Component{
        render(){
            return <h1>Hello, {this.props.name}</h1>
        }
    }
    ```

## Props(Properties)
- 정의
  - Immutable Data로 선언 후 직접 수정할 수 없는 데이터이다.
  - 부모 컴포넌트에서 자식 컴포넌트로 데이터를 넘길때 사용한다.

<br/>

## 예시
- 함수형 컴포넌트 렌더링

    ```javascript 
    function FnWelcome(props){
        return <h1> Hello, {props.name}</h1>
    }

    const element = <FnWelcome name="Sara"/>;
    
    ReactDOM.redner(
        element,
        document.getElementById('root)
    );
   ```
- 클래스형 컴포넌트 렌더링
    ```javascript
    class ClassWelcome extends React.Component{
        render(){
            return <h1>Hello, {this.props.name}</h1>
        }
    }
    const element = <ClassWelcome name="Jack"/>;
    ReactDOM.redner(
        element,
        document.getElementById('root)
    );
    ```
- 작동 순서   
    1. 아래 엘리먼트로 ReactDOM.render() 호출
        ```css
            <fnWelcome name='Sara'/> or <classWelcome name='Jack'/>
        ```
    2. React는 {name:'Sara'} or {name:'Jack'}를 props로 하여 각 컴포넌트를 호출
    3. fnWelcome 컴포넌트는 Hello, Sara 출력, classWelcome 컴포넌트는 Hello, Jack 출력함
    4. ReactDOM은 각 element와 일치하도록 DOM을 효율적으로 업데이트 수행

<br/>

# Reference
- [Props 세부설명](https://medium.com/@yeon22/react-js-react-js%EC%9D%98-props-%EC%82%AC%EC%9A%A9%EB%B0%A9%EB%B2%95-bc59a5c257a)
- [render 세부설명](https://ko.reactjs.org/docs/react-dom.html#render)