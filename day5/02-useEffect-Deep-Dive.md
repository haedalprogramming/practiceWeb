# 2) useEffect Hook 심화

`useEffect`는 React 컴포넌트를 외부 시스템과 동기화하기 위해 사용하는 Hook입니다. 여기서 '외부 시스템'이란 API 서버, 브라우저의 `document`나 `window` 객체, `setInterval` 같은 타이머, 혹은 외부 라이브러리 등을 의미합니다.

지금까지 우리는 `useEffect`를 컴포넌트가 처음 렌더링될 때 데이터를 가져오는 등의 용도로 사용해 왔습니다. 이번 시간에는 `useEffect`의 두 번째 인자인 <strong>의존성 배열(Dependency Array)</strong>과, `useEffect`가 반환하는 <strong>클린업(Clean-up) 함수</strong>에 대해 깊이 있게 알아보며 `useEffect`를 완벽하게 마스터해 보겠습니다.

## 2-1) 의존성 배열(Dependency Array) 다시 보기

`useEffect(setup, dependencies)`에서 두 번째 인자인 `dependencies`는 `setup` 함수(effect)가 언제 다시 실행될지를 결정하는 배열입니다. 이 배열을 어떻게 설정하느냐에 따라 Effect의 동작 방식이 크게 달라집니다.

### 1) `[]` (빈 배열)

의존성 배열을 빈 배열로 전달하면, `setup` 함수는 **컴포넌트가 처음 화면에 렌더링될 때 단 한 번만 실행**됩니다. 그 이후로는 컴포넌트가 리렌더링되어도 다시 실행되지 않습니다.

*   **주요 사용처**: 최초 데이터 로딩, 한 번만 실행해야 하는 초기화 작업

```jsx
useEffect(() => {
  // 이 코드는 처음에 단 한 번만 실행됩니다.
  console.log("컴포넌트가 화면에 처음 나타났습니다.");
}, []);
```

### 2) `[dep1, dep2, ...]` (의존성 지정)

배열 안에 특정 값(state, props 등)을 넣으면, `setup` 함수는 처음 렌더링될 때 한 번 실행되고, 그 이후에는 **배열 안의 값이 변경될 때마다 다시 실행**됩니다.

*   **주요 사용처**: 특정 `props`가 바뀔 때마다 데이터를 새로 가져오기, 특정 `state`가 변할 때마다 특정 효과를 재실행하기

```jsx
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);

  useEffect(() => {
    // userId가 바뀔 때마다 새로운 사용자 정보를 가져옵니다.
    fetch(`https://api.example.com/users/${userId}`)
      .then(res => res.json())
      .then(data => setUser(data));
  }, [userId]); // userId가 바뀔 때만 이 effect를 다시 실행합니다.

  // ...
}
```

### 3) 배열 생략

의존성 배열을 아예 전달하지 않으면, `setup` 함수는 **컴포넌트가 렌더링될 때마다 매번 실행**됩니다.

*   **주요 사용처**: 거의 사용하지 않으며, 종종 버그의 원인이 됩니다.

> [!CAUTION]
> **무한 루프의 위험!**
> 의존성 배열을 생략한 `useEffect` 안에서 상태를 변경하는 코드를 작성하면, `상태 변경 → 리렌더링 → useEffect 실행 → 상태 변경 → ...`의 무한 루프에 빠지게 됩니다. 매우 흔한 실수이며, 주의해야 합니다.

## 2-2) 클린업(Clean-up) 함수

`useEffect`를 사용해 `setInterval`이나 외부 라이브러리의 구독(subscription) 기능 등을 설정했다면, 컴포넌트가 사라질 때 이 설정들을 해제해 주어야 메모리 누수(memory leak)를 막을 수 있습니다. 이때 사용하는 것이 **클린업 함수**입니다.

`useEffect`의 `setup` 함수에서 **새로운 함수를 반환**하면, 이 함수가 바로 클린업 함수가 됩니다.

```jsx
useEffect(() => {
  // (1) Setup: 실행하고자 하는 작업
  console.log('Effect가 실행되었습니다.');

  // (2) Cleanup: 반환되는 함수
  return () => {
    console.log('Effect가 정리(clean-up)됩니다.');
  };
}, [dependencies]);
```

**클린업 함수는 언제 실행될까요?**

1.  **컴포넌트가 화면에서 사라질 때 (Unmount)**
2.  **Effect가 다시 실행되기 직전** (의존성 배열의 값이 변경되어 새로운 `setup` 함수가 실행되기 전에, 이전 `setup`이 남긴 것을 정리하기 위해)

### 클린업 함수 예제: `setInterval`

1초마다 카운트를 1씩 올리는 타이머를 만든다고 생각해 봅시다. 컴포넌트가 사라져도 `setInterval`은 계속 동작하므로, 반드시 정리해주어야 합니다.

```jsx
function Timer() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log('타이머를 시작합니다.');
    const intervalId = setInterval(() => {
      // setCount(count + 1); // 이렇게 하면 stale state 문제가 발생할 수 있습니다.
      setCount(c => c + 1); // 함수형 업데이트를 사용하는 것이 안전합니다.
    }, 1000);

    // 클린업 함수
    return () => {
      console.log('타이머를 종료합니다.');
      clearInterval(intervalId);
    };
  }, []); // 처음에 한 번만 실행

  return <p>Timer: {count}</p>;
}
```

## 2-3) useEffect 주요 실수와 해결책

`useEffect`를 사용할 때 자주 겪는 문제들과 해결 방법을 알아봅시다.

*   **문제 1: 객체나 함수를 의존성 배열에 넣었을 때**
    *   **현상**: 컴포넌트가 리렌더링될 때마다 함수나 객체가 새로 생성되므로, 의존성 배열에 넣으면 `useEffect`가 매번 다시 실행됩니다.
    *   **해결**: 함수는 `useCallback`, 객체는 `useMemo`를 사용해 렌더링 간에 동일한 참조를 유지하도록 하거나(day6에서 배웁니다), `useEffect` 안으로 옮기거나, 의존성에서 제거하는 방법을 고려합니다.

*   **문제 2: 오래된 상태 (Stale State)**
    *   **현상**: `useEffect`의 `setup` 함수는 자신이 생성된 시점의 `state`와 `props`를 기억합니다. 클린업 함수도 마찬가지입니다. 이로 인해 오래된 상태값을 참조하는 문제가 발생할 수 있습니다.
    *   **해결**: 위 `Timer` 예제처럼, `set` 함수에 값을 직접 넣는 대신 `setCount(c => c + 1)`와 같이 **함수형 업데이트**를 사용하면 항상 최신 상태를 기반으로 값을 변경할 수 있어 안전합니다.

## 2-4) 정리

*   `useEffect`의 **의존성 배열**은 Effect가 언제 다시 실행될지를 제어하는 핵심 도구입니다.
*   **클린업 함수**는 `useEffect`에서 설정한 작업(타이머, 구독 등)을 컴포넌트가 사라지거나 Effect가 재실행되기 전에 정리하여 메모리 누수와 버그를 방지합니다.
*   의존성 배열에 객체/함수를 넣을 때의 문제점과 함수형 업데이트의 필요성을 이해하면 `useEffect`를 훨씬 더 안정적으로 사용할 수 있습니다.

---

- [목차로 돌아가기](./README.md)
- [이전 강의로 이동](./01-Custom-Hooks.md)
- [다음 강의로 이동](./03-useRef-Hook.md)