# Fragments

- Fragments란?
    - 접근성의 시멘틱 HTML
- 사용방법
    - 일반 선언
    - 단축 선언
- key가 있는 Fragments
  

## You can answer
- Fragments란 무엇인가?
- Fragments를 사용하는 방법은 무엇인가?
- Fragments를 왜 사용해야 하는가?


---
## Fragments란?
    React에서 컴포넌트가 여러 엘리먼트를 반환하는 것은 흔한 패턴입니다. 
    리엑트에서 return문 안에는 반드시 하나의 최상위 태그가 있어야 합니다.
    이를 해결하기 위해 의미없는 div는 삽입합니다. 리엑트가 하나의 컴포넌트만을 반환해주기 때문에 어쩔 수 없는 선택이지만 이로 인하여 최종 html코드에 의미없는 코드가 생기고 더 많은 메모리를 차지합니다. 만약 엘리먼트 사이에 div가 들어간다면 레이아웃에 유지가 어려워지는 경우가 발생할 수 있습니다.
    Fragments는 별도의 노드를 추가하지 않으며 여러 자식들을 그룹화할 수 있도록 도와줍니다.  

## 사용방법
### 일반 선언

```javascript
import { Fragment } from "react";

function Table() {
  return (
    <Fragment>
      <td>Hello</td>
      <td>World</td>
    </Fragment>
  );
}
```

### 단축 선언
```javascript
function Table() {
  return (
    <>
      <td>Hello</td>
      <td>World</td>
    </>
  );
}
```

## key가 있는 Fragments
    JS에는 다양한 배열 조작 메서드가 있습니다. 그중에서도 배열의 요소를 반환하는 메서드를 사용할때 Fragment가 감싸고 있는 곳에 key를 전달하고 싶다면 반드시 Fragment에 key를 전달해주어야 합니다.

```javascript
function Glossary(props) {
  return (
    <dl>
      {props.items.map(item => (
        // React는 `key`가 없으면 key warning을 발생합니다.
        <React.Fragment key={item.id}>
          <dt>{item.term}</dt>
          <dd>{item.description}</dd>
        </React.Fragment>
      ))}
    </dl>
  );
}
```

    key는 Fragment에 전달할 수 있는 유일한 어트리뷰트입니다. 추후 이벤트 핸들러와 같은 추가적인 어트리뷰트를 지원할 수도 있습니다.



---
## Reference
- [[React] Fragment란?](https://velog.io/@dolarge/React-Fragment%EB%9E%80)
- [React Fragments 사용이유 및 사용법](https://velog.io/@lilyoh/React-Fragments-%EC%82%AC%EC%9A%A9%EC%9D%B4%EC%9C%A0-%EB%B0%8F-%EC%82%AC%EC%9A%A9%EB%B2%95)
