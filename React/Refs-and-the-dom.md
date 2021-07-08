# refs-and-the-dom



Refs는 render 메서드에서 생성 된 DOM 노드 또는 React 요소에 액세스하는 방법을 제공합니다.

일반적인 React 데이터 흐름에서 [props](https://reactjs.org/docs/components-and-props.html) 는 부모 구성 요소가 자식과 상호 작용하는 유일한 방법입니다. 자식을 수정하려면 새 소품으로 다시 렌더링합니다. 그러나 일반적인 데이터 흐름 외부에서 자식을 강제로 수정해야하는 경우가 몇 가지 있습니다. 수정 될 자식은 React 컴포넌트의 인스턴스이거나 DOM 요소 일 수 있습니다. 이 두 경우 모두 React는 탈출구를 제공합니다.



###  When to Use Refs



```javascript
import React, { PureComponent } from 'react';

class HabitAddForm extends PureComponent {
  formRef = React.createRef();
  inputRef = React.createRef();

  onSubmit = (event) => {
    event.preventDefault();
    const name = this.inputRef.current.value;
    name && this.props.onAdd(name);
    // this.inputRef.current.value = '';
    this.formRef.current.reset();
  };
  render() {
    return (
      <form ref={this.formRef} className="add-form" onSubmit={this.onSubmit}>
        <input
          ref={this.inputRef}
          type="text"
          className="add-input"
          placeholder="Habit"
        />
        <button className="add-button">Add</button>
      </form>
    );
  }
}

export default HabitAddForm;

```



저희는 User가 input에 입력한 값을 이용해서 어떤 기능을 수행하려고 합니다. input에 입력된 데이터를 알아야 하는데, DOM에서는 기본적으로 document.queryselector를 이용해서 input필드를 받아온 다음, input에 있는 value를 알아왔지만 리액트에서는 이 대신에 Ref라는 것을 이용해서 input의 value를 받아올 수 있다. 

 즉,  리액트에서는 다른 리액트의 요소에 접근하고 싶다면 Ref를 쓰면 된다.

우선 우리는 input에 접근해야하기 때문에 inputRef라는 멤버변수를 입력해준다. 그 후 리액트가 내장하고 있는

createRef() 라는 함수를 호출하면 Ref라는 오브젝트가 생긴다. 

```javascript
 inputRef = React.createRef();
```

그리고 이것을 원하는 요소의 Ref에다가 전달해 주면 된다.



```javascript
		<input
          ref={this.inputRef}
          type="text"
          className="add-input"
          placeholder="Habit"
        />
```



따라서 컴포넌트가 브라우저에 표기가 되면 이 input이라는 요소가 inputRef이라는 멤버변수와 연결이 되고 접근이 가능하게 되서, 해당하는 데이터를 참고하여 읽어올 수가 있다. 콘솔 로그를 이용해서 this.inputRef 안에 있는 current(현재 있는) 요소의 value를 읽어 와보자.

```javascript
onSubmit = (event) => {
    event.preventDefault();
    console.log(this.inputRef.current.value);
  };
```

![Reeeef](C:\Users\User\Desktop\효범\커넵\목요일 스터디\20210708\Reeeef.JPG)

잘 읽어오는 것을 확인할 수 있다.



이 createRef는 우리가 브라우저에서 DOM요소에 접근해서 그 요소의 value난 클릭 이벤트와 같은 것들을 등록했던 것처럼 리액트는 바로 DOM요소에 접근하지 않고 필요할 때는 리액트에서 제공하는 createRef라는 것을 이용해서 멤버변수를 정의한 다음, 그것을 해당하는 원하는 리액트 컴포넌트에 ref로 연결하면 된다.



이를 한번 더 복습하기 위해서 formRef를 이용해 form을 초기화함으로써 위 input을 이용한 것과 똑같은 기능을 구현할 수도 있다. 

```javascript
onSubmit = (event) => {
    event.preventDefault();
    const name = this.inputRef.current.value;
    name && this.props.onAdd(name);
    // this.inputRef.current.value = '';
    this.formRef.current.reset();
  };


<form ref={this.formRef} className="add-form" onSubmit={this.onSubmit}>

```

