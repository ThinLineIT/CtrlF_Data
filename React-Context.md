

# React - Context

- What Is The Context API?
- useContext
- Conclusion
- Advanced

## You can answer

- What is context?
- How can we use context?



---

## What Is The Context API?

​	React는 여러 중첩 구성 요소에 걸쳐 있는 상태에 대해 매우 필요한 솔루션으로 Context API를 출시했습니다. 불행히도 컨텍스트용 API는 약간 부피가 커서 클래스 구성 요소에서 사용하기 어려웠습니다. React Hooks가 출시되면서 React 팀은 Context와 상호 작용하는 방법을 재고했고, useContext 후크를 사용하여 코드를 대폭 단순화하기로 결정했습니다.



​	이미 알고 있듯이 React는 state를 사용하여 데이터를 저장하고 props를 사용하여 구성 요소 간에 데이터를 전달합니다. 이것은 로컬 상태를 처리하고 부모/자식 구성 요소 간에 간단한 소품을 전달하는 데 적합합니다. 하지만 이러한 시스템은 깊이 중첩된 구성 요소에 전달해야 하는 과정을 갖기 시작하면 에러 발생이 일어날 수 있습니다. 결국 props와 state만 있으면 props 드릴에 의존해야 합니다. 이것은 props를 여러 구성 요소를 통해 전달하여 계층 구조에서 멀리 떨어진 하나의 단일 구성 요소에 도달할 수 있도록 하는 것입니다.

여기에서 컨텍스트 API가 필요합니다. 컨텍스트 API를 사용하면 각 구성 요소를 통해 이 데이터를 전달할 필요 없이 컨텍스트 내부에 중첩된 모든 구성 요소에서 사용할 수 있는 특정 데이터 조각을 지정할 수 있습니다. 이는 본질적으로 컨텍스트 내 어디에서나 사용할 수 있는 반 전역 상태입니다. 다음은 예시를 보겠습니다.



```jsx
const ThemeContext = React.createContext()

function App() {
  const [theme, setTheme] = useState('dark')

  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      <ChildComponent />
    </ThemeContext.Provider>
  )
}
```

```jsx
function ChildComponent() {
  return <GrandChildComponent />
}
```

```jsx
class GrandChildComponent {
  render() {
    return (
      <ThemeContext.Consumer>
        {({ theme, setTheme }) => {
          return (
            <>
              <div>The theme is {theme}</div>
              <button onClick={() => setTheme('light')}>
                Change To Light Theme
              </button>
            </>
          )
        }}
      </ThemeContext.Consumer>
    )
  }
}
```



​	위의 코드에서 우리는 **React.createContext**를 사용하여 새로운 컨텍스트를 생성하고 있습니다. 이것은 두 부분으로 된 변수를 제공합니다.



​	첫 번째 부분은 내부에 중첩된 모든 구성 요소에 값을 제공하는 공급자**Provider**입니다. 우리의 경우 값은 **theme** 및 **setTheme** 속성이 있는 단일 객체입니다.



​	두 번째 부분은 소비자**Consumer**입니다. 이것은 **Context**의 값에 액세스하기 위해 코드를 래핑해야 하는 것입니다. 이 구성 요소는 함수의 자식으로 함수를 기대하고 해당 함수는 함수에 대한 유일한 인수로 **Context value**를 제공합니다. 그런 다음 해당 함수에서 컨텍스트를 활용하는 JSX를 반환할 수 있습니다.



​	**Context**의 이 두 번째 부분은 **Context**를 작업하기 어렵게 만듭니다. **Context value**를 가져오는 기능을 허용하는 구성 요소에서 JSX를 래핑해야 하는 것은 클래스 구성 요소에서 피할 수 없는 코드에 중첩 및 혼란을 추가시킬 수 있습니다. 따라서 **React Hooks **의 **useContext** 를 사용하여 모든 혼란을 피할 수 있습니다.



## useContext



함수 구성 요소에서 **Context**를 사용하기 위해 더 이상 **Consumer**에서 JSX를 래핑할 필요가 없습니다. 대신 **Context**에 **useContext** 를 전달하기만 하면 됩니다. 다음은 예시입니다.



```jsx
function GrandChildComponent() {
  const { theme, setTheme } = useContext(ThemeContext)
  return (
    <>
      <div>The theme is {theme}</div>
      <button onClick={() => setTheme('light')}>
        Change To Light Theme
      </button>
    </>
  )
}
```



**useContext**의 도움으로 컨텍스트의 모든 **Consumer** 부분을 잘라내고 모든 복잡한 중첩을 제거할 수 있었습니다. 이제 컨텍스트는 컨텍스트를 호출하는 일반 함수처럼 작동하며 나중에 코드에서 사용할 수 있도록 내부 값을 제공합니다. 이것은 컨텍스트와 관련된 코드를 크게 단순화하고 컨텍스트 작업을 훨씬 더 즐겁게 만듭니다.



또한 **useContext** 후크와 함께 사용할 컨텍스트 제공자를 설정하는 것은 일반 컨텍스트 **Consumer**에 대해 수행하는 것과 정확히 동일하므로 시작 부분에서 클래스 구성 요소 예제의 컨텍스트 제공자 부분에 대해 동일한 코드를 모두 사용할 수 있습니다.



## Conclusion



