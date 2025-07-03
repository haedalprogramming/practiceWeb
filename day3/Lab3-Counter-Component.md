# Lab 1. Counter 컴포넌트 만들기

지금까지 배운 React의 핵심 개념(컴포넌트, JSX, State, `useState`)을 활용하여 첫 번째 실습을 진행해 보겠습니다. 이번 실습의 목표는 사용자가 버튼을 클릭하여 숫자를 올리거나 내릴 수 있는 간단한 **카운터(Counter)** 컴포넌트를 만드는 것입니다.

이 실습을 통해 여러분은 다음을 할 수 있게 됩니다:

*   `useState` Hook을 사용하여 컴포넌트의 상태를 관리합니다.
*   버튼 클릭과 같은 사용자 이벤트를 처리합니다.
*   State가 변경될 때 컴포넌트가 어떻게 다시 렌더링되는지 직접 확인합니다.

--- 

## Step-by-Step 가이드

### Step 1: 실습 환경 준비

먼저, 이전에 `Vite`를 통해 생성했던 React 프로젝트를 준비합니다. 터미널에서 프로젝트 폴더로 이동한 후, 다음 명령어를 실행하여 개발 서버를 시작해주세요.

```bash
npm run dev
```

브라우저에서 `http://localhost:5173` 주소로 접속하여 React 기본 페이지가 보이는지 확인합니다.

### Step 2: `Counter` 컴포넌트 파일 생성

1.  `src` 폴더 안에 `components` 라는 이름의 폴더가 없다면 새로 만들어주세요.
2.  `src/components` 폴더 안에 `Counter.jsx` 라는 새 파일을 생성합니다.

### Step 3: 기본 컴포넌트 코드 작성

`Counter.jsx` 파일에 가장 기본적인 함수형 컴포넌트 코드를 작성합니다. 아직 아무 기능도 없는, 제목과 숫자 0을 표시하는 간단한 구조입니다.

```jsx
// src/components/Counter.jsx

function Counter() {
  return (
    <div>
      <h2>카운터</h2>
      <p>현재 카운트: 0</p>
      <button>증가</button>
      <button>감소</button>
    </div>
  );
}

export default Counter;
```

### Step 4: `useState`로 State 만들기

이제 정적인 숫자 0 대신, 변경 가능한 State를 사용하도록 코드를 수정합니다. `useState`를 `react`로부터 가져와서 `count`라는 이름의 State 변수를 만듭니다. 초기값은 `0`으로 설정합니다.

```jsx
// src/components/Counter.jsx

import { useState } from 'react'; // 1. useState를 import 합니다.

function Counter() {
  // 2. count라는 state 변수를 만들고, 초기값을 0으로 설정합니다.
  // setCount는 count 값을 변경하는 함수입니다.
  const [count, setCount] = useState(0);

  return (
    <div>
      <h2>카운터</h2>
      {/* 3. 정적인 숫자 0 대신, count state 변수를 표시합니다. */}
      <p>현재 카운트: {count}</p>
      <button>증가</button>
      <button>감소</button>
    </div>
  );
}

export default Counter;
```

### Step 5: 이벤트 핸들러 함수 만들기

사용자가 버튼을 클릭했을 때 State를 변경하는 함수들을 만듭니다. '증가' 버튼을 위한 `handleIncrement`와 '감소' 버튼을 위한 `handleDecrement` 함수를 작성합니다.

> [!IMPORTANT]
> State를 변경할 때는 반드시 `setCount`와 같은 세터(Setter) 함수를 사용해야 한다는 점을 잊지 마세요!

```jsx
// src/components/Counter.jsx

import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  // '증가' 버튼 클릭 시 실행될 함수
  const handleIncrement = () => {
    setCount(count + 1); // 현재 count에 1을 더한 값으로 state를 업데이트
  };

  // '감소' 버튼 클릭 시 실행될 함수
  const handleDecrement = () => {
    setCount(count - 1); // 현재 count에서 1을 뺀 값으로 state를 업데이트
  };

  return (
    <div>
      <h2>카운터</h2>
      <p>현재 카운트: {count}</p>
      <button>증가</button>
      <button>감소</button>
    </div>
  );
}

export default Counter;
```

