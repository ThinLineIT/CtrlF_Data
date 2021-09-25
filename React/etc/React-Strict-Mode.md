# Strict Mode

- 서론
- 안전하지 않은 생명주기를 사용하는 컴포넌트 검사
- legacy string ref 사용에 대한 경고
- 권장되지 않는 findDOMNode 사용에 대한 경고
- 예상치 못한 부작용 검사
- legacy context API 검사

## You can answer

- What is Strict Mode?
- 어떠한 상황에서 Strict Mode를 활용할 수 있나?

------

### 서론

StrictMode는 애플리케이션 내의 잠재적인 문제를 알아내기 위한 도구입니다. 
Fragment와 같이, 렌더링되는 UI는 없으며, 자식과 자손들에 대한 부가적인 검사와 경고를 활성화합니다.

참고 : StrictMode는 오직 development mode에서만 실행됩니다. production mode에서는 아무런 영향력을 가지고 있지 않습니다.



다음과 같이 애플리케이션 내 어디서든지 Strict Mode를 활성화할 수 있습니다.

```javascript
import React from 'react';

function ExampleApplication() {
  return (
    <div>
      <Header />
      <React.StrictMode>
        <div>
          <ComponentOne />
          <ComponentTwo />
        </div>
      </React.StrictMode>
      <Footer />
    </div>
  );
}
```

위의 예시에서 Header와 Footer 컴포넌트는 Strict 모드 검사가 이루어지지 않습니다. 
하지만, ComponentOne과 ComponentTwo는 각각의 자손까지 검사가 이루어집니다.

StrictMode는 아래와 같은 부분에서 도움이 됩니다.
1. 안전하지 않은 생명주기를 사용하는 컴포넌트 검사
2. legacy string ref 사용에 대한 경고
3. 권장되지 않는 findDOMNode 사용에 대한 경고
4. 예상치 못한 부작용 검사
5. legacy context API 검사

-----

### 안전하지 않은 lifecycle 메서드 식별

비동기 React 애플리케이션에서 특정한 lifecycle 메서드들은 안전하지 않습니다. 하지만 애플리케이션이 서드 파티 라이브러리를 사용한다면, lifecycle 
메서드가 사용되지 않는다고 장담하기 어렵습니다. Strict 모드는 이러한 경우에 도움이 됩니다!

Strict 모드가 활성화되면, React는 안전하지 않은 생명주기 메서드를 사용하는 모든 클래스 컴포넌트 목록을 정리해 다음과 같이 컴포넌트에 대한 정보가 담긴 경고 로그를 출력합니다. 

