# Accessibility
<!--Table of Contents-->
- 접근성의 정의
    - 접근성이란?
    - 접근성이 필요한 이유
- 접근성 표준 및 지침
    - WCAG와 WAI-ARIA
- 시멘틱 HTML
- 접근성 있는 폼
    - 라벨링
    - 사용자들에게 오류 안내하기
- 포커스 컨트롤
    - 키보드 포커스와 포커스 윤곽선
    - 원하는 콘텐츠로 건너뛸 수 있는 방법
    - 프로그래밍적으로 포커스 관리하기
- 마우스와 포인터 이벤트
- 기타 고려사항
    - 언어설정
    - 문서제목설정
    - 색 대비

<!-- 어떤 질문을 대답할 수 있어야 하는지-->
## You can answer
- 접근성이란 무엇이며 왜 필요한가?
- 접근성을 향상 시킬 수 있는 요소는 무엇인가?

<!--Contents-->

---
## 접근성의 정의
### 접근성이란?
    접근성은 어떤 사이트가 접근 가능하다거나 액세스 가능하다고 할 때 그 사이트의 콘텐츠를 사용할 수 있고 말 그대로 누구든 사이트에서 제공하는 기능을 조작할 수 있음을 의미합니다. 
### 접근성이 필요한 이유
    개발자로서는 별 다른 생각 없이 모든 사용자가 키보드, 마우스 또는 터치 스크린을 보고 사용할 수 있고, 자신과 똑같은 방식으로 웹페이지 콘텐츠와 상호 작용할 수 있을 거라 여기기 십상입니다. 이런 생각을 바탕으로 웹사이트를 개발하면 어떤 사람들에게는 문제 없이 잘 작동하지만 또 어떤 사람들에게는 단순히 성가신 수준부터 아예 웹사이트를 이용하지 못할 정도의 수준까지 다양한 문제를 일으킬 수 있습니다.
    그렇기 때문에 우리는 접근성을 고려한 개발을 진행해야합니다.
    

## 접근성 표준 및 지침
### WCAG와 WAI-ARIA
    WCAG(Web Content Accessibility Guidelines)는 접근성을 갖춘 웹사이트를 만드는 데 필요한 지침을 제공합니다.
