# React
- 정의
- 특징
  - 가상 돔(Virtual DOM)
    - 가상 돔의 정의 및 효과
    - 기존 DOM에 대한 개념과 이해

## You can answer
- React의 정의를 알 수 있다.
- React의 특징 중 하나인 가상 돔을 이해하고 이에 대한 효과를 알 수 있다.

---
## 정의

- 싱글 페이지 어플리케이션의 UI를 생성하는데 집중한 라이브러리이다.
    - 페이지 전환 기능을 제공하지 않기에 react-router와 같은 추가적인 라이브러리를 이용해야하는 등다른 프레임워크에 비해 부족한 부분이 존재한다.
- Javascript에서 HTML을 포함하는 JSX(Javscript XML)으로 구성된다.
- 가상 돔(Virtual DOM)을 이용해 웹 어플리케이션의 퍼포먼스를 최적화한 라이브러리이다.

---
## 특징
### 가상 돔(Virtual DOM) 이란?
  - 기존 DOM과 동일한 DOM을 메모리 상에 만든 것.
  - DOM의 조작이 발생하면 가상 DOM에서 모든 연산을 수행하여 그 결과를 실제 DOM에 전달하여 Reflow/Repainting 횟수를 줄임.
<br/>

#### DOM(Document Object Model)?
    - 웹 페이지를 이루는 태그들(html, head, body..)을 Javascript가 이용할 수 있게끔 브라우저가 트리구조로 만든 객체 모델
    - 즉, HTML/CSS와 Javasript를 서로 이어주는 역할을 한다.

#### 브라우저 렌더링 과정
<img src="./image/rendering%20process.png"/>

    1. HTML 파싱으로 브라우저가 알 수 있는 형태로 가공하여 DOM 노드로 이루어진 트리를 생성함.
    2. CSS 파일이나 HTML의 요소들을 인라인 스타일을 파싱하여 CSSOM과 같은 트리를 구성함.
    3. Attachment 과정을 통해 HTML의 각 태그에 맞는 스타일 정보를 객체 형태로 반환함.
    4. Render Tree 단계에서 화면에 표시되는 모든 노드의 콘텐츠 및 스타일 정보를 저장함.
    4-1. 만약 외부 CSS 파일이 존재하여 Render Tree에 새로운 노드가 추가된다면 이를 필요료하는 각 HTML의 요소들을 결함하고 이전에 Attachment에서 받은 스타일 값들을 계산함.
    5. Layout 단계에서 Render Tree의 각 노드들에 좌표를 부여함.
    6. 마지막 Painting 과정으로 각 노드들에 색상을 입힘.

#### 리플로우(Reflow), 리페인팅(Repainting)?
    Reflow나 Repainting이 수행되면 DOM의 각 노드에 대한 연산 과정을 다시 수행해야한다.   
    그러므로, 이 둘은 웹 서비스 성능에 큰 영향을 미친다.

    Reflow(리플로우) : DOM의 조작으로 레이아웃 과정이 다시 진행되는 것을 의미함.   
    Repainting(리페인팅): 페인팅 과정이 다시 진행되는 것을 의미함.




