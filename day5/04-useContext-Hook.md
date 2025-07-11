# 04. useContext Hook

React에서 컴포넌트 간에 데이터를 전달하는 가장 일반적인 방법은 `props`를 사용하는 것입니다. 부모 컴포넌트에서 자식 컴포넌트로 데이터를 전달하고, 그 자식 컴포넌트가 또 다른 자식 컴포넌트로 데이터를 전달하는 방식으로 데이터를 아래로 흘려보냅니다.

하지만 컴포넌트 트리가 깊어질수록, 멀리 떨어진 자식 컴포넌트에 데이터를 전달하기 위해 중간에 있는 여러 컴포넌트들이 필요 없는 `props`를 계속해서 전달해야 하는 상황이 발생할 수 있습니다. 이러한 문제를 <strong>Prop Drilling (프롭 드릴링)</strong>이라고 부릅니다.

## 1) Prop Drilling 문제

**Prop Drilling**은 데이터를 실제로 필요로 하는 컴포넌트까지 도달하기 위해, 중간에 있는 여러 컴포넌트들이 해당 데이터를 `props`로 받아서 다시 다음 자식 컴포넌트로 전달하는 과정을 의미합니다.

예를 들어, `App` 컴포넌트에서 `User` 정보를 가지고 있고, 이 `User` 정보를 `Header` 컴포넌트 안에 있는 `Avatar` 컴포넌트에서 사용해야 한다고 가정해 보겠습니다.

```jsx
// App.jsx
function App() {
  const user = { name: '김코딩', avatarUrl: 'https://example.com/avatar.jpg' };
  return (
    <Header user={user} /> {/* Header에 user 정보를 전달 */}
  );
}

// Header.jsx
function Header({ user }) { {/* Header는 user 정보를 받지만, 직접 사용하지는 않습니다. */}
  return (
    <div>
      <h1>환영합니다!</h1>
      <Avatar user={user} /> {/* Avatar에 user 정보를 다시 전달 */}
    </div>
  );
}

// Avatar.jsx
function Avatar({ user }) { {/* Avatar는 user 정보를 받아서 사용합니다. */}
  return (
    <img src={user.avatarUrl} alt={user.name} />
  );
}
```

위 예시에서 `Header` 컴포넌트는 `user` 정보를 직접 사용하지 않으면서도, `Avatar` 컴포넌트에 전달하기 위해 `user` `props`를 받아야 합니다. 컴포넌트 트리가 더 깊어진다면, 이러한 중간 전달 과정이 많아져 코드를 이해하고 유지보수하기 어렵게 만듭니다.

> [!CAUTION]
> **Prop Drilling의 단점**
> *   **코드 가독성 저하**: 데이터가 어디서 왔고 어디로 가는지 추적하기 어려워집니다.
> *   **유지보수 어려움**: 중간 컴포넌트가 필요 없는 `props`를 가지고 있으므로, 데이터 구조가 변경되면 여러 컴포넌트를 수정해야 할 수 있습니다.
> *   **재사용성 저하**: 특정 `props`에 의존하게 되어 컴포넌트의 재사용이 어려워집니다.

## 2) Context API란?

React의 **Context API**는 이러한 Prop Drilling 문제를 해결하기 위해 고안되었습니다. Context API를 사용하면 컴포넌트 트리의 모든 레벨에 있는 컴포넌트들이 데이터를 명시적으로 `props`로 전달하지 않고도, **전역적으로 데이터를 공유**할 수 있게 해줍니다.

Context API는 마치 React 애플리케이션 안에 <strong>모두가 접근할 수 있는 '공유 보관함'</strong>을 만드는 것과 같습니다. 이 보관함에 데이터를 넣어두면, 보관함에 접근할 수 있는 모든 컴포넌트들이 그 데이터를 꺼내 사용할 수 있습니다.

> [!NOTE]
> Context API는 주로 다음과 같은 **전역적인 데이터**를 공유할 때 사용됩니다.
> *   현재 로그인한 사용자 정보
> *   애플리케이션 테마 (다크 모드/라이트 모드)
> *   언어 설정

## 3) `useContext` Hook 사용법

Context API를 사용하기 위해서는 크게 세 가지 단계를 거칩니다.

1.  **Context 생성**: `createContext` 함수를 사용하여 Context 객체를 만듭니다.
2.  **Context 제공**: `Context.Provider` 컴포넌트를 사용하여 Context 값을 제공합니다.
3.  **Context 사용**: `useContext` Hook을 사용하여 Context 값을 읽어옵니다.

### 3-1) Context 생성: `createContext`

먼저 `React`에서 `createContext` 함수를 `import`하여 Context 객체를 생성합니다. 이 객체는 `Provider`와 `Consumer` (또는 `useContext` Hook)를 포함합니다.

