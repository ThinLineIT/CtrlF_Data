# Error Boundary

- Error Boundary 정의 
    - Error Boundary란?
- Error Boundary가 잡을 수 없는 예외 상황
- Error Boundary 사용 방법
- Error Boundary가 배치될 적절한 위치
- 포착할 수 없는 에러에 대한 새로운 동작
- 컴포넌트 스택 추적
- try ~ catch와 이벤트 헨들러

## You can answer
- Error Boundary 사용 이유
- Error Boundary가 처리하지 못하는 부분은 무엇이 있는지?
- Error Boundary가 잡아내지 못하는 부분의 처리는 어떻게 하면 좋은 지?
- 명령적 코드와 선언적 코드의 의미 사이에서 Error Boundary는 어디에 속하는지?


---
## Error Boundary 정의
### Error Boundary 배경
    과거시절 컴포넌트 내부의 JS 코드에서 발생하는 에러가 React의 내부 상태를 훼손하고 렌더링 과정에서 암호화 에러를 유발하곤 했습니다. 애플리케이션 코드의 이전 단계에서 발생한 에러는 그동안 React에서 컴포넌트 내의 에러를 처리할 수 있는 방법이 없었기 때문에 이를 해결할 수 없었습니다. 
### Error Boundary란?
    당연하게 UI일부분에 존재하는 JS에러가 애플리케이션 전체를 중단시키는 일은 일어나면 안됩니다. 
    이를 위해서 React 16에서는 Error Boundary라는 개념이 추가되었습니다.
    
    Error Boundary는 하위 컴포넌트 트리의 어디에서든 JS 에러를 일으키는 컴포넌트 트리 대신 FallBack UI를 보여주는 React 컴포넌트입니다. Error Boundary는 렌더링 도중 Life Cycle 메서드 및 그 자손이 있는 전체 트리에서 에러를 잡아냅니다. 

## Error Boundary가 잡을 수 없는 예외 상황
    Error Boundary는 다음과 같은 에러는 포착할 수 없습니다. 
    
    - 이벤트 핸들러
    - 비동기적 코드( Ex. setTimeout 혹은 requestAnimaionFrame)
    - 서버 사이드 렌더링
    - 자손에서가 아닌 Error Boundary 자체에서 발생하는 에러

## Error Boundary의 사용
    생명주기 메서드인 static getDerivedStateFromError() 와 componentDidCatch() 중 하나 (혹은 둘 다)를 정의하면 클래스 컴포넌트 자체가 에러 경계가 됩니다. 에러가 발생한 뒤에 fallback UI를 렌더링하려면 static getDerivedStateFromError()를 사용하세요. 에러 정보를 기록하려면 componentDidCatch()를 사용하세요.

```javascript
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    // 다음 렌더링에서 폴백 UI가 보이도록 상태를 업데이트 합니다.
    return { hasError: true };
  }

  componentDidCatch(error, errorInfo) {
    // 에러 리포팅 서비스에 에러를 기록할 수도 있습니다.
    logErrorToMyService(error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      // 폴백 UI를 커스텀하여 렌더링할 수 있습니다.
      return <h1>Something went wrong.</h1>;
    }

    return this.props.children;
  }
}

```

    Error Boundary는 JS의 catch {} 구문과 유사하게 동작하지만 컴포넌트에 적용됩니다. 오직 클래스 컴포넌트만이 Error Boundary가 될 수 있습니다. 대부분의 경우 Error Boundary 컴포넌트를 한 번만 선언하여 애플리케이션 전체에서 활용하는 것이 좋습니다.
    
    Error Boundary는 트리 내에서 하위에 존재하는 컴포넌트의 에러만을 포착합니다. Error Boundary 자체적으로는 에러를 포착할 수 없습니다. Error Boundary가 에러 메시지를 렌더링하는 데에 실패한다면 에러는 그 위의 가장 가까운 Error Boundary로 전파될 것입니다. 이 또한 JS의 catch {} 구문이 동작하는 방식과 유사합니다.

## Error Boundary가 배치될 적절한 위치 
    디테일한 부분들은 개발자에게 달려있습니다. 주된 위치는 최상위 경로의 컴포넌트를 감싸서 유저에게 "문제가 발생했습니다" 라는 알림을 보여줄 수 있도록 합니다. 
    또한 각 위젯을 Error Boundary로 감써서 애플리케이션의 나머지 부분이 충돌하지 않도록 보호할 수 있습니다. 