[![strict mode unsafe lifecycles warning](https://ko.reactjs.org/static/e4fdbff774b356881123e69ad88eda88/1e088/strict-mode-unsafe-lifecycles-warning.png)](https://ko.reactjs.org/static/e4fdbff774b356881123e69ad88eda88/1628f/strict-mode-unsafe-lifecycles-warning.png)

Strict 모드에 의해 발견된 문제들을 해결한다면, 향후 릴리즈되는 React에서 concurrent 렌더링의 이점을 얻을 수 있을 것입니다.

- Concurrent 렌더링 모드 : (아직 비활성) 더 작은 단위로 나누고, 작업을 중지했다가 재개하는 방식으로 브라우저가 멈추는 것을 피함. 커밋하기 전에 렌더링 단계의 생명주기 메서드를 여러 번 호출하거나 아예 커밋을 하지 않을 수도 있음.

### legacy string ref 사용에 대한 경고

이전의 React에서 레거시 문자열 ref API와 콜백 API라는, ref를 관리하는 두 가지 방법을 제공하였습니다. 문자열 ref가 사용하기 더 편리했지만 몇몇 단점들이 있었습니다. 그래서 공식적으로는 콜백 형태를 사용하는 것을 권장하였습니다.

React 16.3에서는 여러 단점 없이 문자열 ref의 편리함을 제공하는 세 번째 방법을 추가하였습니다.

```javascript
class MyComponent extends React.Component {
  constructor(props) {
    super(props);

    this.inputRef = React.createRef();
  }

  render() {
    return <input type="text" ref={this.inputRef} />;
  }

  componentDidMount() {
    this.inputRef.current.focus();
  }
}
```

이제는 객체 ref가 문자열 ref를 교체하는 용도로 널리 더해졌기 때문에, Strict 모드는 문자열 ref의 사용에 대해 경고합니다.

콜백 ref는 새로운 createRef API와 별개로 지속해서 지원될 예정입니다.
컴포넌트의 콜백 ref를 교체할 필요는 없습니다. 콜백 ref는 조금 더 유연하기 때문에, 고급 기능으로서 계속 지원할 예정입니다.

### 권장되지 않는 findDOMNode 사용에 대한 경고

이전의 React에서 주어진 클래스 인스턴스를 바탕으로 트리를 탐색해 DOM 노드를 찾을 수 있는 findDOMNode를 지원하였습니다. 
DOM 노드에 바로 ref를 지정할 수 있기 때문에 보통은 필요하지 않습니다. 따라서 findDOMNode 대신 ref를 넘겨주는 방식을 권장합니다.


findDOMNode는 클래스 컴포넌트에서도 사용할 수 있었지만, 부모가 특정 자식이 렌더링되는 것을 요구하는 상황이 허용되어, 추상화 레벨이 무너지게 되었습니다. 이로 인해 부모가 자식의 DOM 노드에까지 닿을 가능성이 있어 컴포넌트의 세세한 구현을 변경할 수 없게 되어 리팩토링이 어려워지는 상황을 만들고 말았습니다. findDOMNode는 항상 첫 번째 자식을 반환하지만, Fragment와 함께 사용할 경우 컴포넌트에서 여러 DOM 노드를 렌더링하게 됩니다. findDOMNode는 일회성, 읽기 전용 API입니다. 물어보았을 때만 값을 반환합니다. 자식 컴포넌트가 다른 노드를 렌더링할 경우, 변경 사항에 대응할 방법이 없습니다. 그러므로, findDOMNode는 항상 변하지 않는, 단일 DOM 노드를 반환하는 컴포넌트에서만 정상적으로 작동해왔습니다.

ref를 넘겨주는 방식을 사용해 커스텀 컴포넌트에 ref를 넘겨 DOM까지 닿게 하는 것으로, 이를 분명하게 만들 수 있습니다.

DOM 노드를 감싸는 래퍼를 만들어 ref를 바로 붙이는 것 역시 가능합니다.

```javascript
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.wrapper = React.createRef();
  }
  render() {
    return <div ref={this.wrapper}>{this.props.children}</div>;
  }
}
```

### 예상치 못한 부작용 검사

리액트는 렌더링 단계와 커밋 단계 두가지의 단계로 동작합니다.
렌더링 단계는 render 함수를 호출해서 이전 렌더와 비교를 수행하는 단계이고, 커밋 단계의 경우에는 라이프 사이클 함수를 실행시키며 DOM 노드를 추가/변경해주는 단계입니다. 여기서 커밋단계는 일반적으로 렌더링 단계보다 빠릅니다.
그로인해 느린 렌더링 단계에서 여러 생명주기 메서드가 여러번 호출되기도 합니다. 이러한 것들을 strict 모드에서는 미리 파악하고 우리에게 경고해줍니다. 

보통 class component의 생명 주기 메소드인 componentWillMount()나 componentWillUpdate() 등에서 이런 형태가 나타나지만 setState에서 역시 이러한 현상들이 나타납니다. 당연히 setState와 같은 기능으로 사용되는 useState에서도 이러한 현상이 나타나는 경우가 있습니다.

이러한 메서드들은 여러 번 호출될 수 있기 때문에 원하는 결과값을 보존하기 위해서 미리 잡아야 합니다. 그럴 때 우리가 Strict 모드를 통해 2번 수행되는 메서드들을 잡아 미리 고칠 수가 있는 것입니다. 물론 Strict 모드가 자동적으로 모든 부작용을 찾아낼 수는 없지만, 문제가 될 만한 함수를 두 번 실행하는 방법으로써 이러한 발견을 도와줍니다. 즉 Double-Invoke 방식을 통해 이를 우리에게 알려주는 것입니다.

> 만약 double invoke가 실행되었을 때 두개의 결과 값이 서로 다르다면? 해당 코드는 문제가 있다는 뜻이 됩니다.

렌더링 단계 생명주기 메서드는 클래스 컴포넌트의 메서드를 포함해 다음과 같습니다.

- constructor
- componentWillMount (or UNSAFE_componentWillMount)
- componentWillReceiveProps (or UNSAFE_componentWillReceiveProps)
- componentWillUpdate (or UNSAFE_componentWillUpdate)
- getDerivedStateFromProps
- shouldComponentUpdate
- render
- setState 업데이트 함수 (첫 번째 인자)

위의 메서드들은 여러 번 호출될 수 있기 때문에, 부작용을 포함하지 않는 것이 중요합니다. 이 규칙을 무시할 경우, 메모리 누수 혹은 잘못된 애플리케이션 상태 등 다양한 문제를 일으킬 가능성이 있습니다. 불행히도, 보통 이러한 문제들은 예측한 대로 동작하지 않기 때문에 발견하는 것이 어려울 수 있습니다.

Strict 모드가 자동으로 부작용을 찾아주는 것은 불가능합니다. 하지만, 조금 더 예측할 수 있게끔 만들어서 문제가 되는 부분을 발견할 수 있게 도와줍니다.



### legacy context API 검사

레거시 context API는 오류가 발생하기 쉬워 이후 릴리즈에서 삭제될 예정입니다. 모든 16.x 버전에서 여전히 돌아가지만, Strict 모드에서는 아래와 같은 경고 메시지를 노출합니다.

[![warn legacy context in strict mode](https://ko.reactjs.org/static/fca5c5e1fb2ef2e2d59afb100b432c12/1e088/warn-legacy-context-in-strict-mode.png)](https://ko.reactjs.org/static/fca5c5e1fb2ef2e2d59afb100b432c12/51800/warn-legacy-context-in-strict-mode.png)



------

## Reference

- [Strict Mode](https://ko.reactjs.org/docs/strict-mode.html)
- [이화랑 블로그](https://leehwarang.github.io/docs/tech/2020-11-29-ref.html)

