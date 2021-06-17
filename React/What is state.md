# LifeCycle
- Mounting
- Updating
- Unmounting

# You can answer
- 클래스형 컴포넌트의 생명주기를 이해할 수 있다.

---
<img src="./image/lifecycle.jpeg">

## Mounting
- 컴포넌트가 처음 실행될 때를 의미한다.

    ### constructor
        컴포넌트의 생성자이다.
    
    ### getDerivedStateFromProps
        - 컴포넌트가 최초로 마운트되거나 갱신될때 렌더링 직전에 호출되는 함수이다.
        - 일반적으로 추천되는 함수는 아니다.
        - 시간에 따라 변하는 props에 대해 state가 반응해야할 때 적합한 함수이다.

    ### componentDidMoount
        - 컴포넌트가 DOM에 렌더링이 된 후에 실행된다.
        - setInterval, setTimer 같은 함수나 네트워크 요청을 위한 Ajax라이브러리를 사용하는 구간이다.
  
## Updating
- props와 state가 변경될 때를 의미한다.

    ### shouldComponentUpdate
        - props나 state가 갱신되어 렌더링 발생되기 전에 호출된다.
        - 기본적으로 true를 반환(false 반환시에는 render, compnentDidUpdate가 실행X)
        - 초기 렌더링이나 forceUpdate 시에는 실행X
        - 단순히 성능최적화를 위해 사용되어야 된다.(렌더링 방지 목적으로 사용하려면 Pure Component를 추천함)

    ### getSnapshotBeforeUpdate
        - 가장 마지막으로 렌더링 결과가 DOM에 반영되었을 때 호출된다.
        - 현재 화면의 스크롤 위치 등을 알 수 있다.
    
    ### componentDidUpdate
        - DOM에 접근이 가능하다.
        - setState를 통해 상태를 변경할 수 있지만, 조건문을 사용해 무한루프에 빠지는 것에 주의해야 한다.

## Unmounting
- 컴포넌트가 제거될 때를 의미한다.

    ### componentWillUnmount
        - 주로 연결했던 이벤트 리스너를 없애주는 작업이 진행된다.

# Reference
- [PureComponent 설명](https://velog.io/@dolarge/Pure-Component%EB%9E%80)
