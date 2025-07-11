# 5. React 에러 처리 (Error Boundaries)

열심히 만든 애플리케이션이 사소한 오류 하나 때문에 전체가 하얀 화면으로 멈춰버린다면 사용자 경험에 매우 좋지 않을 것입니다. 예를 들어, 특정 컴포넌트가 데이터를 불러오다 실패했을 때, 그 컴포넌트만 에러 메시지를 보여주고 나머지 부분은 정상적으로 동작하게 할 수는 없을까요?

이러한 문제를 해결하기 위해 React는 **에러 바운더리(Error Boundaries)** 라는 개념을 도입했습니다.

---

### 목차

1.  [에러 바운더리란?](#1-에러-바운더리란)
2.  [클래스 컴포넌트로 에러 바운더리 만들기](#2-클래스-컴포넌트로-에러-바운더리-만들기)
3.  [에러 바운더리 사용하기](#3-에러-바운더리-사용하기)
4.  [함수형 컴포넌트와 `react-error-boundary`](#4-함수형-컴포넌트와-react-error-boundary)
5.  [에러 바운더리, 어디에 위치시켜야 할까?](#5-에러-바운더리-어디에-위치시켜야-할까)

---

## 1) 에러 바운더리란?

<strong>에러 바운더리(Error Boundary)</strong>는 이름 그대로 '에러의 경계'를 만드는 React 컴포넌트입니다. 이 컴포넌트는 자신의 자식 컴포넌트 트리에서 발생하는 JavaScript 에러를 감지하여 기록하고, 에러가 발생한 컴포넌트 대신 미리 준비해둔 대체 UI(Fallback UI)를 보여주는 역할을 합니다.

> [!WARNING]
> **에러 바운더리가 감지할 수 없는 에러들**
> 에러 바운더리는 모든 종류의 에러를 감지하지는 못합니다. 다음과 같은 경우에는 동작하지 않습니다.
> -   **이벤트 핸들러 내부의 에러**: `onClick`, `onChange` 등 이벤트 핸들러 함수 안에서 발생하는 에러는 React의 렌더링 과정과 무관하므로 에러 바운더리가 감지할 수 없습니다. (이 경우, 전통적인 `try...catch` 구문을 사용해야 합니다.)
> -   **비동기 코드**: `setTimeout`, `fetch` API 호출 등 비동기적으로 실행되는 코드 내부의 에러
> -   **서버 사이드 렌더링(SSR)**
> -   **에러 바운더리 자신에게서 발생하는 에러**

## 2) 클래스 컴포넌트로 에러 바운더리 만들기

React 공식 문서에 따르면, 에러 바운더리는 반드시 **클래스 컴포넌트**로 만들어야 합니다. 함수형 컴포넌트에는 아직 에러 바운더리 역할을 하는 Hook이 없기 때문입니다.

클래스 컴포넌트가 에러 바운더리가 되기 위해서는 다음 두 가지 생명주기(Lifecycle) 메서드 중 하나 이상을 구현해야 합니다.

-   `static getDerivedStateFromError(error)`: 자식 컴포넌트에서 에러가 발생했을 때 호출됩니다. 이 메서드에서는 에러가 발생했음을 나타내는 상태 값을 반환하여, 대체 UI를 렌더링하도록 지시합니다.
-   `componentDidCatch(error, errorInfo)`: 에러 정보나 추가 정보를 기록(logging)할 때 사용됩니다. (예: 에러 리포팅 서비스에 정보 전송)

다음은 간단한 에러 바운더리 컴포넌트 예제입니다.

```jsx
// src/ErrorBoundary.jsx
import React from 'react';

class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  // 1. 에러가 발생하면 호출되어 state를 변경
  static getDerivedStateFromError(error) {
    // 다음 렌더링에서 대체 UI가 표시되도록 상태를 업데이트
    return { hasError: true };
  }

  // 2. 에러 정보를 기록
  componentDidCatch(error, errorInfo) {
    // 예: Sentry, LogRocket 같은 서비스에 에러 리포트 전송
    console.error("Uncaught error:", error, errorInfo);
  }

  render() {
    // 3. state의 hasError 값에 따라 다른 UI를 렌더링
    if (this.state.hasError) {
      // 미리 준비해둔 대체 UI
      return <h1>죄송합니다. 문제가 발생했습니다.</h1>;
    }

    // 에러가 없다면 자식 컴포넌트를 그대로 렌더링
    return this.props.children; 
  }
}

export default ErrorBoundary;
```

## 3) 에러 바운더리 사용하기

에러 바운더리를 사용하는 방법은 아주 간단합니다. 에러가 발생할 가능성이 있는 컴포넌트를 직접 만든 `ErrorBoundary` 컴포넌트로 감싸주기만 하면 됩니다.

```jsx
import ErrorBoundary from './ErrorBoundary';
import Profile from './Profile'; // 에러가 발생할 수 있는 컴포넌트

function App() {
  return (
    <div>
      <h1>내 애플리케이션</h1>
      <ErrorBoundary>
        <Profile />
      </ErrorBoundary>
    </div>
  );
}
```

이제 `Profile` 컴포넌트나 그 하위 컴포넌트에서 렌더링 도중 에러가 발생하면, `ErrorBoundary`가 이를 감지하여 "죄송합니다. 문제가 발생했습니다." 라는 `h1` 태그를 보여주게 됩니다.

## 4) 함수형 컴포넌트와 `react-error-boundary`

"에러 바운더리는 클래스 컴포넌트로만 만들 수 있다니, 함수형 컴포넌트와 Hook을 사용하는 현대적인 React 개발 방식과는 맞지 않는 것 같아요."

네, 맞습니다. 그래서 커뮤니티에서는 이러한 불편함을 해소하기 위해 `react-error-boundary` 라는 훌륭한 라이브러리를 만들었습니다. 이 라이브러리를 사용하면 함수형 컴포넌트 환경에서도 매우 쉽고 유연하게 에러 처리를 할 수 있습니다.

### 4-1) `react-error-boundary` 설치 및 사용법

먼저 라이브러리를 설치합니다.
```bash
npm install react-error-boundary
```

이제 `ErrorBoundary` 컴포넌트를 라이브러리에서 가져와 사용하면 됩니다.

```jsx
import { ErrorBoundary } from 'react-error-boundary';
import Profile from './Profile';

// 에러 발생 시 보여줄 대체 컴포넌트
function ErrorFallback({error, resetErrorBoundary}) {
  return (
    <div role="alert">
      <p>문제가 발생했습니다:</p>
      <pre>{error.message}</pre>
      <button onClick={resetErrorBoundary}>다시 시도</button>
    </div>
  )
}

function App() {
  return (
    <div>
      <h1>내 애플리케이션</h1>
      <ErrorBoundary
        FallbackComponent={ErrorFallback}
        onReset={() => console.log('다시 시도!')}
      >
        <Profile />
      </ErrorBoundary>
    </div>
  );
}
```

> [!NOTE]
> `react-error-boundary`의 장점
> - `FallbackComponent` prop을 통해 에러 발생 시 보여줄 UI를 간단한 함수형 컴포넌트로 전달할 수 있습니다.
> - `fallback` prop을 사용하면 JSX를 직접 전달할 수도 있습니다.
> - `resetErrorBoundary` 함수를 제공하여, 사용자가 '다시 시도' 버튼을 클릭했을 때 에러 상태를 초기화하고 컴포넌트를 다시 렌더링하도록 쉽게 구현할 수 있습니다.
> - 이벤트 핸들러 내부의 에러도 처리할 수 있는 `useErrorHandler` 훅을 제공하는 등 더 강력한 기능을 지원합니다.

우리 커리큘럼에서는 실용성과 편의성을 고려하여 **`react-error-boundary` 라이브러리 사용을 권장**합니다.

## 5) 에러 바운더리, 어디에 위치시켜야 할까?

에러 바운더리를 어디에 둘지는 애플리케이션의 구조에 따라 달라집니다.

-   **최상위 레벨**: 전체 애플리케이션을 하나의 에러 바운더리로 감싸서, 어떤 페이지에서든 에러가 발생하면 "서비스에 문제가 발생했습니다"와 같은 공통 에러 페이지를 보여줄 수 있습니다.
-   **개별 위젯/컴포넌트**: 사이드바, 뉴스피드, 채팅창 등 독립적으로 동작하는 주요 UI 영역들을 각각 에러 바운더리로 감싸서, 하나의 위젯이 실패하더라도 다른 부분은 계속 사용할 수 있도록 만들 수 있습니다.

일반적으로는 여러 에러 바운더리를 조합하여 사용하는 것이 가장 좋습니다. 이를 통해 사용자 경험을 해치지 않으면서 안정적인 서비스를 제공할 수 있습니다.

---

- [목차로 돌아가기](./README.md)
- [이전 강의로 이동](./04-Getting-Started-Zustand.md)
- [다음 강의로 이동](./06-Understanding-Rendering.md)
