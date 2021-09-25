# JSX In Depth


- **Specifying The React Element Type**

    
- **Props in JSX**

    

- **Children in JSX**



-------

## You can answer
- **JSX 의 고급 용법은 무엇인가?**

- **JSX 의 고급 용법은 어떻게 사용하는가?**

  

---


​	**JSX**는 컴포넌트와 해당 컴포넌트에 전달 될 props 및 하위 요소를 전달하기 쉽도록 도와주는 일종의 

**syntactic sugar**의 역할을 하는게 기초적인 내용이다.



## Specifying The React Element Type
### React Must Be in Scope!
    JSX는 React.createElement를 호출하는 코드로 컴파일 되기 때문에 React 라이브러리 역시 JSX 코드와 같은 스코프 내에 존재해야만 합니다.

```javascript
import React from 'react';
import CustomButton from './CustomButton';

function WarningButton() {
  // return React.createElement(CustomButton, {color: 'red'}, null);
  return <CustomButton color="red" />;
}
```

    위 규칙은 React 17릴리스와 함께 개선이 되었습니다. 더이상 React를 import 시키지 않아도 됩니다. 
    하지만 React가 제공하는 Hook이나 다른 내보내기를 사용하려면 React를 가져와야 합니다.
### JSX에서의 점 표기법 사용
    JSX 내에서도 점 표기법을 사용하여 React 컴포넌트를 참조할 수 있습니다. 이 방법은 하나의 모듈에서 복수의 React 컴포넌트들을 export 하는 경우에 편리하게 사용할 수 있습니다.

```javascript
import React from 'react';

const MyComponents = {
  DatePicker: function DatePicker(props) {
    return <div>Imagine a {props.color} datepicker here.</div>;
  }
}

function BlueDatePicker() {
  return <MyComponents.DatePicker color="blue" />;
}
```

### 사용자 정의 컴포넌트의 표기법
    만약 소문자로 Element를 사용한다면 div, span 같은 내장 컴포넌트로 인식합니다. 반드시 대문자로 시작해야 React.createElement(Xxxx)의 형태로 컴파일 되며 JavaScript 파일 내에 사용자가 정의했거나 import 한 컴포넌트를 가리킵니다.
### JSX에서 표현식 사용하기
    React element 타입에 일반적인 표현식은 사용할 수 없습니다. 사용하고 싶다면 대문자로 시작하는 변수에 배정한 후 사용이 가능합니다. 

```javascript
return <components[props.storyType] story={props.story} />;

const SpecificStory = components[props.storyType];
return <SpecificStory story={props.story} />;
```

## Props in JSX
### JavaScript Expressions as Props
    JavaScript 표현식을 {} 안에 넣어서 JSX 안에서 prop으로 사용할 수 있습니다.

```javascript
<MyComponent foo={1 + 2 + 3 + 4} />
```

### 문자열 리터럴
    문자열 리터럴은 prop으로 넘겨줄 수 있습니다. 
    문자열 리터럴을 넘겨줄 때, 그 값은 HTML 이스케이프 처리가 되지 않습니다. 즉 &lt;는 <로 변경되지 않습니다. 덕분에 에러 없이 값을 넘깁니다.

```javascript
<MyComponent message="&lt;3" />

<MyComponent message={'<3'} />
```

### Props의 기본값은 "True"
    Prop에 어떤 값도 넘기지 않을 경우, 기본값은 true입니다.
    일반적으로 prop에 대한 값을 전달하지 않는 것을 권장하지 않습니다. 이는 ES6 object shorthand 와 헷갈릴 수 있기 때문입니다. 
    
    * ES6 object shorthand는 객체의 요소를 추가할 때 key 값과 value 값이 동일한 경우 사용되는 축약 효현입니다.
### 속성 펼치기
    props에 해당하는 객체를 이미 가지고 있다면, ...를 Spread 연산자로 사용해 전체 객체를 그대로 넘겨주거나 특정 prop을 선택하고 나머지 prop은 전개 연산자를 통해 넘길 수 있습니다. 
    하지만 이는 불필요한 prop을 컴포넌트에 넘기거나 유효하지 않은 HTML 속성들을 DOM에 넘기기도 합니다. 꼭 필요할 때만 사용하는 것을 권장합니다.

```javascript
const Button = props => {
  const { kind, ...other } = props;
  const className = kind === "primary" ? "PrimaryButton" : "SecondaryButton";
  return <button className={className} {...other} />;
};

const App = () => {
  return (
    <div>
      <Button kind="primary" onClick={() => console.log("clicked!")}>
        Hello World!
      </Button>
    </div>
  );
};
```



## Children in JSX

`	<>`를 통해 여는 태그와 `</>`를 통해 닫는 태그를 가지고 사용하는 **JSX** 태그의 경우에, 그 중간에 있는 내용들은 하위 컴포넌트의 `props.children`이라는 특수 **prop**으로 전달 된다. 이러한 것처럼 태그 요소의 하위 엘리먼트를 전달하는 것에는 여러가지 방법이 있다.



### String Literals

```javascript
 
<MyComponent>Hello world!</MyComponent>
<div>This is valid HTML &amp; JSX at the same time.</div>
<div>Hello World</div>

// 아래의 3개 태그는 모두 Hello World로 출력된다. 
<div>
  Hello World
</div>

<div>
  Hello
  World
</div>

<div>

  Hello World
</div>
```
​	위와 같이 단순히 문자열을 기록하면 자동적으로 태그 사이의 내용들이 `props.children`으로 전달 된다. 이때 중간에 쓰여지는 내용은 **html**에 작성하는 문서처럼 **html-unescaped**를 주의하여 사용해야 한다.

