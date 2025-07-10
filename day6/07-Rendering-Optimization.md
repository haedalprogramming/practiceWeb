# 7) 렌더링 최적화 (React.memo, useCallback)

이전 시간에 우리는 부모 컴포넌트가 리렌더링되면 자식 컴포넌트도 함께 리렌더링된다는 것을 배웠습니다. 대부분의 경우 이는 문제가 되지 않지만, 애플리케이션이 복잡해지고 컴포넌트 구조가 깊어지면 불필요한 리렌더링이 쌓여 성능 저하의 원인이 될 수 있습니다.

이번 시간에는 이러한 불필요한 렌더링을 방지하여 애플리케이션을 더 빠르고 효율적으로 만드는 **최적화** 기법에 대해 알아보겠습니다.

---

## 1) 최적화는 왜 필요할까?

다음과 같은 상황을 가정해 보겠습니다.

```jsx
import React, { useState, useEffect } from 'react';

// 매초 시간이 흐르는 것을 보여주는 부모 컴포넌트
function TimerApp() {
  const [time, setTime] = useState(new Date());

  useEffect(() => {
    const timerId = setInterval(() => {
      setTime(new Date());
    }, 1000);
    return () => clearInterval(timerId);
  }, []);

  console.log("부모 컴포넌트(TimerApp) 렌더링...");

  return (
    <div>
      <h1>현재 시간: {time.toLocaleTimeString()}</h1>
      <Greeting message="안녕하세요!" />
    </div>
  );
}

// 간단한 인사말을 보여주는 자식 컴포넌트
function Greeting({ message }) {
  console.log("자식 컴포넌트(Greeting) 렌더링...");
  return <p>{message}</p>;
}
```

위 코드에서 `TimerApp` 컴포넌트는 1초마다 `time` 상태를 갱신하며 리렌더링됩니다. 이때 자식 컴포넌트인 `Greeting`은 어떤 일이 일어날까요? `Greeting`에게 전달되는 `message` prop은 "안녕하세요!"로 전혀 변하지 않았음에도 불구하고, 부모인 `TimerApp`이 리렌더링될 때마다 함께 리렌더링됩니다.

개발자 도구의 콘솔을 열어보면 1초마다 부모와 자식의 렌더링 로그가 함께 찍히는 것을 볼 수 있습니다.

> [!NOTE]
> 지금은 `Greeting` 컴포넌트가 매우 단순해서 성능에 영향이 없지만, 만약 이 컴포넌트가 복잡한 계산을 하거나 무거운 그래픽을 포함하고 있다면 불필요한 리렌더링은 앱 전체를 느리게 만들 수 있습니다.

이런 문제를 해결하기 위해 React는 `React.memo`라는 도구를 제공합니다.

---

## 2) React.memo를 이용한 컴포넌트 최적화

`React.memo`는 **컴포넌트의 props가 변경되지 않았다면, 리렌더링을 건너뛰도록 만드는 기능**을 합니다. 즉, 컴포넌트의 렌더링 결과를 '기억(memoization)' 해두고, 다음 렌더링 시 props가 동일하다면 기억해 둔 결과를 재사용하는 것입니다.

`React.memo`는 <strong>고차 컴포넌트(Higher-Order Component, HOC)</strong>입니다.

> [!TIP]
> **고차 컴포넌트(HOC)란?**
> HOC는 컴포넌트를 인자로 받아서, 새로운 기능이 추가된 컴포넌트를 반환하는 '함수'입니다. `React.memo`는 우리가 만든 컴포넌트를 인자로 받아, 렌더링 최적화 기능이 추가된 새로운 컴포넌트를 만들어주는 HOC입니다.

### 사용법

사용법은 매우 간단합니다. 최적화하고 싶은 자식 컴포넌트를 `React.memo()`로 감싸주기만 하면 됩니다.

```jsx
// Greeting 컴포넌트를 React.memo로 감싸서 내보내기
const MemoizedGreeting = React.memo(Greeting);

// 부모 컴포넌트에서는 이 메모이즈된 컴포넌트를 사용
function TimerApp() {
  // ... (이전 코드와 동일)
  return (
    <div>
      <h1>현재 시간: {time.toLocaleTimeString()}</h1>
      <MemoizedGreeting message="안녕하세요!" />
    </div>
  );
}
```

이제 다시 실행해보면, `TimerApp`은 여전히 1초마다 리렌더링되지만 `MemoizedGreeting` 컴포넌트의 렌더링 로그는 처음 한 번만 찍히고 더 이상 나타나지 않는 것을 확인할 수 있습니다. `message` prop이 변하지 않았기 때문이죠!

---

## 3) useCallback을 이용한 함수 최적화

`React.memo`를 사용하면 대부분의 불필요한 렌더링을 막을 수 있지만, 한 가지 함정이 있습니다. **prop으로 함수를 전달하는 경우**입니다.

```jsx
import React, { useState } from 'react';

const MemoizedButton = React.memo(function Button({ onClick }) {
  console.log("버튼 컴포넌트 렌더링...");
  return <button onClick={onClick}>카운트 올리기</button>;
});

function CounterApp() {
  const [count, setCount] = useState(0);
  console.log("부모 컴포넌트(CounterApp) 렌더링...");

  // 버튼 클릭 시 count를 1 증가시키는 함수
  const increment = () => {
    setCount(count + 1);
  };

  return (
    <div>
      <p>Count: {count}</p>
      <MemoizedButton onClick={increment} />
    </div>
  );
}
```

