# 1) 커스텀 Hook (Custom Hooks)

이전 강의에서 우리는 `useState`, `useEffect`와 같은 React Hook들을 사용해 컴포넌트의 상태를 관리하고, 부수 효과(Side Effect)를 처리하는 방법을 배웠습니다.

하지만 여러 컴포넌트에서 동일한 로직(예: 데이터를 가져오고, 로딩 상태를 관리하는 로직)을 반복해서 작성해야 하는 경우가 생기면 어떻게 해야 할까요? 코드는 길어지고, 유지보수는 어려워질 것입니다.

이러한 **'반복되는 상태 관련 로직'을 재사용**하기 위해 사용하는 것이 바로 **커스텀 Hook(Custom Hook)** 입니다.

## 1-1) 커스텀 Hook이란?

**커스텀 Hook**은 이름 그대로 우리가 직접 만드는 '특별한' 함수입니다. 이 함수의 역할은 컴포넌트 내부의 상태 관련 로직(Stateful Logic)을 함수로 추출하여, 여러 컴포넌트에서 재사용할 수 있도록 하는 것입니다.

> [!IMPORTANT]
> **상태 관련 로직(Stateful Logic)이란?**
> `useState`, `useEffect`처럼 컴포넌트의 '상태(State)'와 직접적으로 관련된 로직을 의미합니다. 예를 들어, 상태를 만들고, 그 상태를 특정 조건에 따라 변경하거나, 상태가 변할 때 특정 작업을 수행하는 코드 전체를 말합니다.

커스텀 Hook을 만들 때는 두 가지 중요한 규칙이 있습니다.