## 포착할 수 없는 에러에 대한 새로운 동작
    React 16부터는 Error Boundary에서 포착되지 않은 에러로 인해 전체 React 컴포넌트 트리의 마운트가 해제됩니다.
    문제가 생긴 UI를 완전히 제거하는 것이 사용자로 인한 에러를 일으킬 가능성이 더 적기 때문입니다.

## 컴포넌트 스택 추적
    React 16은 애플리케이션이 실수로 에러를 집어삼킨 경우에도 개발 과정에서 렌더링하는 동안 발생한 모든 에러를 콘솔에 출력합니다. 에러 메시지 및 자바스크립트 스택과 더불어 React 16은 컴포넌트 스택 추적 또한 제공합니다. 이제 정확히 컴포넌트 트리의 어느 부분에서 에러가 발생했는지 확인할 수 있게 되었습니다. 또한 컴포넌트 스택 추적 내에서 파일 이름과 줄 번호도 확인할 수 있습니다.

## try/catch와 이벤트 핸들러
### try/catch와의 비교
    try / catch는 훌륭하지만 명령형 코드에서만 동작합니다. 그러나 React 컴포넌트는 선언적이며 무엇을 렌더링할지 구체화합니다.
    Error Boundary는 선언적 특성에 따라서 예상된 대로 동작합니다. 복잡한 트리의 어느 부분의 setState에 의해 유발된 componentDidUpdate 메서드에서 에러가 발생하더라도 가장 가까운 Error Boundary에 올바르게 전달됩니다.
### 이벤트 핸들러에서 발생하는 에러
    Error Boundary는 이벤트 핸들러 내부의 에러를 발견할 수 없습니다. 
    render 메서드 및 생명주기 메서드와 달리 이벤트 핸들러는 렌더링 중에 발생하지 않습니다. 그렇기 때문에 Error Boundary는 이벤트 핸들러가 에러를 던져도 Error Boundary는 잡아낼 수 없고 기본 에러 페이지를 방출합니다.
    이벤트 헨들러에서 에러를 검출해내고 싶다면 기본 JS의 try/catch 구문을 사용해야 합니다. 

## 서버 사이드 렌더링 과정의 에러
    Node 기반의 NEXT에서 SSR을 진행하여 초기 빌드를 진행할 때 Window나 document 객체가 존재하지 않습니다. 그래서 React SSR 적용 시 가장 흔하게 발생되는 에러 구문이 "window is not defined,,,,," , "document is not defined..."가 대표적입니다. 앞서 말했듯 렌더링 과정의 에러를 탐지하는 Error Boundary는 SSR시 발생하는 에러를 처리할 수 없습니다. 그렇기 때문에 이는 다른 방식을 사용하여 에러를 우회하여야 합니다.
    이를 해결하기 위해서 렌더링 과정으로 작업을 옮기는 방법이 있습니다. 
```javascript
const useMenu = ({ navRef, curtainRef, listRef, device }) => {
  useEffect(() => {
    const mql = window.matchMedia(`(max-width: ${device.sm})`);
  })
  /* ... */
}
```
    React의 hook인 useEffect는 렌더링 과정에서 실행되게 됩니다. 그렇기 때문에 window, document같은 객체를 사용할 코드를 useEffect안으로 선언하여 우회하는 방법이 가장 대표적인 해결방법입니다. 
---
## Reference
- [명령형 프로그래밍 VS 선언형 프로그래밍](https://boxfoxs.tistory.com/430)
- [React Error Handling And Reporting With Error Boundary And Sentry](https://www.smashingmagazine.com/2020/06/react-error-handling-reporting-error-boundary-sentry/)
- [리액트 SSR을 적용했습니다!!](https://www.junggri.com/topic/%EA%B0%9C%EB%B0%9C%EC%9D%BC%EC%A7%80/f66e3b4b-49c3-4dfc-bd23-1a8e6e81525b)
- [빌드 에러 'window is not available...' 해결법](https://www.sungikchoi.com/blog/window-is-not-available/)