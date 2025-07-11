# 03. useRef Hook 이란?

React에서 컴포넌트의 상태를 관리할 때는 주로 `useState`를 사용합니다. `useState`로 관리하는 값은 변경될 때마다 컴포넌트를 다시 그리게(리렌더링) 하여 화면을 업데이트합니다.

하지만 때로는 값이 변경되어도 **컴포넌트를 다시 그릴 필요가 없으면서**, 컴포넌트가 다시 그려져도 **그 값을 잃어버리지 않고 계속 기억해야 하는** 경우가 있습니다. 이럴 때 사용하는 것이 바로 **`useRef` Hook**입니다.

`useRef`는 마치 컴포넌트가 다시 그려져도 변하지 않고 계속 기억해야 하는 '상자'를 만든다고 생각하면 됩니다. 이 상자 안의 내용물은 `current`라는 이름으로 접근할 수 있습니다.

## 3-1) useRef의 기본 사용법

`useRef`는 다음과 같이 사용합니다.

```jsx
import { useRef } from 'react';

function MyComponent() {
  const myRef = useRef(초기값);
  // myRef는 { current: 초기값 } 형태의 객체를 반환합니다.
  // 실제 값은 myRef.current에 저장됩니다.

  // ...
}
```

`useRef`가 반환하는 객체는 컴포넌트의 생명주기 동안 계속 유지됩니다. 즉, 컴포넌트가 여러 번 렌더링되어도 `myRef` 객체는 항상 동일한 것을 가리킵니다.

> [!IMPORTANT]
> **`useRef`의 값이 변경되어도 컴포넌트는 리렌더링되지 않습니다.**
> 이것이 `useState`와 `useRef`의 가장 큰 차이점입니다. `useRef`는 값을 저장하지만, 그 값이 변경되어도 화면을 업데이트하지 않습니다. 화면 업데이트가 필요 없는 값을 저장할 때 유용합니다.

## 3-2) useRef의 주요 사용 사례

### 1. DOM 요소 직접 접근

React는 선언적으로 UI를 다루는 것을 권장하지만, 가끔은 특정 DOM 요소에 직접 접근해야 할 때가 있습니다. 예를 들어, `<input>` 필드에 자동으로 포커스를 주거나, `<video>`를 재생/정지하는 등의 작업입니다.

`useRef`를 사용하면 React가 관리하는 DOM 요소에 직접 접근할 수 있습니다.

```jsx
import { useRef } from 'react';

function FocusInput() {
  const inputRef = useRef(null); // 초기값은 보통 null로 설정합니다.

  const handleFocus = () => {
    // inputRef.current는 <input> DOM 요소를 가리킵니다.
    if (inputRef.current) {
      inputRef.current.focus(); // input 요소에 포커스를 줍니다.
    }
  };

  return (
    <div>
      <input type="text" ref={inputRef} placeholder="여기에 포커스" />
      <button onClick={handleFocus}>Input에 포커스 주기</button>
    </div>
  );
}
```

> [!NOTE]
> `inputRef.current`는 컴포넌트가 처음 렌더링될 때는 `null`입니다. React가 DOM 요소를 화면에 그린 후에야 `inputRef.current`에 실제 DOM 요소가 할당됩니다.

### 2. 렌더링과 무관하게 값을 저장 (Mutable Value)

컴포넌트가 리렌더링되어도 초기화되지 않고, 계속 유지되어야 하는 값을 저장할 때 `useRef`를 사용합니다. 이 값의 변경이 리렌더링을 유발해서는 안 될 때 특히 유용합니다.

**예제: 컴포넌트 렌더링 횟수 세기**

```jsx
import { useRef, useState } from 'react';

function RenderCounter() {
  const renderCount = useRef(0);
  const [_, setForceRender] = useState(0); // 리렌더링을 강제로 유발하기 위한 상태

  // 컴포넌트가 렌더링될 때마다 renderCount.current 값을 1 증가시킵니다.
  // 이 값의 변경은 리렌더링을 유발하지 않습니다.
  renderCount.current = renderCount.current + 1;

  return (
    <div>
      <p>이 컴포넌트는 {renderCount.current}번 렌더링되었습니다.</p>
      <button onClick={() => setForceRender(prev => prev + 1)}>리렌더링 강제하기</button>
    </div>
  );
}
```

**예제: `setInterval` ID 저장**

`useEffect` 강의에서 `setInterval`을 사용하고 `clearInterval`로 정리하는 것을 배웠습니다. 이때 `setInterval`이 반환하는 ID를 `useRef`에 저장하면, 컴포넌트가 리렌더링되어도 ID 값을 잃어버리지 않고 클린업 함수에서 사용할 수 있습니다.