### Step 6: 버튼에 이벤트 핸들러 연결하기

이제 `button` 태그의 `onClick` 속성을 사용하여, 버튼 클릭 시 방금 만든 함수들이 실행되도록 연결합니다.

```jsx
// src/components/Counter.jsx

// ... (import, 함수 선언 부분은 이전과 동일)

function Counter() {
  const [count, setCount] = useState(0);

  const handleIncrement = () => {
    setCount(count + 1);
  };

  const handleDecrement = () => {
    setCount(count - 1);
  };

  return (
    <div>
      <h2>카운터</h2>
      <p>현재 카운트: {count}</p>
      {/* onClick 속성에 handleIncrement 함수를 연결 */}
      <button onClick={handleIncrement}>증가</button>
      {/* onClick 속성에 handleDecrement 함수를 연결 */}
      <button onClick={handleDecrement}>감소</button>
    </div>
  );
}

export default Counter;
```

### Step 7: `App.jsx`에서 `Counter` 컴포넌트 사용하기

마지막으로, 우리가 만든 `Counter` 컴포넌트를 메인 페이지인 `App.jsx`에 표시해 봅시다. 기존 `App.jsx`의 내용을 지우고, `Counter` 컴포넌트를 `import`하여 사용합니다.

```jsx
// src/App.jsx

import Counter from './components/Counter'; // 우리가 만든 Counter 컴포넌트를 가져옵니다.

function App() {
  return (
    <div>
      <h1>My React App</h1>
      <Counter /> {/* Counter 컴포넌트를 렌더링합니다. */}
    </div>
  );
}

export default App;
```

### Step 8: 결과 확인

이제 브라우저 화면을 확인해보세요. '카운터' 컴포넌트가 보이고, '증가'와 '감소' 버튼을 클릭할 때마다 숫자가 정상적으로 바뀌는 것을 볼 수 있습니다.

React Developer Tools의 `Components` 탭을 열어 `Counter` 컴포넌트를 선택하고, 버튼을 클릭할 때마다 `State` 값이 어떻게 변하는지 관찰해보세요!

---

## 🚀 도전 과제 (Optional)

실습을 완료했다면, 다음 기능들을 추가하여 스스로의 힘으로 컴포넌트를 확장해보세요.

1.  **초기화 버튼**: 숫자를 0으로 되돌리는 '초기화' 버튼을 추가해보세요.
2.  **음수 방지**: 카운트가 0보다 작아지지 않도록 `handleDecrement` 함수를 수정해보세요. (힌트: `if` 조건문을 사용할 수 있습니다.)

## ✅ 최종 코드

실습을 진행하다 막히는 부분이 있다면 아래의 완성된 코드를 참고하세요.

<details>
<summary><b>src/components/Counter.jsx</b></summary>

```jsx
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  const handleIncrement = () => {
    setCount(count + 1);
  };

  const handleDecrement = () => {
    setCount(count - 1);
  };

  return (
    <div>
      <h2>카운터</h2>
      <p>현재 카운트: {count}</p>
      <button onClick={handleIncrement}>증가</button>
      <button onClick={handleDecrement}>감소</button>
    </div>
  );
}

export default Counter;
```

</details>

<details>
<summary><b>src/App.jsx</b></summary>

```jsx
import Counter from './components/Counter';

function App() {
  return (
    <div>
      <h1>My React App</h1>
      <Counter />
    </div>
  );
}

export default App;
```

</details>

---

- [목차로 돌아가기](README.md)
- [이전 강의로 이동](18-React-DevTools-and-Hot-Reloading.md)
- [다음 강의로 이동](Lab4-Toggle-Component.md)