```jsx
// src/contexts/ThemeContext.js (새로운 파일 생성)
import { createContext } from 'react';

// ThemeContext라는 이름의 Context 객체를 생성합니다.
// createContext() 안에 전달하는 값은 Context의 기본값입니다.
// 이 값은 Provider가 제공되지 않았을 때 사용됩니다.
export const ThemeContext = createContext('light'); // 기본 테마를 'light'로 설정
```

> [!TIP]
> Context 파일을 별도로 분리하여 관리하는 것이 좋습니다. 보통 `src/contexts` 폴더 안에 생성합니다.

### 3-2) Context 제공: `Context.Provider`

생성된 Context 객체에는 `Provider`라는 컴포넌트가 포함되어 있습니다. 이 `Provider` 컴포넌트를 사용하여 Context 값을 제공할 컴포넌트들을 감싸줍니다. `value` `props`를 통해 실제로 공유할 데이터를 전달합니다.

```jsx
// App.jsx
import React, { useState } from 'react';
import { ThemeContext } from './contexts/ThemeContext'; // 생성한 Context를 import
import Toolbar from './Toolbar'; // Toolbar 컴포넌트 (아래에서 생성)

function App() {
  const [theme, setTheme] = useState('light'); // 현재 테마 상태

  const toggleTheme = () => {
    setTheme(prevTheme => (prevTheme === 'light' ? 'dark' : 'light'));
  };

  return (
    // ThemeContext.Provider로 감싸진 모든 하위 컴포넌트는 theme 값을 사용할 수 있습니다.
    <ThemeContext.Provider value={theme}>
      <button onClick={toggleTheme}>테마 변경</button>
      <Toolbar />
    </ThemeContext.Provider>
  );
}

export default App;
```

`ThemeContext.Provider`로 감싸진 `Toolbar` 컴포넌트와 그 하위의 모든 컴포넌트들은 `theme` 값을 `props`로 전달받지 않고도 직접 접근할 수 있게 됩니다.

### 3-3) Context 사용: `useContext` Hook

이제 `Provider`가 제공한 Context 값을 실제로 사용할 차례입니다. `useContext` Hook을 사용하면 컴포넌트에서 Context 값을 쉽게 읽어올 수 있습니다.

```jsx
// Toolbar.jsx
import React from 'react';
import ThemedButton from './ThemedButton'; // ThemedButton 컴포넌트 (아래에서 생성)

function Toolbar() {
  return (
    <div>
      <ThemedButton /> {/* ThemedButton은 theme 값을 props로 받지 않습니다. */}
    </div>
  );
}

export default Toolbar;

// ThemedButton.jsx
import React, { useContext } from 'react';
import { ThemeContext } from './contexts/ThemeContext'; // Context를 import

function ThemedButton() {
  // useContext Hook을 사용하여 ThemeContext의 현재 값을 읽어옵니다.
  // 이 값은 ThemeContext.Provider의 value prop으로 전달된 값입니다.
  const theme = useContext(ThemeContext);

  return (
    <button style={{ background: theme === 'dark' ? '#333' : '#eee', color: theme === 'dark' ? 'white' : 'black' }}>
      현재 테마: {theme}
    </button>
  );
}

export default ThemedButton;
```

`ThemedButton` 컴포넌트는 `props`를 통해 `theme` 값을 전달받지 않고, `useContext(ThemeContext)`를 사용하여 `App` 컴포넌트에서 제공한 `theme` 값을 직접 가져와 사용합니다.

## 4) `useContext`를 사용한 예제: 테마 변경 기능

위에서 설명한 내용을 바탕으로, `useContext`를 사용하여 애플리케이션의 테마를 변경하는 전체 예제를 살펴보겠습니다.

**1. Context 파일 생성 (`src/contexts/ThemeContext.js`)**

```jsx
// src/contexts/ThemeContext.js
import { createContext } from 'react';

export const ThemeContext = createContext('light'); // 기본값 'light'
```

**2. `App` 컴포넌트 (`src/App.js`)**

`App` 컴포넌트에서 `theme` 상태를 관리하고, `ThemeContext.Provider`를 통해 이 값을 하위 컴포넌트에 제공합니다.

```jsx
// src/App.js
import React, { useState } from 'react';
import { ThemeContext } from './contexts/ThemeContext';
import FunctionComponent from './FunctionComponent'; // 하위 컴포넌트

function App() {
  const [theme, setTheme] = useState('light'); // 'light' 또는 'dark'

  const toggleTheme = () => {
    setTheme(currentTheme => (currentTheme === 'light' ? 'dark' : 'light'));
  };

  return (
    <ThemeContext.Provider value={theme}>
      <div style={{ padding: '20px', border: '1px solid #ccc', margin: '20px' }}>
        <h2>App 컴포넌트</h2>
        <p>현재 테마: {theme}</p>
        <button onClick={toggleTheme}>테마 전환</button>
        <FunctionComponent />
      </div>
    </ThemeContext.Provider>
  );
}

export default App;
```

**3. `FunctionComponent` (`src/FunctionComponent.js`)**

