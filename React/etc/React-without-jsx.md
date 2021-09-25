# React-without-jsx

- How to use React without jsx
- react-hyperscript
- How to use react-hyperscript

## You can answer

- React-without-jsx를 사용하는 방법은?
- react-hyperscript는 무엇인가?
- react-hyperscript를 사용하는 방법?

------

## How to use React without jsx

리액트에서 JSX 는 컴포넌트가 렌더링하는 dom 구조를 정의할 때 사용됩니다. 순수 JS에서 html 을 마크업하는 것은 여간 귀찮은 일이 아니기 때문에 JSX 는 리액트에서 정말 유용합니다.
React를 사용할 때 JSX는 필수가 아닙니다. 빌드 환경에서 컴파일 설정을 하고 싶지 않을 때 JSX 없이 React를 사용하는 것은 특히 편리합니다.
각 JSX element는 React.createElement(component, props, ...children)를 호출하기 위한 synthetic sugar입니다. 


예를 들어 다음의 JSX로 작성된 코드는

```javascript

class Hello extends React.Component {
  render() {
    return <div>Hello {this.props.toWhat}</div>;
  }
}

ReactDOM.render(
  <Hello toWhat="World" />,
  document.getElementById('root')
);

```

아래처럼 JSX를 사용하지 않은 코드로 컴파일될 수 있습니다.

```javascript
class Hello extends React.Component {
  render() {
    return React.createElement('div', null, `Hello ${this.props.toWhat}`);
  }
}

ReactDOM.render(
  React.createElement(Hello, {toWhat: 'World'}, null),
  document.getElementById('root')
);
```


Babel에서는 JSX가 JavaScript로 변환되는 예시를 더 볼 수 있습니다.

컴포넌트는 문자열이나 React.Component의 하위 클래스 또는 컴포넌트를 위한 일반 함수로 제공됩니다.

React.createElement를 너무 많이 입력하는 것이 피곤하다면 짧은 변수에 할당하는 방법이 있습니다.

```javascript
const e = React.createElement;

ReactDOM.render(
  e('div', null, 'Hello World'),
  document.getElementById('root')
);
```


React.createElement를 짧은 변수에 할당하면 편리하게 JSX 없이 React를 사용할 수 있습니다.

## react-hyperscript

react-hyperscript는 더 간결한 구문을 제공합니다.

## How to use react-hyperscript

우선 일반적으로 사용되는 JSX를 이용한 마크업을 보겠습니다.

```javascript
import React from 'react'
import AnotherComponent from './another-component'

export default () => (
  <div className='.example'>
    <h1 id="heading">This is hyperscript</h1>
    <h2>creating React.js markup</h2>
    <AnotherComponent foo="bar">
      <li>
        <a href={'http://whatever.com'}>One list item</a>
      </li>
      <li>
        Another list item
      </li>
    </AnotherComponent>
  </div>
)
```

react-hyperscript 를 이용하면 아래와 같이 dom 을 정의할 수 있습니다.



```javascript
import h from 'react-hyperscript'
import AnotherComponent from './another-component'

export default () =>
  h('div.example', [
    h('h1#heading', 'This is hyperscript'),
    h('h2', 'creating React.js markup'),
    h(AnotherComponent, { foo: 'bar' }, [
      h('li', [h('a', { href: 'http://whatever.com' }, 'One list item')]),
      h('li', 'Another list item'),
    ]),
  ])
```

h 함수가 React.createElement 와 뭐가 다른가 의문이 들 수도 있습니다. 
단지 함수를 간단한 변수명에 담은 것에 지나지 않은가 생각할 수 있습니다. const h = React.createElement 와 같이 말입니다. 
하지만 여기에는 중요한 차이가 있습니다.

우선 첫번째 인자를 이용해 태그와 실렉터를 함께 정의할 수 있게 해준다는 것입니다.
React.createElement 는 인자의 순서가 매우 중요하지만. react-hyperscript 는 인자의 타입에 따라 유연하게 attr 과 children 을 전달할 수 있습니다.

첫번째 인자는 반드시 type 이 오게 될 것이고, 두번째 인자가 {} 와 같은 객체일 경우에는 attr 로 인식되며 [] 와 같은 배열이나 단순 문자열인 경우에는 children 으로 처리가 됩니다.

React.createElement는 attr 없이 children 만 전달하려면 두번째 인자로 반드시 null 을 명시해야 하는 불편함이 있지만, react-hyperscript 는 attr 을 생략하고 두번째 인자로서 바로 children 을 직접 전달할 수가 있습니다.

더 나아가 hyperscript-helpers 와 함께 사용하면 아래와 같이 보다 가독성 높은 마크업이 가능합니다.

```javascript
import h from 'react-hyperscript'
import helper from 'hyperscript-helpers'
import AnotherComponent from './another-component'

const {div, h1, h2, li, a} = helper(h)

export default () =>
  div('.example', [
    h1('#heading', 'This is hyperscript'),
    h2('creating React.js markup'),
    h(AnotherComponent, { foo: 'bar' }, [
      li(a({ href: 'http://whatever.com' }, 'One list item')),
      li('Another list item'),
    ]),
  ])
```

hyperscript-helpers 팁은 다음과 같습니다.

기본 형식: h(componentOrTag, properties, children)
은 React 요소를 반환합니다.

- componentOrTag Object|String - 형식의 선택적 CSS 클래스 이름/ID 가 있는 React 구성 요소 또는 태그 문자열일 수 있습니다 h1'#'some-id.foo.bar. 태그 문자열인 경우 태그 이름을 구문 분석 하고 개체 의 id및 className속성을 변경 properties합니다.
- 속성 Object (선택 사항) - 요소에 설정하려는 속성이 포함된 개체입니다.
- children Array|String (선택 사항) - h()자식 배열 또는 문자열입니다. 이것은 각각 자식 요소 또는 텍스트 노드를 생성합니다.
- listOfElements Array - 로 래핑될 React 요소의 배열입니다 React.Fragment.

태그명 함수의 첫번째 인자로 주어지는 값이 문자열일 경우
. 으로 시작하면 클래스명, '#'으로 시작하는 문자열은 아이디명이 됩니다
두번째 세번째 인자 없이, 첫번째 인자가 . or '#' 으로 시작하지 않는 문자열일 경우 children 이 됩니다
. or '#' 으로 시작되지 않는 문자열이 첫번째 인자이고, 두번째 인자가 문자열인 경우 첫번째 인자는 무시되고 두번째 문자열 인자가 children 이 됩니다.
여러 개의 children 은 배열([])로 묶습니다.
이런 특성 말고도 전달되는 인자들을 알아서 적절하게 type, attr, children 값으로 사용해 주는 것이 좋습니다.

------

## Reference

- [react-without-jsx](https://ko.reactjs.org/docs/react-without-jsx.html)
- [Babel](https://babeljs.io/repl/#?browsers=defaults%2C%20not%20ie%2011%2C%20not%20ie_mob%2011&build=&builtIns=false&corejs=3.6&spec=false&loose=false&code_lz=GYVwdgxgLglg9mABACwKYBt1wBQEpEDeAUIogE6pQhlIA8AJjAG4B8AEhlogO5xnr0AhLQD0jVgG4iAXyJA&debug=false&forceAllTransforms=false&shippedProposals=false&circleciRepo=&evaluate=false&fileSize=false&timeTravel=false&sourceType=module&lineWrap=true&presets=react&prettier=false&targets=&version=7.14.8&externalPlugins=)
- [[리액트] jsx 없이 js 만으로 html 작성하기](https://min9nim.vercel.app/2020-08-30-react-hyperscript/)