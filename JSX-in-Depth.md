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



## SpecifyingThe React Element Type

**	JSX** 태그의 첫번째 파트가 리액트 엘리먼트의 타입을 결정한다.

​	대문자로 시작하는 태그의 경우에는 리액트 컴포넌트임을 의미한다. 이러한 태그들은 리액트의 컴포넌트를 곧바로 사용하므로, `<Foo />`와 같은 **JSX** 태그를 사용하면, 같은 코드 스코프 내에 `Foo` 컴포넌트나 변수가 존재해야 한다.



### React Must Be in Scope

​	리액트 코드가 컴파일 될 때 **JSX** 태그는 곧바로 `React.createElement` 스크립트로 변환이 되므로, `React` 라이브러리 또한 같은 코드에 있어야 한다.

```javascript
 import React from 'react'; // JSX 태그를 컴파일 한 뒤 사용할 때 필요함. 
import CustomButton from './CustomButton'; // JSX 태그에서 사용됨

function WarningButton() {
  // return React.createElement(CustomButton, {color: 'red'}, null);
  return <CustomButton color="red" />;
}

```

​	위의 예제에서 `CustomButton`은 **JSX** 태그로만 사용됨에도 불구하고, 컴파일 타임의 필요성으로 인해 `import` 되어야 하며, **JSX** 태그 자체의 컴파일을 위해서 `react` 라이브러리 또한 `import` 되어야 함을 알 수 있다.

​	단순히 html의 `script` 태그안에서 `react` 라이브러리를 로드 했다면, 전역 객체로 로드 되므로 따로 추가적인 로드를 하지 않아도 된다.



### Using Dot Notation for JSX Type

**	JSX** 태그 안에서 dot-notation을 통해 리액트 컴포넌트에 접근 할 수도 있다. 다양한 리액트 컴포넌트를 가지고 있는 하나의 모듈을 임포트 했을 때 매우 유용한 방법이다. 아래의 예제 처럼 `MyComponents.DatePicker` 가 리액트 컴포넌트 라면 dot-notation으로 접근 할 수 있다.

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



### User-Defined Components Must Be Capitalized

​	**JSX** 태그의 시작이 소문자라면, `<div>` 나 `<span>` 과 같은 이미 정의되어 있는 컴포넌트를 의미하게 된다. 이러한 태그들은 `React.creatElement`에 `'div'` 나 `'span'`과 같이 `''` 로 감싸진 스트링으로 전달 되는 반면에, `<Foo />`와 같이 대문자로 시작하는 컴포넌트 들은 `React.createElement(Foo)` 형태 처럼 객체 이름 그대로 전달된다.

​	이러한 이유로 컴포넌트 이름은 반드시 대문자로 시작하게 짓기를 권장하지만, 소문자로 사용하고 싶다면 **JSX** 스크립트에서 사용하기 전에 대문자로 시작하는 변수 이름에 해당 컴포넌트를 할당 한뒤 **JSX** 태그에서 사용해야 한다.



### Choosing the Type at Runtime

```javascript
import React from 'react';
import { PhotoStory, VideoStory } from './stories';

const components = {
  photo: PhotoStory,
  video: VideoStory
};

function Story(props) {
  // Wrong! JSX type can't be an expression.
  return <components[props.storyType] story={props.story} />;
}
```

일반적인 자바스크립트 구문을 **JSX** 태그에서 리액트 컴포넌트로 사용할 수 없다. 무슨 말인지 이해하기 위에서 위의 코드를 살펴보자. `components` 객체에 2개의 리액트 컴포넌트가 있음에도, **JSX**의 태그로 `components[props.stroyType]` 같은 형태의 자바스크립트 구문을 사용할 경우 에러가 발생한다. 

위의 예제처럼 props나 기타 변수를 통해 전달된 내용으로 **JSX** 태그를 결정하고 싶을 때에는, 대문자로 시작하는 변수에 원하는 컴포넌트를 할당 한 뒤 사용해야한다. 정상적인 코드는 아래와 같다. 

