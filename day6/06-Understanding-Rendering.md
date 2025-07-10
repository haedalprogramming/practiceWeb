# 6. React의 렌더링 이해하기

React 애플리케이션의 동작 원리를 깊이 이해하고 성능을 최적화하기 위해서는 '렌더링(Rendering)' 과정을 아는 것이 매우 중요합니다. 이번 시간에는 React가 어떻게 화면을 그리고 업데이트하는지, 그 핵심 메커니즘인 렌더링에 대해 자세히 알아보겠습니다.

---

## 1) 렌더링(Rendering)이란?

**렌더링**은 간단히 말해 **웹 페이지에 내용을 그리는 과정**을 의미합니다. 우리가 작성한 React 컴포넌트 코드(JSX)가 실제 사용자가 볼 수 있는 화면, 즉 UI(User Interface)로 변환되는 모든 과정을 렌더링이라고 부릅니다.

React는 이 렌더링 과정을 매우 효율적으로 처리하여 사용자에게 빠르고 부드러운 경험을 제공합니다.

> [!NOTE]
> '렌더링'이라는 용어는 "만들다", "표현하다"라는 뜻의 'render'에서 유래했습니다. 즉, 코드를 실제 화면으로 만들어 표현하는 과정이라고 생각할 수 있습니다.

---

## 2) DOM과 가상 DOM(Virtual DOM)

React의 효율적인 렌더링 방식을 이해하려면 먼저 DOM과 가상 DOM의 개념을 알아야 합니다.

### 2-1) DOM (Document Object Model)

**DOM**은 **웹 문서(HTML)의 구조를 객체(Object) 형태로 표현한 모델**입니다. 브라우저는 HTML 문서를 읽어들여 `<html>`, `<body>`, `<div>` 같은 각 태그를 하나의 객체로 인식하고, 이들 사이의 관계를 나무와 같은 계층 구조(Tree Structure)로 만듭니다.

JavaScript는 이 DOM을 통해 HTML 요소에 접근하고 내용을 변경하거나 스타일을 수정할 수 있습니다. 예를 들어, 특정 `<div>` 요소의 텍스트를 바꾸거나 버튼 클릭 시 색상을 변경하는 작업은 모두 DOM을 조작하는 것입니다.

하지만 DOM을 직접적으로, 그리고 자주 변경하는 것은 비용이 많이 드는 작업입니다. DOM이 변경될 때마다 브라우저는 화면을 다시 계산하고 그리는 과정을 거치는데, 이 과정이 반복되면 애플리케이션 성능이 저하될 수 있습니다.

### 2-2) 가상 DOM (Virtual DOM)

React는 DOM을 직접 조작하는 대신 **가상 DOM**이라는 중간 단계를 사용합니다.

**가상 DOM**은 **실제 DOM의 모습을 메모리상에 그대로 복사해 둔 가상의 표현**입니다. React는 컴포넌트의 상태(State)가 변경되면 실제 DOM을 바로 바꾸지 않고, 먼저 메모리상에 있는 가상 DOM을 업데이트합니다.

이점은 다음과 같습니다.
- **빠른 처리 속도**: 메모리에서 동작하기 때문에 실제 브라우저의 DOM을 건드리는 것보다 훨씬 빠릅니다.
- **효율적인 업데이트**: 변경이 발생하면, React는 새로운 가상 DOM과 이전 가상 DOM을 비교하여 바뀐 부분만 찾아냅니다. 그리고 그 차이점만 실제 DOM에 딱 한 번 적용합니다.

> [!IMPORTANT]
> React는 가상 DOM을 활용하여 <strong>"어디가 바뀌었는지"</strong>를 효율적으로 찾아내고, <strong>"꼭 필요한 최소한의 변경"</strong>만 실제 DOM에 적용합니다. 이 덕분에 불필요한 렌더링을 줄이고 성능을 크게 향상시킬 수 있습니다.

---

## 3) React의 렌더링 과정 (Reconciliation)

