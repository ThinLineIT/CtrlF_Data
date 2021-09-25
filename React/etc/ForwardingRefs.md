# Forwarding Refs

- DOM에 refs 전달하기
- forwardRef 사용시 주의사항
- 고차원 컴포넌트에서의 ref 전달하기
- DevTools에 사용자 정의 이름 표시하기
    -  forwardRef() 함수에 이름이 있는 함수 넘기기
    -  forwardRef() 함수의 호출 결과로 기존 컴포넌트를 대체 
    -  forwardRef() 함수의 호출 결과의 displayName 속성 설정


## You can answer
- forwardRef를 사용해야하는 이유는?
- forwardRef를 디버깅하는 방법은?
- forwardRef는 어떤 컴포넌트에 사용하면 좋은지?


---
## DOM에 refs 전달하기
    간단한 버튼 예제가 있습니다. 

```javascript
function FancyButton(props) {
  return (
    <button className="FancyButton">
      {props.children}
    </button>
  );
}
```

    이는 다양한 컴포넌트에서 재활용이 가능합니다. 이 버튼을 재활용할 때는 <FancyButton props={...} /> 처럼 간단하게 사용이 가능합니다. 대부분 특별한 기능을 버튼에 부여하지 않는 이상 이 Button의 DOM 요소에 접근할 일이 잘 없기때문에 당연하게도 ref를 얻을 일도 잘 없습니다. 하지만 이 컴포넌트와 비슷한 형태로 사용되는 컴포넌트가 재사용성이 매우 높은 말단 요소에서는 불편할 수 있습니다. 
    
    애플리케이션 전체에서 사용되며 접근성의 향상이나 애니메이션 같은 작업을 처리하기 위해서는 컴포넌트 내의 button DOM 노드에 직접적으로 접근하는 것이 불가피할 수 밖에 없습니다. Comment, FeedStory 같은 변함없이 일정한 형태를 유지하면서 재사용이 되는 컴포넌트라면 큰 의미가 없는것은 당연합니다.

```javascript
import React, { useRef } from "react";

function Input({ ref }) {
  return <input type="text" ref={ref} />;
}

function Field() {
  const inputRef = useRef(null);

  function handleFocus() {
    inputRef.current.focus();
  }

  return (
    <>
      <Input ref={inputRef} />
      <button onClick={handleFocus}>입력란 포커스</button>
    </>
  );
}
```

    얼핏 보기에 위 예제는 문제가 없어 보입니다. props를 전달하는 것처럼 ref를 전달하고 전달받은 ref를 이용해 DOM 요소에 접근하고 있습니다. 
    하지만 위 예제는 에러를 불러오게 됩니다. 
    ref는 절대로 props가 아니기 때문에 undefined로 자동적으로 설정이되고 그렇게 전달된 ref는 당연히 undefined이므로 에러를 발생시킵니다. 
    
    forwardRef를 사용한다면 에러를 피하면서 ref라는 이름으로 전달이 가능합니다. 

```javascript
const FancyButton = React.forwardRef((props, ref) => (
  <button ref={ref} className="FancyButton">
    {props.children}
  </button>
));

// 이제 DOM 버튼으로 ref를 작접 받을 수 있습니다.
const ref = React.createRef();
<FancyButton ref={ref}>Click me!</FancyButton>;
```

## forwardRef 사용시 주의사항
    forwardRef를 사용할 때는 주의사항이 있습니다.
    
    첫 번째 주의사항은 컴포넌트 라이브러리들을 반드시 새로운 버젼으로 설치하는 것입니다. 이는 forwardRef 때문에 라이브러리들을 에러를 발생시킬 가능성이 높고 특히 이전 동작에 의존하는 앱이나 다른 라이브러리들이 손상될 가능성이 크기 때문입니다. 
    
    두 번째는 조건적부적으로 React.forwardRef를 사용하는 것입니다. React 자체를 업데이트할때 애플리케이션을 손상시킬 가능성이 있습니다. 

## 고차원 컴포넌트에서의 ref 전달하기
    HOC는 props를 래핑되는 컴포넌트에 전달합니다. 하지만 ref는 props가 아니기 때문에 React에서는 다르게 처리됩니다. ref는 래핑되는 컴포넌트가 아닌 바깥쪽 컨테이너 컴포넌트를 참조하게 됩니다. 그렇기 때문에 ref.current.focus 같이 ref를 활용해 DOM에 접근하는 작업을 호출할 수 없습니다.
    
    다행히도 forwardRef를 사용하여 내부 컴포넌트에 대한 refs를 명시적으로 전달할 수 있습니다.



```javascript
export default function withHOC(WrappedComponent, someProp) {
  ...code
  return forwardRef((props, ref) => (
    <WrappedComponent {...props} ref={ref} someProp={someProp} />
  ));
}
```



## DevTools에 사용자 정의 이름 표시하기

    React.forwardRef는 렌더링 함수를 인자로 받습니다. React DevTools는 이 함수를 사용하여 ref 전달 컴포넌트에 대해서 무엇을 표시할 것인지 정의합니다.

### forwardRef() 함수에 이름이 있는 함수 넘기기
    첫 번째 방법은 forwardRef() 함수에 익명 함수를 대신에 이름이 있는 함수를 넘깁니다.

```javascript
const Input = forwardRef(function Input(props, ref) {
  return <input type="text" ref={ref} />;
});
```

### forwardRef() 함수의 호출 결과로 기존 컴포넌트를 대체 
    두 번째 방법은 forwardRef() 함수의 호출 결과로 기존 컴포넌트를 대체합니다.

```javascript
function Input(props, ref) {
  return <input type="text" ref={ref} />;
}

Input = forwardRef(Input);
```

### forwardRef() 함수의 호출 결과의 displayName 속성 설정
    마지막 방법은 forwardRef() 함수의 호출 결과의 displayName 속성에 컴포넌트 이름을 설정해줍니다.

```javascript
const Input = forwardRef((props, ref) => {
  return <input type="text" ref={ref} />;
});

Input.displayName = "Input";
```

---
## Reference
- [리액트 HOC 집중 탐구 (1)](https://meetup.toast.com/posts/137)
- [forwardRef 사용법]()
- [ref란? - DOM에 이름 달기](https://chanhuiseok.github.io/posts/react-7/)