`FunctionComponent`는 `theme` 값을 직접 사용하지 않고, `ChildComponent`에 전달합니다. (이전의 `Header` 역할)

```jsx
// src/FunctionComponent.js
import React from 'react';
import ChildComponent from './ChildComponent'; // 하위 컴포넌트

function FunctionComponent() {
  return (
    <div style={{ padding: '15px', border: '1px solid #aaa', margin: '15px' }}>
      <h3>FunctionComponent</h3>
      <ChildComponent />
    </div>
  );
}

export default FunctionComponent;
```

**4. `ChildComponent` (`src/ChildComponent.js`)**

`ChildComponent`는 `useContext`를 사용하여 `theme` 값을 직접 읽어와 사용합니다.

```jsx
// src/ChildComponent.js
import React, { useContext } from 'react';
import { ThemeContext } from './contexts/ThemeContext'; // Context import

function ChildComponent() {
  const theme = useContext(ThemeContext); // ThemeContext에서 현재 테마 값을 가져옵니다.

  const style = {
    background: theme === 'dark' ? '#555' : '#f0f0f0',
    color: theme === 'dark' ? 'white' : 'black',
    padding: '10px',
    border: '1px solid #777',
    margin: '10px'
  };

  return (
    <div style={style}>
      <h4>ChildComponent</h4>
      <p>이 컴포넌트의 테마는 "{theme}" 입니다.</p>
    </div>
  );
}

export default ChildComponent;
```

이 예제를 실행하면, `App` 컴포넌트의 버튼을 클릭할 때마다 `theme` 상태가 변경되고, `ThemeContext.Provider`를 통해 새로운 `theme` 값이 하위 컴포넌트들에게 전달됩니다. `ChildComponent`는 `props` 없이 `useContext`를 통해 이 변경된 `theme` 값을 즉시 반영하여 자신의 배경색과 글자색을 변경합니다.

## 5) `useContext` 사용 시 고려사항

`useContext`는 Prop Drilling 문제를 해결하는 강력한 도구이지만, 항상 최선의 선택은 아닐 수 있습니다.

*   **언제 사용하는 것이 좋은가?**
    *   애플리케이션 전체에서 자주 사용되는 **전역적인 데이터** (예: 사용자 인증 정보, 테마, 언어 설정)를 공유할 때 유용합니다.
    *   컴포넌트 트리가 매우 깊고, 중간 컴포넌트들이 데이터를 단순히 전달만 하는 경우에 Prop Drilling을 피할 수 있습니다.

*   **주의할 점**
    *   **성능 문제**: Context의 `value`가 변경되면, 해당 Context를 `useContext`로 사용하고 있는 모든 하위 컴포넌트들은 **모두 리렌더링**됩니다. 만약 Context에 자주 변경되는 많은 데이터를 넣는다면, 불필요한 리렌더링이 많이 발생하여 성능에 영향을 줄 수 있습니다.
    *   **재사용성**: Context에 너무 많은 데이터를 넣거나, 특정 Context에 강하게 의존하는 컴포넌트를 만들면 해당 컴포넌트의 재사용성이 떨어질 수 있습니다.
    *   **대안**: 만약 Context가 너무 자주 변경되거나, 복잡한 전역 상태 관리가 필요하다면 Redux, Zustand, Recoil과 같은 **상태 관리 라이브러리**를 고려하는 것이 좋습니다. 이들은 Context API보다 더 최적화된 방법으로 상태를 관리할 수 있습니다.

> [!IMPORTANT]
> `useContext`는 Prop Drilling을 피하기 위한 좋은 방법이지만, 모든 `props` 전달을 `useContext`로 대체해야 한다는 의미는 아닙니다. 컴포넌트 간의 명확한 데이터 흐름이 필요한 경우에는 여전히 `props`를 사용하는 것이 좋습니다.

## 6) 정리

*   **Prop Drilling**은 컴포넌트 트리가 깊어질 때, 데이터를 필요로 하는 컴포넌트까지 `props`를 계속해서 전달해야 하는 비효율적인 상황을 말합니다.
*   **Context API**는 이러한 Prop Drilling 문제를 해결하고, 컴포넌트 트리의 모든 레벨에서 데이터를 명시적인 `props` 전달 없이 전역적으로 공유할 수 있게 해줍니다.
*   `useContext` Hook은 `createContext`로 생성된 Context의 값을 컴포넌트에서 쉽게 읽어올 수 있도록 합니다.
*   `Context.Provider`는 Context 값을 제공하는 역할을 하며, `value` `props`를 통해 공유할 데이터를 전달합니다.
*   `useContext`는 전역적인 데이터를 공유할 때 유용하지만, Context 값이 자주 변경되거나 너무 많은 데이터를 포함하면 성능 문제가 발생할 수 있으므로 주의해야 합니다.

---

- [목차로 돌아가기](./README.md)
- [이전 강의로 이동](./03-useRef-Hook.md)
- [다음 강의로 이동](./05-API-Integration.md)