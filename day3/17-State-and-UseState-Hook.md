# 05. State와 useState Hook

지난 시간에는 Props를 통해 부모 컴포넌트가 자식 컴포넌트에게 데이터를 전달하는 방법을 배웠습니다. Props는 컴포넌트를 동적으로 만들지만, 데이터는 부모로부터 일방적으로 전달될 뿐, 컴포넌트 스스로가 데이터를 변경할 수는 없었습니다. 이번 시간에는 컴포넌트가 **스스로 데이터를 관리**하고, 사용자의 행동(클릭, 입력 등)에 따라 UI를 업데이트할 수 있게 해주는 **State**에 대해 배우겠습니다.

## 1. State란 무엇인가?

**State**는 React 컴포넌트의 <strong>'상태'</strong>를 의미합니다. 간단히 말해, **컴포넌트 내부에서 관리되며 시간이 지남에 따라 변할 수 있는 데이터**입니다. 예를 들어, 사용자가 버튼을 클릭한 횟수, 토글 스위치의 ON/OFF 상태, 입력창에 입력된 텍스트 등이 모두 State가 될 수 있습니다.

> [!IMPORTANT]
> **Props vs. State**
> 
> Props와 State는 React에서 데이터를 다루는 두 가지 핵심 개념이며, 그 차이를 이해하는 것이 매우 중요합니다.
> 
> *   **Props**: 부모 컴포넌트로부터 전달받는 데이터입니다. 자식 컴포넌트 내부에서 **수정할 수 없습니다 (읽기 전용)**.
> *   **State**: 컴포넌트 내부에서 자체적으로 생성하고 관리하는 데이터입니다. 컴포넌트의 이벤트나 사용자의 행동에 따라 **수정할 수 있습니다**.

State가 변경되면, React는 이를 감지하고 <strong>컴포넌트를 자동으로 다시 렌더링(Re-rendering)</strong>하여 변경된 내용을 화면에 반영합니다. 이것이 바로 React가 동적인 사용자 인터페이스를 만드는 핵심 원리입니다.

## 2. `useState` 훅

함수형 컴포넌트에서 State를 사용하려면 **훅**이라는 특별한 함수를 사용해야 합니다. State를 관리하기 위한 훅이 바로 **`useState`** 입니다.

> [!NOTE]
> **훅이란?**
> 
> 훅은 함수형 컴포넌트에서 React의 기능(State 관리, 생명주기 등)을 "걸어서(훅 onto)" 사용할 수 있게 해주는 함수들입니다. 훅 함수의 이름은 항상 `use`로 시작합니다. (예: `useState`, `useEffect`) 이후에 자세히 다루겠습니다.

### `useState` 사용법

`useState`를 사용하려면 먼저 React로부터 `import` 해야 합니다.

```jsx
import { useState } from 'react';
```

그리고 컴포넌트 내부에서 다음과 같이 호출합니다.

```jsx
function MyComponent() {
  const [state, setState] = useState(initialState);
  // ...
}
```

*   `useState(initialState)`: `useState` 함수를 호출할 때 괄호 안에 **State의 초기값**을 넣어줍니다.
*   `[state, setState]`: `useState`는 **배열**을 반환합니다. 이 배열에는 두 개의 요소가 들어있습니다.
    1.  `state`: 현재 State 값 (우리가 정한 초기값으로 시작)
    2.  `setState`: 이 State 값을 변경할 때 사용하는 **세터(Setter) 함수**

> [!TIP]
> `const [state, setState]` 와 같은 문법은 JavaScript의 **배열 구조 분해 할당**입니다. `useState`가 반환한 배열의 첫 번째 요소를 `state` 변수에, 두 번째 요소를 `setState` 변수에 할당하는 것입니다. 변수 이름은 원하는 대로 지을 수 있지만, `[변수명, set변수명]` 형태의 관례를 따르는 것이 좋습니다. (예: `[count, setCount]`, `[name, setName]`)

## 3. State를 이용한 카운터 예제

State가 어떻게 동작하는지 알아보기 위해, 버튼을 누르면 숫자가 1씩 증가하는 간단한 카운터 컴포넌트를 만들어 보겠습니다.

```jsx
// src/components/Counter.jsx

import { useState } from 'react';

function Counter() {
  // `count`라는 이름의 새로운 state 변수를 선언하고, 초기값을 0으로 설정합니다.
  const [count, setCount] = useState(0);

  // 버튼 클릭 시 호출될 함수
  const handleIncrement = () => {
    // 💥 잘못된 방법: state를 직접 수정하면 안 됩니다!
    // count = count + 1;

    // ✅ 올바른 방법: 반드시 세터 함수를 사용해야 합니다.
    setCount(count + 1);
    console.log('현재 카운트:', count + 1);
  };

  return (
    <div>
      {/* 현재 count 값을 화면에 표시 */}
      <p>현재 카운트: {count}</p>
      
      {/* 버튼 클릭 시 handleIncrement 함수 실행 */}
      <button onClick={handleIncrement}>증가</button>
    </div>
  );
}

export default Counter;
```

### 동작 원리

1.  **최초 렌더링**: `Counter` 컴포넌트가 처음 렌더링될 때, `useState(0)`에 의해 `count` 변수는 `0`이 됩니다. 화면에는 "현재 카운트: 0"이 표시됩니다.
2.  **이벤트 발생**: 사용자가 '증가' 버튼을 클릭하면 `onClick` 이벤트에 연결된 `handleIncrement` 함수가 호출됩니다.
3.  **State 업데이트**: `handleIncrement` 함수 내부의 `setCount(count + 1)`가 실행됩니다. React는 `count` State를 `1`로 업데이트하라고 예약합니다.
4.  **리렌더링(Re-rendering)**: React는 State가 변경된 것을 감지하고, `Counter` 컴포넌트를 **다시 호출(리렌더링)**합니다.
5.  **화면 업데이트**: 다시 호출된 `Counter` 컴포넌트에서 `count` 변수는 이제 `1`입니다. React는 이 변경된 값을 화면에 반영하여 "현재 카운트: 1"을 표시합니다.

> [!WARNING]
> **State는 직접 수정하면 안 됩니다!**
> 
> `count = count + 1` 처럼 State 변수를 직접 수정하면, React는 State가 변경되었는지 알 수 없어 리렌더링을 하지 않습니다. 따라서 화면의 내용도 바뀌지 않습니다. State를 변경할 때는 **반드시 `useState`가 제공하는 세터 함수(`setCount`)를 사용해야 합니다.**

이제 우리는 컴포넌트가 스스로 상태를 가지고, 그 상태 변화에 따라 화면이 업데이트되는 방법을 배웠습니다. 이것이 바로 React를 이용한 인터랙티브한 웹 개발의 핵심입니다.

다음 시간에는 개발 편의성을 높여주는 React 개발 도구에 대해 알아보겠습니다.

---

- [목차로 돌아가기](README.md)
- [이전 강의로 이동](16-Props.md)
- [다음 강의로 이동](18-React-DevTools-and-Hot-Reloading.md)
