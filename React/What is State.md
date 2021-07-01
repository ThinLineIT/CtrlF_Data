# State
- 정의
- 특징
- 예제(클래스형 컴포넌트, 함수형 컴포넌트)

# You can answer
- State가 무엇인지 알 수 있다.
- State의 쓰임을 알 수 있다.
  
## 정의
- 값을 저장하거나 변경할 수 있는 객체이다.
- 보통 버튼을 클릭하거나 값을 입력하는 등의 이벤트와 함께 사용된다.   
    ex) 버튼을 눌렀을때 버튼 색을 변경하거나, 글씨 모양을 변경할 때 사용된다.

## 특징
1. 클래스 컴포넌트 사용 시, 생성자(constructor)에서 반드시 초기화해야 한다.   
    - 초기화 하지 않으면, 내부 함수에서 state값을 접근할 수 없다.
2. 클래스 컴포넌트 사용 시, setState()를 이용해 값을 변경해야한다.
    - render() 함수로 화면을 그려주는 시점은 리액트 엔진이 정하기 때문이다.
    - 즉, state를 직접 변경해도 render() 함수는 호출되지 않는다.
3. setState()는 비동기로 처리되며, setState() 코드 이후로 연결된 함수들의 실행이 완료된 시점에 화면 동기화 과정을 거친다.

<br/>

## 예시(State를 이용해 4초후 값 변경하는 예제)
<br/>

### 1. StateExample.js (클래스형 컴포넌트)
```jsx
import React from 'react';

class StateExample extends React.Compoent{
    constructor(props){
        super(props);
        //state 정의
        this.state={
            loading:'로딩중...',
            formData:'no data',.
        };
        //콜백 함수에서 bind(this)에 대해 설명하겠음.
        this.handleData = this.handleData.bind(this)
    }
    componentDidMount(){
        // 4초 후 handleData 함수 호출
    }
    handleData(){
        const data = 'Hello! React';

        //state 변경
        this.setState({
            loading:'로딩 완료',
            formData:data,
        });
    }

    render(){
        return(
            <div>
                <span>로딩:{String(this.state.loading)}</span>
                <div/>
                <span>결과:{this.state.formData}</span>

            </div>
        )
    }
}
export default StateExample;
```
App.js
```jsx
import StateExample from './StateExample';

function App(){
    return(
        <div>
            <StateExample/>
        </div>
    )
}
export default App;
```
### 2. FnStateExample.js(함수형 컴포넌트)
```jsx
import  React, {useEffect, useState} from 'react';

function FnStateExample(){
    // useEffect()는 컴포넌트가 렌더링 될때마다 특정 작업을 수행하도록 하는 Hook이다.
    // 클래스형 컴포넌트의 componentDidMount와 componentDidUpdate를 합친 형태로 보아도 무방하다.
    // useEffect()에서 특정 값이 변경 될 때에만 실행하고 싶은 경우 => 두번째 파라미터로 [해당 값] 형식으로 입력.
    // useEffect()에서 가장 처음 렌더링 될 때만 실행하고 싶은 경우 => 두번째 파라미터 [](빈 배열)을 입력.
    useEffect(()=>{
        // 처음 렌더링이 되고 4초 후에 handleData() 실행
        setTimeout(()=>handleData()),4000)
    },[])

    // 함수형 컴포넌트는 Hook의 useState()로 state를 관리한다.
    const [state,setState] = useState({
        loading:true,
        formData:'no data',
    });

    const handleData = () =>{
        setState({
            loading:false,
            formData:'Hello! React'
        });
    }

    return(
        <div>
            <span>로딩:{String(state.loading)}</span>
            <div/>
            <span>결과:{state.formData}</span>
        </div>
    )
}
export default FnStateExample;
```
App.js
```jsx
import FnStateExample from './FnStateExample';

function App(){
    return(
        <div>
            <FnStateExample/>
        </div>
    );
}
export default App;
```
3. 실행 결과  
<img src="./image/state_test.gif"></img>