[WCAG](https://www.w3.org/WAI/standards-guidelines/wcag/)

    WAI-ARIA 문서에는 접근성을 갖춘 JavaScript 위젯을 만드는 데 필요한 기술들이 담겨있습니다.
    * JSX에서는 모든 aria-* HTML 어트리뷰트를 지원하고 있습니다.
[WAI-ARIA](https://www.w3.org/WAI/standards-guidelines/aria/)

## 시멘틱 HTML
    시멘틱이란 코드 조각의 의미를 나타냅니다. 쉽게 예시를 들자면 "이 HTML 엘리먼트가 가진 목적이나 역할은 무엇인가?"처럼 아주 작은 단위인 HTML 엘리먼트가 그 의미를 갖고 있어야함을 의미합니다.
    즉 시맨틱 HTML은 웹 애플리케이션에 있어 접근성의 기초입니다.
    종종 React로 구성한 코드가 돌아가게 만들기 위해 (<ol>, <ul>, <dl>, <div>)와 같은 엘리먼트를 사용해 HTML의 의미를 깨트리곤 합니다. 이런 경우에 React는 Fragment를 사용하여 엘리먼트를 하나로 의미있게 묶어줍니다. 

```javascript
import React, { Fragment } from 'react';

function ListItem({ item }) {
  return (
    <Fragment>
      <dt>{item.term}</dt>
      <dd>{item.description}</dd>
    </Fragment>
  );
}

function Glossary(props) {
  return (
    <dl>
      {props.items.map(item => (
        <ListItem item={item} key={item.id} />
      ))}
    </dl>
  );
}

```
    더 자세한 내용은 React의 Fragment 문서를 참고해주시기 바랍니다.

## 접근성 있는 폼
### 라벨링
    스크린 리더를 사용하는 사용자를 위해 <input>과 <textarea> 같은 모든 HTML 폼 컨트롤은 구분할 수 있는 라벨이 필요합니다.

```javascript
<label htmlFor="namedInput">Name:</label>
<input id="namedInput" type="text" name="name"/>
```

### 사용자들에게 오류 안내하기
    오류 상황은 모든 사용자가 알 수 있어야 합니다. 아래 링크는 스크린 리더에 오류 문구를 노출하는 방법을 설명합니다.

[WebAIM looks at form validation](https://webaim.org/techniques/formvalidation/)

## 포커스 컨트롤
    모든 웹 애플리케이션은 키보드만 사용하여 모든 동작을 할 수 있어야 합니다.
### 키보드 포커스와 포커스 윤곽선
    키보드 포커스는 키보드 입력을 받아들일 수 있는 DOM 내의 현재 엘리먼트를 나타냅니다. 아래 이미지와 비슷하게 포커스 윤곽선이 표시됩니다.

<img src="./image/keyboard-focus.png">

    다른 포커스 윤곽선으로 교체할 때 outline: 0과 같은 윤곽선을 제거하는 CSS를 사용합니다.

### 원하는 콘텐츠로 건너뛸 수 있는 방법
    애플리케이션은 사용자들의 키보드 탐색을 돕고 탐색 속도를 높일 수 있도록, 이전에 탐색한 영역을 건너뛸 방법을 제공해야 합니다. Skiplinks 또는 Skip Navigation Link들은 키보드 사용자가 페이지와 상호작용할 때만 표시되는 숨겨진 탐색 링크입니다. 내부의 페이지 앵커와 약간의 스타일링으로 매우 쉽게 구현할 수 있습니다.

### 프로그래밍적으로 포커스 관리하기
    React 애플리케이션들은 런타임 동안 지속해서 HTML DOM을 변경하기 때문에, 가끔 키보드 포커스를 잃거나 예상치 못한 엘리먼트에 포커스를 맞추곤 합니다.
    React에서 포커스를 지정하려면, DOM 엘리먼트에 ref를 사용하면됩니다

```javascript
class CustomTextInput extends React.Component {
  constructor(props) {
    super(props);
    // DOM 엘리먼트를 저장할 textInput이라는 ref을 생성합니다.
    this.textInput = React.createRef();
  }
  render() {
  // `ref` 콜백으로 텍스트 input DOM을 저장합니다.
  // 인스턴스 필드의 엘리먼트 (예를 들어, this.textInput)
    return (
      <input
        type="text"
        ref={this.textInput}
      />
    );
  }
}

focus() {
  // DOM API를 사용해 텍스트 input에 정확히 포커스를 맞춥니다.
  // 주의: ‘현재’의 DOM 노드에 접근하고 있습니다.
  this.textInput.current.focus();
}
```

## 마우스 포인터와 이벤트
    마우스 혹은 포인터 이벤트로 노출된 모든 기능을 키보드만으로 사용할 수 있도록 보장해야 합니다. 포인터 장치만 고려할 경우, 키보드 사용자들이 애플리케이션을 사용하지 못하는 경우가 많습니다.

<img src="./image/outerclick-with-mouse.gif">

    열린 팝오버의 바깥을 클릭해 팝오버를 닫을 수 있는 ‘외부 클릭 패턴(outside click pattern)’입니다. 일반적으로 팝오버를 닫는 click 이벤트를 window 객체에 붙여 구현합니다. 이는 키보드 사용자에게 큰 불편이 될 수 있어 onBlur와 onFocus 같은 적절한 이벤트 핸들러를 사용하여 같은 기능을 제공할 필요가 있습니다.
    
```javascript
class BlurExample extends React.Component {
  constructor(props) {
    super(props);

    this.state = { isOpen: false };
    this.timeOutId = null;

    this.onClickHandler = this.onClickHandler.bind(this);
    this.onBlurHandler = this.onBlurHandler.bind(this);
    this.onFocusHandler = this.onFocusHandler.bind(this);
  }

  onClickHandler() {
    this.setState(currentState => ({
      isOpen: !currentState.isOpen
    }));
  }

  // setTimeout을 사용해 다음 순간에 팝오버를 닫습니다.
  // 엘리먼트의 다른 자식에 포커스가 맞춰져있는지 확인하기 위해 필요합니다.
  // 새로운 포커스 이벤트가 발생하기 전에
  // 블러(blur) 이벤트가 발생해야 하기 때문입니다.
  onBlurHandler() {
    this.timeOutId = setTimeout(() => {
      this.setState({
        isOpen: false
      });
    });
  }

  // 만약 자식이 포커스를 받으면, 팝오버를 닫지 않습니다.
  onFocusHandler() {
    clearTimeout(this.timeOutId);
  }

  render() {
    // React는 블러와 포커스 이벤트를 부모에 버블링해줍니다.
    return (
      <div onBlur={this.onBlurHandler}
           onFocus={this.onFocusHandler}>
        <button onClick={this.onClickHandler}                aria-haspopup="true"
                aria-expanded={this.state.isOpen}>

          Select an option
        </button>
        {this.state.isOpen && (
          <ul>
            <li>Option 1</li>
            <li>Option 2</li>
            <li>Option 3</li>
          </ul>
        )}
      </div>
    );
  }
}
```

## 기타 고려사항
### 언어설정
    스크린 리더 소프트웨어들이 올바른 음성을 선택할 수 있도록, 페이지에 나타내야 합니다. 

[WebAIM - 문서 언어](https://webaim.org/techniques/screenreader/#language)

### 문서 제목 설정
    문서의 <title>이 현재 페이지에 대한 올바른 설명을 담아야 합니다. 이를 통해 사용자들이 현재 페이지의 맥락을 놓치지 않도록 할 수 있습니다.
    React에서는 React Document Title 컴포넌트를 사용해 설정할 수 있습니다.

### 색 대비
    읽을 수 있는 모든 글에 충분한 색 대비를 주어, 저시력 사용자들이 최대한 읽을 수 있도록 해야 합니다.

---
## Reference
- [접근성 | Web | Google Developers](https://developers.google.com/web/fundamentals/accessibility?hl=ko)
- [Semantics - 용어 사전 | MDN](https://developer.mozilla.org/ko/docs/Glossary/Semantics)