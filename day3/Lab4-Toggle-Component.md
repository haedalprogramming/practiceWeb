# Lab 2. Toggle 컴포넌트 만들기

첫 번째 실습으로 카운터 컴포넌트를 만들어 보았습니다. 이번에는 또 다른 일반적인 UI 패턴인 **토글(Toggle)** 컴포넌트를 만들어 보겠습니다. 토글은 스위치를 켜고 끄는 것처럼 두 가지 상태를 번갈아 전환하는 기능을 말합니다.

이 실습의 목표는 버튼을 클릭하여 특정 내용을 보였다가 숨겼다가 할 수 있는 간단한 토글 컴포넌트를 만드는 것입니다.

이 실습을 통해 여러분은 다음을 할 수 있게 됩니다:

*   `true`/`false` 값을 가지는 불리언(Boolean) State를 관리합니다.
*   State 값에 따라 다른 UI를 보여주는 <strong>조건부 렌더링(Conditional Rendering)</strong>을 구현합니다.
*   이전 State 값을 바탕으로 새로운 State를 안전하게 업데이트하는 방법을 배웁니다.

---

## Step-by-Step 가이드

### Step 1: `Toggle` 컴포넌트 파일 생성

1.  `src/components` 폴더 안에 `Toggle.jsx` 라는 새 파일을 생성합니다.

### Step 2: 기본 컴포넌트 코드 작성

`Toggle.jsx` 파일에 기본적인 함수형 컴포넌트 코드를 작성합니다. 토글 스위치 역할을 할 버튼과, 토글에 따라 보였다/숨겨졌다 할 텍스트를 포함합니다.

```jsx
// src/components/Toggle.jsx

function Toggle() {
  return (
    <div>
      <h2>토글</h2>
      <button>토글</button>
      <p>여기에 보이는 텍스트</p>
    </div>
  );
}

export default Toggle;
```

### Step 3: `useState`로 불리언 State 만들기

토글의 "켜짐"(보임) 또는 "꺼짐"(숨김) 상태를 저장할 State가 필요합니다. `isOn`이라는 이름의 State 변수를 만들고, 초기값을 `true` (보이는 상태)로 설정합니다.

```jsx
// src/components/Toggle.jsx

import { useState } from 'react'; // 1. useState를 import 합니다.

function Toggle() {
  // 2. isOn이라는 state 변수를 만들고, 초기값을 true로 설정합니다.
  const [isOn, setIsOn] = useState(true);

  return (
    <div>
      <h2>토글</h2>
      <button>토글</button>
      <p>여기에 보이는 텍스트</p>
    </div>
  );
}

export default Toggle;
```

### Step 4: 조건부 렌더링 구현하기

`isOn` State의 값에 따라 `<p>` 태그가 보이거나 보이지 않도록 만들어야 합니다. JSX 안에서 `{}`와 JavaScript의 논리 AND(`&&`) 연산자를 사용하면 간단하게 구현할 수 있습니다.

> [!TIP]
> **`&&`를 이용한 조건부 렌더링**
> `A && B` 라는 JavaScript 코드는 A가 `true`일 때만 B를 실행(반환)하고, A가 `false`이면 즉시 `false`를 반환합니다. React에서 `{true && <p>...</p>}` 는 `<p>`를 렌더링하고, `{false && <p>...</p>}`는 아무것도 렌더링하지 않습니다.

```jsx
// src/components/Toggle.jsx

import { useState } from 'react';

function Toggle() {
  const [isOn, setIsOn] = useState(true);

  return (
    <div>
      <h2>토글</h2>
      <button>토글</button>
      {/* isOn이 true일 때만 <p> 태그가 렌더링됩니다. */}
      {isOn && <p>여기에 보이는 텍스트</p>}
    </div>
  );
}

export default Toggle;
```

### Step 5: 이벤트 핸들러 만들고 연결하기

이제 토글 버튼을 클릭했을 때 `isOn`의 값을 `true`에서 `false`로, `false`에서 `true`로 바꿔주는 함수를 만듭니다. 그리고 그 함수를 버튼의 `onClick` 이벤트에 연결합니다.

```jsx
// src/components/Toggle.jsx

import { useState } from 'react';

function Toggle() {
  const [isOn, setIsOn] = useState(true);

  // 토글 버튼 클릭 시 실행될 함수
  const handleToggle = () => {
    // setIsOn에 현재 isOn 값의 반대(!isOn)를 넣어줍니다.
    setIsOn(!isOn);
  };

  return (
    <div>
      <h2>토글</h2>
      {/* onClick 속성에 handleToggle 함수를 연결 */}
      <button onClick={handleToggle}>토글</button>
      {isOn && <p>여기에 보이는 텍스트</p>}
    </div>
  );
}

export default Toggle;
```

### Step 6: `App.jsx`에서 `Toggle` 컴포넌트 사용하기

`Lab1`에서 만들었던 `Counter` 컴포넌트 아래에, 우리가 만든 `Toggle` 컴포넌트를 추가해 봅시다.

```jsx
// src/App.jsx

import Counter from './components/Counter';
import Toggle from './components/Toggle'; // 1. Toggle 컴포넌트를 가져옵니다.

function App() {
  return (
    <div>
      <h1>My React App</h1>
      <Counter />
      <hr /> {/* 컴포넌트 구분을 위한 수평선 추가 */}
      <Toggle /> {/* 2. Toggle 컴포넌트를 렌더링합니다. */}
    </div>
  );
}

export default App;
```

### Step 7: 결과 확인

브라우저 화면에서 '토글' 버튼을 클릭해보세요. 버튼 아래의 텍스트가 나타났다가 사라지는 것을 확인할 수 있습니다.

React Developer Tools를 열어 `Toggle` 컴포넌트를 선택하고, 버튼을 클릭할 때 `State` 값이 `true`와 `false` 사이를 오가는 것을 직접 눈으로 확인해보세요.

---

## 🚀 도전 과제 (Optional)

1.  **버튼 텍스트 변경**: 토글 상태에 따라 버튼의 텍스트가 "숨기기"와 "보이기"로 바뀌도록 수정해보세요. (힌트: 삼항 연산자 `{isOn ? '숨기기' : '보이기'}`를 사용할 수 있습니다.)

## ✅ 최종 코드

<details>
<summary><b>src/components/Toggle.jsx</b></summary>

```jsx
import { useState } from 'react';

function Toggle() {
  const [isOn, setIsOn] = useState(true);

  const handleToggle = () => {
    setIsOn(!isOn);
  };

  return (
    <div>
      <h2>토글</h2>
      <button onClick={handleToggle}>토글</button>
      {isOn && <p>여기에 보이는 텍스트</p>}
    </div>
  );
}

export default Toggle;
```

</details>

<details>
<summary><b>src/App.jsx</b></summary>

```jsx
import Counter from './components/Counter';
import Toggle from './components/Toggle';

function App() {
  return (
    <div>
      <h1>My React App</h1>
      <Counter />
      <hr />
      <Toggle />
    </div>
  );
}

export default App;
```

</details>

---

- [목차로 돌아가기](README.md)
- [이전 강의로 이동](Lab3-Counter-Component.md)
- [다음 강의로 이동](Lab5-Input-Form-Component.md)