​	문자열 한줄의 시작과 끝에 있는 공백은 제거 되며, 공백라인도 제거된다. 서로 다른 줄에 쓰여진 문자열은 공백 1개로 이어지게 된다.



### JSX Children

​	**JSX** 태그안에 수많은 **JSX 엘리먼트**를 하위 컴포넌트로 전달 하기도 한다. 아래의 예제처럼 **JSX 태그**만을 하위 컴포넌트로 전달할 수도 있고, 문자열이 섞인 **JSX 태그**를 전달할 수도 있다. 또한 여러개의 **JSX 태그**를 동시에 반환하는 리액트 컴포넌트를 구성할 수도 있다.

```javascript
// 컴포넌트만을 전달하는 태그
<MyContainer>
  <MyFirstComponent />
  <MySecondComponent />
</MyContainer>

// HTML 스타일의 혼합 태그
<div>
  Here is a list:
  <ul>
    <li>Item 1</li>
    <li>Item 2</li>
  </ul>
</div>

// 컴포넌트 내부의 render() 메소드에서 JSX 태그 배열을 반환. 
render() {
  // No need to wrap list items in an extra element!
  return [
    // Don't forget the keys :)
    <li key="A">First item</li>,
    <li key="B">Second item</li>,
    <li key="C">Third item</li>,
  ];
}
```



### JavaScript Expressions as Children

`	{}`를 통해서 자바스크립트 표현식을 전달할 수도 있다. 이러한 형태는 길이를 알 수없는 데이터를 **JSX**를 통해 표현하고 싶을 때 유용하다.

```javascript
function Item(props) {
  return <li>{props.message}</li>;
}

function TodoList() {
  const todos = ['finish doc', 'submit pr', 'nag dan to review'];
  return (
    <ul>
      {todos.map((message) => <Item key={message} message={message} />)}
    </ul>
  );
}

```



### Functions as Children

`	props.children`으로 전달하는 데이터는 위의 예제들처럼 리액트가 렌더링 가능한 정보들도 있지만, 리액트가 렌더링 하지 못하는 일반 자바스크립트 구문들도 전달 할 수 있다. 아래의 예제를 살펴보자.

```javascript
// Calls the children callback numTimes to produce a repeated component
function Repeat(props) {
  let items = [];
  for (let i = 0; i < props.numTimes; i++) {
    items.push(props.children(i)); // props.children을 콜백함수로 사용하고 있음. 
  }
  return <div>{items}</div>;
}

function ListOfTenThings() {
  return (
    <Repeat numTimes={10}>
      {/* anonymous function을 생성하고 전달함. props.children이 콜백 함수처럼 동작하게 됨 */}
      {(index) => <div key={index}>This is item {index} in the list</div>}
    </Repeat>
  );
}
```

​	위와 같이 **JSX** 의 하위 `props.children`으로는 어느 형태의 스크립트 던지 전달 할 수 있다. 함수를 전달하는 형태는 잘 사용하지 않지만, **JSX**로 얼마나 많은 기능을 수행할 수 있는지 실험해 보고 싶으면 한번쯤 써보고 말면 되겠다.



### Booleans, Null, and Undefined Are Ignored

​	`false`, `null`, `undefined`, `true`와 같은 값으로서의 의미가 없는 데이터들은 화면에 렌더링 되지 않는다. 그런 이유로 인해 아래의 예제 코드는 모두 아무것도 렌더링 되지 않는 동일한 기능을 하는 코드임을 알 수 있다.

```javascript
<div />

<div></div>

<div>{false}</div>

<div>{null}</div>

<div>{undefined}</div>

<div>{true}</div>
```

​	이러한 속성은 간단한 자바스크립트 표현식으로 conditional 렌더링을 구현하는데 유용하다. 아래의 예제에서는 `showHeader`의 값이 `true`인 경우에 `<Header />`만을 렌더링 하고, 그 외에는 아무것도 렌더링 하지 않는다.

```javascript
<div>
  {showHeader && <Header />}
  <Content />
</div>
```



​	그러나 이런 형태의 자바스크립트 구문을 **JSX**에서 사용할 때 주의할 점은, 실제 자바 스크립트에서 조건 데이터로 사용되는 `0` 과 같은 숫자 데이터는 렌더링이 된다는 점이다. 아래의 예제를 살펴보자

```javascript
 // length에 저장된 숫자 자체가 렌더링 되어 버린다. 
// 물론 MessageList도 항상 렌더링 된다. 
<div>
  {props.messages.length &&
    <MessageList messages={props.messages} />
  }
</div>
​
// length > 0 은 true/false를 반환 하므로 아무 출력도 남기지 않는다.
// 길이가 0 이상일때만 MessageList 컴포넌트 만을 렌더링 한다. 
<div>
  {props.messages.length > 0 &&
    <MessageList messages={props.messages} />
  }
</div>
```

​	반대로 `false`, `null`, `undefined`, `true` 값을 화면에 출력하고 싶을 때에는, 아래의 예제처럼 [스트링으로 변환](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String#String_conversion)한 뒤 사용해야 한다.

```javascript
<div>
  My JavaScript variable is {String(myVariable)}.
</div>
```





## Reference

- [JSX in Depth](https://reactjs.org/docs/jsx-in-depth.html)

  