결국 **useContext** 후크는 매우 간단합니다.  **useContext** 후크가 하는 일은 **Context** 를 알맞게 사용하기 위한 것뿐이지만 이러한 일을 함으로써 원래의 컨텍스트 **Consumer** 보다 훨씬 나은 편리성을 갖습니다. 따라서 우리는 애플리케이션에서 **Context** 작업을할 때 **useContext** 를 시도해서 작업을 해야 합니다.







## Advanced

한 단계 더 나아가서,

위의 예제에서 실제로 더욱 코드 훨씬 더 단순화할 수 있습니다.

App.js 내부에 저장되어 있는 모든 것을 자체 사용자 정의 후크로 추출하는 방법을 이용하면 됩니다.

ThemeContext라는 새로운 파일을 만들고, 이 안에 모든 내용들을 추출하여 App 파일의 간결함을 유지하겠습니다. 이 ThemeContext는 Theme에 대한 모든 정보와 논리를 추출할 것입니다.  이는 ThemeContext라는 하나의 단일 구성 요소 내부에 Theme에 대한 정보를 저장하고 가져올 수 있게 합니다. 이 안에서 useState를 이용하여 상태를 업데이트 하며 사용자 정의 후크를 갖습니다. 이는 다양한 값에 쉽게 액세스할 수 있게 합니다. 이는 Context 처리를 위한 모든 논리를 래핑합니다. 그 후, 구성 요소 내부에서 사용자 정의 후크를 사용해주기만 하면 됩니다. 

생성되는 모든 Context는 이 기본 템플릿을 생성하여 사용할 수 있습니다. 이를 통해선 우린 어떻게 반응해야하고 처리해야하는 복잡성에 대해 걱정할 필요가 없어지는 장점을 갖고 따라서 나머지 코드를 작업하기 쉽고 니다.



이는 단지 App.js에  ThemeContext를 import 해줌으로써 간편하게 이용 가능합니다.

```jsx
import { ThemeProvider } from './ThemeContext';
```

이제 우리는 Theme 제공자를 가지고 있습니다. 이를 통해 우리의 Theme과 기능에 대한 엑세스 권한을 순서대로 제공할 것입니다. 즉, Theme를 업데이트하고 children을 렌더링할 것입니다.



따라서 이 모든 코드를 한 곳으로 대폭 단순화하면 복잡한 처리에 대해 걱정할 필요가 없습니다. App.js는 이 ThemeProvider만 대폭 간소화하여 제공하며, 그 안에 원하는 모든 것을 넣을 수 있으며 그 뒤에는  ThemeProvider가 자체 내부 상태를 업데이트하거나 렌더링하는 것처럼 모든 일들을 처리합니다.



하지만 이를 실제로 동작하게 하기 위해선  ChildComponent 내부에서 

ThemeProvider에 엑세스 권한을 갖도록 해야합니다.



우리는 useTheme()이라는 후크를 생성하고, 단순히 ThemeContext의 사용 컨텍스트를 반환하게 하면 됩니다.

```jsx
export function useTheme() {
  return useContext(ThemeContext);
}
```

이는 기본적으로 ThemeContext를 래핑하여 내부 컨텍스트를 사용하게 합니다.

useTheme()이라는 자체 후크를 사용하면 걱정할 필요가 없습니다.

외부 파일에서 이 ThemeContext에 엑세스하면 정확한 작업을 수행할 수 있습니다.



이제는 ChildComponent에 가서 다음과 같은 작업을 수행하여 useTheme을 가져와, ThemeContext에 엑세스가 가능하게 합니다.

```jsx
// ChildComponent.js

import { useTheme, useThemeUpdate } from './ThemeContext';

export default function ChildComponent() {
  const darkTheme = useTheme();
  const toggleTheme = useThemeUpdate();
    /.. 생략 ../
```

이제 토글 Theme이 올바르게 토글되고 작동할 것입니다.





## Finished Code





```jsx
// App.js

import React from 'react';
import ChildComponent from './ChildComponent';
import { ThemeProvider } from './ThemeContext';

export default function App() {
  return (
    <ThemeProvider>
      <ChildComponent />
    </ThemeProvider>
  );
}

```



```jsx
// ThemeContext.js

import React, { useContext, useState } from 'react';

const ThemeContext = React.createContext();
const ThemeUpdateContext = React.createContext();

export function useTheme() {
  return useContext(ThemeContext);
}

export function useThemeUpdate() {
  return useContext(ThemeUpdateContext);
}

export function ThemeProvider({ children }) {
  const [darkTheme, setDarkTheme] = useState(true);

  function toggleTheme() {
    setDarkTheme((prevDarkTheme) => !prevDarkTheme);
  }

  return (
    <ThemeContext.Provider value={darkTheme}>
      <ThemeUpdateContext.Provider value={toggleTheme}>
        {children}
      </ThemeUpdateContext.Provider>
    </ThemeContext.Provider>
  );
}
```

```jsx
// ChildComponent.js

import React from 'react';
import { useTheme, useThemeUpdate } from './ThemeContext';

export default function ChildComponent() {
  const darkTheme = useTheme();
  const toggleTheme = useThemeUpdate();
  const themeStyles = {
    backgroundColor: darkTheme ? '#333' : '#CCC',
    color: darkTheme ? '#CCC' : '#333',
    paddng: '2rem',
    margin: '2rem',
  };
  return (
    <>
      <button onClick={toggleTheme}>Toggle Theme</button>
      <div style={themeStyles}>Function Theme</div>
    </>
  );
}

```





