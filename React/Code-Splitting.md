# Code-Splitting
<!--Table of Contents-->

- 번들링
    - 번들링이란?
- 코드 분할
    - 코드 분할이란?
- 코드 분할의 다양한 방법
    - import
    - React.lazy
    - Router-based code splitting
    - Named Exports

<!-- 어떤 질문을 대답할 수 있어야 하는지-->

## You can answer
- 번들링과 코드 분할의 둘은 어떤 연관관계를 갖고 있는가?
- 코드 분할이란 무엇인지, 왜 사용해야 하는지?
- 코드 분할의 방법에는 어떤 것들이 존재하는지?

<!--Contents-->

---
## Bundling
### 번들링이란?
    번들링이란 의미 그대로 "어떤 것들을 묶는다"입니다. React에서는 기능별로 모듈화했던 자바스크립트 파일들을 묶어주는 의미를 갖게 된다. 
    대분의 React앱은 Webpack, Rollup 같은 툴을 사용, 여러 파일을 하나로 병합한 파일을 웹 페이지에 포함하여 한 번에 전체 앱을 로드할 수 있습니다.(CSR의 개념)

## Code-splitting
### 코드 분할이란?
    프로젝트의 규모가 커지면 번들 또한 거대해지기 마련입니다. 큰 규모의 서드 파티 라이브러리를 추가할 때 실수로 앱이 커져서 로드 시간이 길어지는 것을 주의해야합니다.
    이를 해결할 방법이 번들을 나누는 것입니다. 이 방법은 런타임 시간동안 여러 번들을 동적으로 만들고 불러오는 것으로 Webpack같은 번들러가 지원하는 기능입니다. 
### 코드 분할의 기능
    앞서 말했듯 너무 큰 번들을 나눠주고, 앱의 지연 로딩을 도와주어서 사용자가 성능 향상을 느낄 수 있게 해줍니다. 
    앱의 코드 양을 따로 줄이지 않으며 사용자에게 불필요한 코드를 불러오지 않기 때문에 앱의 초기화 로딩에 필요한 비용을 절감시켜줍니다. 

## 코드 분할 방법
### import()
    코드 분할을 도입하는 가장 좋은 방법은 동적 import() 문법의 시용입니다.
    아래 예제에서 버튼을 클릭하기 전까지 nofity.js를 불러오지 않습니다. 
```javascript
import { Component } from 'react' ;

class App extends Component {
    handleClick = () => {
        import('./notify').then(({default: notify}) => { // 동적 import 사용 
            notify();
        })
    };
    render() {
        return (
            <div>
                <button onClick={this.handleClick}>Click Me!</button> 
            </div>
        );
    }
}

export default App;
```

### React.lazy
    React.lazy 함수를 사용하면 동적 import를 사용해서 컴포넌트를 렌더링 할 수 있습니다. 
    아래의 예시에서 Suspense Component는 lazy 컴포넌트가 로드되길 기다리는 동안 로딩 화면과 같은 예비 컨텐츠를 보여줄 수 있게 해줍니다.
```javascript
import React, { Suspense } from 'react';

const OtherComponent = React.lazy(() => import('./OtherComponent'));
const AnotherComponent = React.lazy(() => import('./AnotherComponent'));

function MyComponent() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <section>
          <OtherComponent />
          <AnotherComponent />
        </section>
      </Suspense>
    </div>
  );
}
```



    하지만 React.lazy와 Suspense는 아직 SSR을 할 수 없습니다. 서버에서 렌더링 된 앱에서 코드 분할을 하기 원한다면 Loadable Components의 사용을 추천합니다.

### lazy의 Error boundaries
    네트워크 장애 같은 이유로 다른 모듈을 로드에 실패할 경우 에러가 발생합니다. 이때 Error Boundaries를 이용할 수 있습니다. Error Boundary를 만들고 lazy 컴포넌트를 감싸면 장애가 발생했을 때 에러를 표시할 수 있습니다.

### Route-based code splitting
    코드 분할을 어느 곳에서 사용해야하는지 결정하는 것은 조금 어렵습니다. 그나마 이를 사용하기 하기 좋은 장소는 라우트 부분입니다. 웹 페이지를 불러오는 시간은 페이지 전환에 나타납니다. CSR의 특성상 대부분 페이지를 한번에 렌더링하기 때문에 사용자가 페이지를 렌더링하는 동안 다른 요소와 상호작용할 수 없습니다.
    첫 구동시 불러오지 못한 페이지를 불러오는 시간을 라우팅시에 발생하도록 합니다.
```javascript
import React, { Suspense, lazy } from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';

const Home = lazy(() => import('./routes/Home'));
const About = lazy(() => import('./routes/About'));

const App = () => (
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Switch>
        <Route exact path="/" component={Home}/>
        <Route path="/about" component={About}/>
      </Switch>
    </Suspense>
  </Router>
);
```

### Named Exports
    React.lazy는 비교적 최근 기술이기 때문에 현재는 default exports만 지원합니다. 따라서 named exports를 사용하고자 한다면 default로 이름을 재정의한 중간 모듈을 생성해야 합니다. 이렇게 하면 tree shaking이 계속 동작하고 사용하지 않는 컴포넌트는 가져오지 않습니다.

---
## Reference
- [리액트 프로젝트 코드 스플리팅 정복하기](https://velog.io/@velopert/react-code-splitting)
- [번들링(Bundling) 개념 및 사용 이유](https://humanwater.tistory.com/2)
- [CSR과 SSR](https://velog.io/@chkim132/React-CSR%EA%B3%BC-SSR)