```jsx
import { useRef, useState, useEffect } from 'react';

function TimerWithRef() {
  const intervalRef = useRef(null); // setInterval ID를 저장할 useRef
  const [count, setCount] = useState(0);

  useEffect(() => {
    // intervalRef.current에 setInterval ID를 저장합니다.
    intervalRef.current = setInterval(() => {
      setCount(c => c + 1);
    }, 1000);

    // 클린업 함수에서 저장된 ID를 사용하여 타이머를 정리합니다.
    return () => {
      if (intervalRef.current) {
        clearInterval(intervalRef.current);
      }
    };
  }, []); // 컴포넌트 마운트 시 한 번만 실행

  return <p>Timer: {count}</p>;
}
```

### 3. 이전 값 저장하기 (Storing Previous Value)

`useRef`는 컴포넌트의 `props`나 `state`의 **이전 값(previous value)**을 저장하고 싶을 때 매우 유용하게 사용됩니다. `useEffect`와 함께 사용하면, 현재 렌더링된 값과 이전 렌더링된 값을 비교하는 로직을 쉽게 구현할 수 있습니다.

```jsx
import { useRef, useEffect } from 'react';

function PreviousValueDisplay({ count }) {
  const prevCountRef = useRef(); // 이전 count 값을 저장할 ref

  useEffect(() => {
    // count 값이 변경될 때마다 prevCountRef.current에 현재 count 값을 저장
    // 이 effect는 count가 변경될 때마다 실행됩니다.
    prevCountRef.current = count;
  }, [count]); // count가 변경될 때만 이 effect 실행

  // prevCount는 이전 렌더링 시점의 count 값을 가리킵니다.
  const prevCount = prevCountRef.current;

  return (
    <div>
      <p>현재 값: {count}</p>
      <p>이전 값: {prevCount === undefined ? '없음' : prevCount}</p>
      {count > (prevCount || 0) && prevCount !== undefined && (
        <p style={{ color: 'green' }}>값이 증가했습니다!</p>
      )}
      {count < (prevCount || 0) && prevCount !== undefined && (
        <p style={{ color: 'red' }}>값이 감소했습니다!</p>
      )}
    </div>
  );
}
```

이 패턴은 특정 값이 변경될 때마다 이전 값과 현재 값을 비교하여 다른 동작을 수행해야 할 때 매우 유용합니다.

## 3-3) useRef vs useState

두 Hook 모두 컴포넌트 내에서 값을 저장하고 관리하는 데 사용되지만, 목적과 동작 방식에 큰 차이가 있습니다.

| 특징       | `useState`                               | `useRef`                                   |
| :--------- | :--------------------------------------- | :----------------------------------------- |
| **목적**   | 상태 관리 (UI에 반영)                    | DOM 요소 접근, 렌더링과 무관한 값 저장     |
| **값 변경**| `set` 함수 사용                          | `.current` 속성 직접 변경                  |
| **리렌더링**| 값 변경 시 **리렌더링 발생**             | 값 변경 시 **리렌더링 발생 안 함**         |
| **초기값** | 첫 렌더링 시 한 번만 설정                | 첫 렌더링 시 한 번만 설정                  |

> [!IMPORTANT]
> `useRef`는 값이 변경되어도 리렌더링을 유발하지 않으므로, **화면에 즉시 반영되어야 하는 값**을 저장하는 데 사용해서는 안 됩니다. 화면에 보여지는 값은 반드시 `useState`로 관리해야 합니다.

## 3-4) 정리

*   `useRef`는 `.current` 속성을 통해 값을 저장하고 접근하는 특별한 객체를 반환하는 Hook입니다.
*   주요 용도는 **DOM 요소에 직접 접근**하거나, **렌더링과 무관하게 컴포넌트 생명주기 동안 유지되어야 하는 값**을 저장하는 것입니다.
*   `useRef`의 값이 변경되어도 컴포넌트는 **리렌더링되지 않습니다.** 이 점이 `useState`와의 가장 큰 차이점입니다.
*   화면에 보여지는 상태는 `useState`로, 렌더링과 무관하게 유지되어야 하는 값은 `useRef`로 관리하는 것이 좋습니다.

---

- [목차로 돌아가기](./README.md)
- [이전 강의로 이동](./02-useEffect-Deep-Dive.md)
- [다음 강의로 이동](./04-useContext-Hook.md)