위 코드에서 `Button` 컴포넌트는 `React.memo`로 감싸여 있습니다. 하지만 `CounterApp`의 `setCount`가 호출되어 리렌더링될 때마다 `Button` 컴포넌트도 계속해서 리렌더링됩니다. 왜 그럴까요?

문제는 `increment` 함수에 있습니다. JavaScript에서 **함수는 객체**입니다. 컴포넌트가 리렌더링될 때마다 `CounterApp` 내부에 있는 `increment` 함수는 **매번 새로 생성**됩니다. 코드는 똑같아 보이지만, 메모리상에는 이전 렌더링의 함수와는 다른 새로운 함수 객체가 만들어지는 것입니다.

따라서 `React.memo`는 `onClick` prop으로 전달된 `increment` 함수가 이전과 다른 '새로운' 함수라고 판단하여 `Button` 컴포넌트를 다시 렌더링하게 됩니다.

이 문제를 해결하기 위해 `useCallback` Hook을 사용합니다.

`useCallback`은 <strong>함수를 메모이제이션(기억)</strong>하기 위해 사용되는 Hook입니다. `useCallback`으로 감싸진 함수는, 의존성 배열(dependency array)에 포함된 값이 변경되지 않는 한 이전에 생성된 함수를 재사용합니다.

### 사용법

```jsx
import React, { useState, useCallback } from 'react';

// ... (MemoizedButton 컴포넌트는 동일)

function CounterApp() {
  const [count, setCount] = useState(0);
  console.log("부모 컴포넌트(CounterApp) 렌더링...");

  // useCallback을 사용하여 함수를 메모이제이션
  const increment = useCallback(() => {
    setCount(c => c + 1);
  }, []); // 의존성 배열이 비어있으므로, 이 함수는 컴포넌트가 처음 생성될 때 단 한 번만 만들어집니다.

  return (
    <div>
      <p>Count: {count}</p>
      <MemoizedButton onClick={increment} />
    </div>
  );
}
```

`increment` 함수를 `useCallback`으로 감싸고 의존성 배열을 빈 배열(`[]`)로 설정하면, 이 함수는 컴포넌트가 처음 마운트될 때 한 번만 생성되고 이후 리렌더링에서는 재생성되지 않습니다. 따라서 `MemoizedButton` 컴포넌트는 더 이상 불필요하게 리렌더링되지 않습니다.

> [!NOTE]
> 만약 `useCallback`으로 감싼 함수 내부에서 `state`나 `props` 값을 사용한다면, 반드시 의존성 배열에 해당 값을 추가해야 합니다. 그렇지 않으면 함수는 항상 초기값만 기억하게 되는 문제가 발생합니다.
> `setCount(count + 1)` 대신 `setCount(c => c + 1)` 과 같은 함수형 업데이트를 사용하면 `count`를 의존성 배열에 넣지 않아도 안전하게 최신 상태 값을 참조할 수 있습니다.

---

## 4) 언제 최적화를 해야 할까? (매우 중요!)

`React.memo`와 `useCallback`은 분명 유용한 도구지만, 무분별하게 사용하는 것은 좋지 않습니다.

> [!WARNING]
> **성급한 최적화는 모든 악의 근원이다. (Premature optimization is the root of all evil.)**
> 
> \- 도널드 크누스 (컴퓨터 과학자)

최적화에는 비용이 따릅니다.
-   `React.memo`는 props를 비교하는 데 계산 비용이 듭니다.
-   `useCallback`은 함수를 메모리에 저장하는 데 비용이 듭니다.

대부분의 경우 React는 충분히 빠르기 때문에 굳이 최적화를 하지 않아도 괜찮습니다. 잘못된 최적화는 오히려 코드를 복잡하게 만들고 성능을 악화시킬 수도 있습니다.

**최적화는 반드시 필요한 경우에만 해야 합니다.**

1.  **프로파일링 도구로 성능을 측정**: React 개발자 도구의 Profiler와 같은 도구를 사용하여 실제로 어떤 컴포넌트에서 렌더링 병목 현상이 발생하는지 먼저 확인해야 합니다.
2.  **병목 지점 최적화**: 측정을 통해 확인된, 실제로 성능에 문제를 일으키는 컴포넌트에 한해서 `React.memo`나 `useCallback` 등을 적용하는 것이 좋습니다.

---

## 5) 정리

-   `React.memo`: 컴포넌트의 props가 변하지 않으면 리렌더링을 방지하는 HOC입니다.
-   `useCallback`: 함수를 메모이제이션하여, 자식 컴포넌트에 prop으로 함수를 넘길 때 불필요한 리렌더링이 발생하는 것을 막아줍니다.
-   **최적화는 공짜가 아닙니다.** 반드시 성능 측정을 통해 병목이 확인된 곳에만 신중하게 적용해야 합니다.

이제 여러분은 React 애플리케이션의 성능을 한 단계 끌어올릴 수 있는 강력한 도구들을 갖추게 되었습니다!

---

- [목차로 돌아가기](./README.md)
- [이전 강의로 이동](./06-Understanding-Rendering.md)
- [다음 강의로 이동](./08-React-DevTools-Advanced.md)