React가 변경 사항을 감지하고 화면을 업데이트하는 과정을 특별히 **재조정(Reconciliation)**이라고 부릅니다.

재조정 과정은 다음과 같은 단계로 이루어집니다.

1.  **변경 발생**: 사용자의 입력, 데이터 요청 등으로 컴포넌트의 `state`나 `props`가 변경됩니다.
2.  **가상 DOM 업데이트**: React는 변경된 `state`나 `props`를 기반으로 새로운 가상 DOM을 생성합니다.
3.  **비교 (Diffing)**: React는 이전 가상 DOM과 새로 생성된 가상 DOM을 비교하여 어떤 부분이 달라졌는지 찾아냅니다. 이 비교 알고리즘을 'Diffing'이라고 합니다.
4.  **실제 DOM 업데이트**: React는 비교를 통해 찾아낸 변경 사항만 실제 DOM에 적용합니다.

이 과정을 통해 React는 전체 페이지를 새로고침하지 않고도 특정 부분만 빠르고 정확하게 업데이트할 수 있습니다.

---

## 4) 리렌더링(Re-rendering)이 발생하는 조건

그렇다면 React 컴포넌트는 정확히 언제 다시 렌더링(리렌더링)될까요? 주요 조건은 다음과 같습니다.

### 4-1) 컴포넌트의 state가 변경될 때

`useState` Hook을 사용하여 관리하는 `state`가 `set` 함수(`setState`)에 의해 새로운 값으로 변경되면, 해당 `state`를 사용하는 컴포넌트와 그 자식 컴포넌트들이 리렌더링됩니다.

```jsx
function Counter() {
  const [count, setCount] = useState(0);

  // 버튼을 클릭하면 count state가 변경되어 Counter 컴포넌트가 리렌더링됩니다.
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

### 4-2) 부모 컴포넌트로부터 받는 props가 변경될 때

부모 컴포넌트가 전달하는 `props`의 값이 변경되면, 해당 `props`를 받는 자식 컴포넌트는 리렌더링됩니다.

```jsx
function Parent() {
  const [text, setText] = useState("Hello");

  return (
    <div>
      <button onClick={() => setText("World")}>Change Text</button>
      <Child message={text} />
    </div>
  );
}

// Parent의 text state가 바뀌면 Child 컴포넌트로 전달되는 message prop이 변경되므로
// Child 컴포넌트가 리렌더링됩니다.
function Child({ message }) {
  return <p>{message}</p>;
}
```

### 4-3) 부모 컴포넌트가 리렌더링될 때

자식 컴포넌트의 `state`나 `props`가 변경되지 않았더라도, **부모 컴포넌트가 리렌더링되면 그 자식 컴포넌트들도 기본적으로 함께 리렌더링**됩니다. 이는 React의 기본 동작 방식입니다.

> [!CAUTION]
> 부모 컴포넌트의 리렌더링으로 인해 불필요한 자식 컴포넌트의 리렌더링이 발생하면 성능 문제가 생길 수 있습니다. 추후에 배우게 될 `React.memo`와 같은 최적화 기법을 사용하여 이러한 문제를 해결할 수 있습니다.

---

## 5) 정리

- **렌더링**은 React 코드를 실제 화면으로 그리는 과정입니다.
- **가상 DOM**은 실제 DOM의 복사본으로, React는 이를 통해 빠르고 효율적인 업데이트를 수행합니다.
- <strong>재조정(Reconciliation)</strong>은 변경된 부분을 찾아 실제 DOM에 최소한으로 반영하는 과정입니다.
- **리렌더링**은 주로 `state` 변경, `props` 변경, 또는 부모 컴포넌트의 리렌더링 시에 발생합니다.

렌더링 과정을 이해하는 것은 React의 동작 방식을 파악하고, 앞으로 마주할 수 있는 성능 문제를 해결하는 데 튼튼한 기초가 될 것입니다.

---

- [목차로 돌아가기](./README.md)
- [이전 강의로 이동](./05-Error-Handling.md)
- [다음 강의로 이동](./07-Rendering-Optimization.md)