```javascript
import React from 'react';
import { PhotoStory, VideoStory } from './stories';

const components = {
  photo: PhotoStory,
  video: VideoStory
};

function Story(props) {
  // Correct! JSX type can be a capitalized variable.
  const SpecificStory = components[props.storyType];
  return <SpecificStory story={props.story} />;
}
```





## Props in JSX

**JSX**에서 **props**를 정의하는 방법은 여러가지가 있다.



### JavaScript Expressions as Props

```javascript
 <MyComponent foo={1 + 2 + 3 + 4} />
```

​	가장 기초적으로는, 위의 예제처럼 자바스크립트 표현식을 **prop**에 사용하는 것이다. **JSX**에서 자바 스크립트 코드를 사용할 때에는 항상 `{}`로 감싸주어야 한다.

​	`MyComponent`에서 받는 `props.foo`에는 계산식이 계산된 `10`이 전달된다.

`	if`나 `for` 와 같은 문법들은 일반 표현식이 아니므로 `JSX에서` 곧바로 사용할 수 없다. 대신에 해당 구문들을 미리 사용하고, 원하는 **JSX**를 자바스크립트 변수에 할당하는 것이 가능하다. 예제를 살펴 보자.

```javascript
function NumberDescriber(props) {
  let description;
  if (props.number % 2 == 0) {
    description = <strong>even</strong>;
  } else {
    description = <i>odd</i>;
  }
  return <div>{props.number} is an {description} number</div>;
}
```



### String Literals

```javascript
<MyComponent message="hello world" />

<MyComponent message={'hello world'} />
```

​	위의 예제처럼 스트링 자체를 **prop**으로 전달 할 수도 있다. 위의 두 예제는 완전히 동일한 기능을 한다.

```javascript
<MyComponent message="&lt;3" />

<MyComponent message={'<3'} />
```

**props**에 자바스크립트 표현식이 아닌, 문자열 자체를 전달할 경우 **html** 표현식에 따라 파싱되므로 위의 두 예제도 완전히 동일한 기능을 한다.



### Props Default to “True”

**prop**을 명시하기만 하고 아무 값도 대입하지 않을 경우 기본적으로 `true`가 전달 된다.

```javascript
<MyTextBox autocomplete />

<MyTextBox autocomplete={true} />
```

​	위의 두 코드는 완전히 동일한 기능을 한다. **ES6**에서 `{foo}` 라고 객체를 선언 할 경우 `{foo: true}`가 아닌 `{foo: foo}` 로 해석되는 **[<u>ES6 object shorthand</u>](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Object_initializer#New_notations_in_ECMAScript_2015)**와 헷갈리므로, `{}` 표현식으로 `true`, `false`를 전달하지 않아야 한다.



### Spread Attributes

​	이미 `props`로 전달할 내용들을 담고 있는 객체를 생성하였다면 spread(`...`) 문법을 사용해서 **JSX**에 전달 할 수 있다. `...` 문법은 *spread* 구문으로, 객체 전체를 전달한다는 의미이다. 아래의 두 예제 함수는 동일한 기능을 한다.

```javascript
function App1() {
  return <Greeting firstName="Ben" lastName="Hector" />;
}

function App2() {
  const props = {firstName: 'Ben', lastName: 'Hector'};
  return <Greeting {...props} />;
}
```

​	전달 받은 props에서 원하는 속성만을 골라내는 것도 가능하다.

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

​	위의 예제에서 `App` 컴포넌트가 `Button` 컴포넌트를 사용하고 있다. `Button` 컴포넌트는 전달 받은 props에서 `kind` 속성만을 제거하고, 나머지 `onClick` prop과 `Hello World!` 하위 엘리먼트를 `<button>` 엘리먼트로 전달한다.

* **Spread** 구문은 경우에 따라서 매우 유용하나, 적절하게 불필요한 속성을 제거하지 않으면 잘못된 props를 컴포넌트에 전달할 수도 있고, 잘못된 html 속성을 DOM에 전달할 수도 있으므로, 사용하지 않는게 좋다.



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

  