1.  **이름이 반드시 `use`로 시작해야 합니다.**
    *   예: `useCounter`, `useFetch`, `useWindowWidth`
    *   React는 이 `use`라는 접두사를 보고 "아, 이것은 Hook이구나!"라고 인식하여, Hook의 규칙([지난 강의 보기](https://react.dev/warnings/invalid-hook-call-warning.html))이 잘 지켜지고 있는지 자동으로 검사해 줍니다.

2.  **내부에서 다른 Hook(`useState`, `useEffect` 등)을 호출할 수 있습니다.**
    *   이것이 커스텀 Hook이 일반 함수와 다른 가장 큰 특징입니다. 상태 관련 로직을 다루기 위해 다른 Hook의 기능을 빌려올 수 있습니다.

## 1-2) 커스텀 Hook vs 일반 함수/컴포넌트

커스텀 Hook을 처음 배우면 기존의 개념과 헷갈릴 수 있습니다. 차이점을 명확히 짚어보겠습니다.

*   **Q: 그냥 일반 함수로 만들면 안 되나요?**
    *   **A:** 안됩니다. 가장 큰 차이점은 **상태를 가질 수 있는가**의 여부입니다. 일반 JavaScript 함수는 `useState`나 `useEffect` 같은 React Hook을 호출할 수 없습니다. 오직 React 컴포넌트와 커스텀 Hook 안에서만 Hook을 호출할 수 있습니다.

*   **Q: 컴포넌트와는 무엇이 다른가요?**
    *   **A:** 반환하는 것이 다릅니다. 컴포넌트는 화면에 보일 <strong>UI(JSX)</strong>를 반환하지만, 커스텀 Hook은 상태 값, 함수 등 <strong>로직(Logic)에 필요한 값들</strong>을 배열이나 객체 형태로 반환합니다. 즉, UI를 재사용하고 싶으면 컴포넌트를, 로직을 재사용하고 싶으면 커스텀 Hook을 사용합니다.

## 1-3) 커스텀 Hook 만들기

커스텀 Hook을 만드는 과정은 간단합니다. 컴포넌트 안에 있던 로직을 잘라내어 `use`로 시작하는 함수에 붙여넣는 것과 같습니다.

예를 들어, 두 개의 다른 컴포넌트에서 각각 카운터 기능을 구현해야 한다고 상상해 봅시다.

**커스텀 Hook 사용 전 (로직 중복 발생)**

```jsx
// ComponentA.js
function ComponentA() {
  const [count, setCount] = useState(0);

  const increment = () => setCount(count + 1);
  const decrement = () => setCount(count - 1);

  return (
    <div>
      <p>Component A Counter: {count}</p>
      <button onClick={increment}>증가</button>
      <button onClick={decrement}>감소</button>
    </div>
  );
}

// ComponentB.js
function ComponentB() {
  const [count, setCount] = useState(0);

  const increment = () => setCount(count + 1);
  const decrement = () => setCount(count - 1);

  return (
    <div>
      <h2>Component B Counter: {count}</h2>
      <button onClick={increment}>증가</button>
      <button onClick={decrement}>감소</button>
    </div>
  );
}
```

두 컴포넌트의 카운터 관련 로직이 완전히 똑같습니다. 이제 이 중복되는 로직을 `useCounter`라는 커스텀 Hook으로 추출해 보겠습니다.

**1. `useCounter.js` 파일 생성**

`use`로 시작하는 함수를 만들고, 중복되던 로직을 그대로 옮겨옵니다. 컴포넌트에서 필요한 값들(`count`, `increment`, `decrement`)을 객체나 배열 형태로 반환해 줍니다.

```jsx
// useCounter.js
import { useState } from 'react';

export function useCounter(initialValue = 0) {
  const [count, setCount] = useState(initialValue);

  const increment = () => setCount(count + 1);
  const decrement = () => setCount(count - 1);

  // 컴포넌트에서 사용할 값과 함수들을 반환
  return { count, increment, decrement };
}
```

**2. 기존 컴포넌트 수정**

이제 컴포넌트에서는 직접 만들었던 로직 대신, `useCounter` Hook을 호출하여 필요한 값과 함수를 받아 사용하기만 하면 됩니다.

**커스텀 Hook 사용 후 (간결해진 코드)**

```jsx
import { useCounter } from './useCounter'; // 만들어 둔 커스텀 Hook 가져오기

function ComponentA() {
  // 커스텀 Hook 호출
  const { count, increment, decrement } = useCounter(0);

  return (
    <div>
      <p>Component A Counter: {count}</p>
      <button onClick={increment}>증가</button>
      <button onClick={decrement}>감소</button>
    </div>
  );
}

function ComponentB() {
  // 초기값을 다르게 줄 수도 있음
  const { count, increment, decrement } = useCounter(10);

  return (
    <div>
      <h2>Component B Counter: {count}</h2>
      <button onClick={increment}>증가</button>
      <button onClick={decrement}>감소</button>
    </div>
  );
}
```

> [!NOTE]
> `ComponentA`와 `ComponentB`는 각자 `useCounter`를 호출했습니다. 따라서 두 컴포넌트의 `count` 상태는 서로에게 전혀 영향을 주지 않는 **독립적인 상태**가 됩니다. 커스텀 Hook은 로직을 공유하는 것이지, 상태(state) 그 자체를 공유하는 것이 아닙니다.

## 1-4) 언제 커스텀 Hook을 만들어야 할까?

> [!TIP]
> 코드를 작성하다 다음과 같은 신호가 보이면, 커스텀 Hook으로 분리하는 것을 적극적으로 고려해 보세요.
> *   **로직이 중복될 때**: 여러 컴포넌트에서 `useState`, `useEffect` 등을 사용한 로직이 복사-붙여넣기 되고 있다면, 즉시 커스텀 Hook으로 만들어야 합니다.
> *   **컴포넌트가 너무 복잡할 때**: 하나의 컴포넌트 안에 UI와 관련 없는 상태 관리 로직이 너무 길어져서 코드를 읽기 어렵다면, 로직 부분을 커스텀 Hook으로 분리하여 컴포넌트를 깔끔하게 유지할 수 있습니다.
> *   **기능별로 로직을 묶고 싶을 때**: 폼 입력 처리, API 요청, 브라우저 창 크기 감지 등 특정 기능과 관련된 여러 상태와 효과들을 하나의 Hook으로 묶어 관리하면 코드가 훨씬 체계적으로 변합니다.

## 1-5) 커스텀 Hook으로 무엇을 할 수 있을까?

`useCounter`는 간단한 예시일 뿐, 커스텀 Hook의 가능성은 무궁무진합니다. 예를 들면 아래와 같은 Hook들을 만들 수 있습니다.

*   `useFetch(url)`: API 요청을 보내고, `data`, `loading`, `error` 상태를 반환하는 Hook
*   `useWindowSize()`: 브라우저 창의 너비와 높이를 실시간으로 알려주는 Hook
*   `useLocalStorage(key, initialValue)`: 브라우저의 로컬 스토리지와 React 상태를 자동으로 동기화하는 Hook
*   `useFormInput(initialValue)`: 폼 `input` 태그의 `value`와 `onChange` 핸들러를 묶어서 관리하는 Hook

이처럼 커스텀 Hook은 복잡한 로직을 재사용 가능한 부품으로 만드는 강력한 도구입니다.

## 1-6) 정리

*   **커스텀 Hook**은 컴포넌트의 **상태 관련 로직**을 재사용하기 위한 `use`로 시작하는 JavaScript 함수입니다.
*   일반 함수와 달리 **`useState` 같은 다른 Hook을 호출**할 수 있으며, 컴포넌트와 달리 **UI(JSX)가 아닌 로직(값, 함수)을 반환**합니다.
*   로직 중복을 제거하고, 복잡한 컴포넌트를 단순하게 만들며, 코드를 기능별로 체계화하는 데 큰 도움을 줍니다.
*   여러 컴포넌트에서 같은 커스텀 Hook을 사용하더라도, 그 안의 **상태는 서로 독립적으로 유지**됩니다.

---

- [목차로 돌아가기](./README.md)
- [다음 강의로 이동](./02-useEffect-Deep-Dive.md)
