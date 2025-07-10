# 실습 3: `useRef`로 정확한 타이머 만들기

`useState`로 관리되는 상태는 값이 변경될 때마다 컴포넌트를 리렌더링시킵니다. 하지만 때로는 리렌더링을 유발하지 않으면서 컴포넌트의 생명주기 동안 특정 값을 계속 유지하고 싶을 때가 있습니다. 예를 들어, `setInterval`이나 `setTimeout`의 ID 값, 또는 DOM 요소 자체에 직접 접근해야 할 때 `useRef` Hook이 유용하게 사용됩니다.

이번 실습에서는 `useRef`를 사용하여 `setInterval`의 ID를 저장하고, 이를 통해 타이머를 정확하게 시작, 정지, 리셋하는 방법을 배웁니다.

---

## 1) 학습 목표

- `useRef` Hook의 기본 사용법과 목적을 이해합니다.
- `useRef`로 생성된 값이 컴포넌트 리렌더링에 영향을 주지 않고 유지되는 특성을 파악합니다.
- `setInterval`, `clearInterval`과 같은 브라우저 Web API를 React 컴포넌트 내에서 `useEffect`와 함께 안전하게 사용하는 방법을 익힙니다.
- 상태 관리(`useState`)와 참조 관리(`useRef`)의 차이점을 명확히 구분하고 적절한 상황에 사용하는 능력을 기릅니다.

---

## 2) `useRef`는 언제 사용할까?

`useRef`는 크게 두 가지 주요 사용 사례를 가집니다.

1.  **DOM 요소에 직접 접근**: 특정 DOM 노드(예: `input` 요소에 포커스 주기, 비디오 재생하기 등)를 직접 제어해야 할 때 사용합니다.
2.  **리렌더링 없는 값 저장**: 컴포넌트가 리렌더링 되어도 값이 초기화되지 않아야 하지만, 그 값의 변경이 리렌더링을 유발해서는 안 되는 경우에 사용합니다. 이번 실습의 `setInterval` ID 저장이 바로 이 경우에 해당합니다.

`useRef`는 `.current` 프로퍼티를 가진 객체를 반환합니다. 이 `.current` 프로퍼티에 원하는 값을 저장할 수 있으며, 이 값을 변경해도 컴포넌트는 다시 렌더링되지 않습니다.

---

## 3) 실습 요구사항

1.  화면에 초(second)를 표시하는 `Timer` 컴포넌트를 만듭니다.
2.  화면에는 현재 시간을 나타내는 숫자와 함께 `시작`, `정지`, `리셋` 세 개의 버튼이 있어야 합니다.
3.  `useState`를 사용하여 화면에 표시될 시간(`seconds`) 상태를 관리합니다.
4.  `useRef`를 사용하여 `setInterval` 함수의 ID를 저장할 `intervalRef`를 생성합니다.
5.  **시작 버튼**: 클릭하면 `setInterval`을 사용하여 1초마다 `seconds` 상태를 1씩 증가시킵니다. 생성된 interval ID는 `intervalRef.current`에 저장합니다.
6.  **정지 버튼**: 클릭하면 `intervalRef.current`에 저장된 ID를 사용하여 `clearInterval`을 호출, 타이머를 멈춥니다.
7.  **리셋 버튼**: 클릭하면 `seconds` 상태를 0으로 초기화하고, 만약 타이머가 동작 중이었다면 `clearInterval`로 멈춥니다.
8.  컴포넌트가 언마운트될 때 메모리 누수를 방지하기 위해 `useEffect`의 반환(cleanup) 함수에서 `clearInterval`을 호출하는 것을 잊지 말아야 합니다.

---

## 4) 코드 구현

### 4-1) `Timer.js` 컴포넌트 작성

`src/components/Timer.js` 파일을 새로 만들고 아래와 같이 코드를 작성합니다.

```javascript
import React, { useState, useRef, useEffect } from 'react';

function Timer() {
  const [seconds, setSeconds] = useState(0);
  const intervalRef = useRef(null); // setInterval의 ID를 저장하기 위한 ref

  // 컴포넌트가 언마운트될 때 interval을 정리(clean up)합니다.
  useEffect(() => {
    // 이 effect는 오직 컴포넌트가 마운트될 때 한 번, 언마운트될 때 한 번 실행됩니다.
    // 따라서 의존성 배열은 비어있어야 합니다.
    return () => clearInterval(intervalRef.current);
  }, []);

  const handleStart = () => {
    // 이미 타이머가 실행 중이면 아무것도 하지 않습니다.
    if (intervalRef.current !== null) {
      console.log('Timer is already running.');
      return;
    }

    // 1초마다 seconds를 1씩 증가시키는 interval을 시작합니다.
    intervalRef.current = setInterval(() => {
      setSeconds((prevSeconds) => prevSeconds + 1);
    }, 1000);
    console.log('Timer started. Interval ID:', intervalRef.current);
  };

  const handleStop = () => {
    // interval을 멈추고 ref의 값을 null로 설정합니다.
    clearInterval(intervalRef.current);
    intervalRef.current = null;
    console.log('Timer stopped.');
  };

  const handleReset = () => {
    // interval을 멈추고 seconds를 0으로 리셋합니다.
    clearInterval(intervalRef.current);
    intervalRef.current = null;
    setSeconds(0);
    console.log('Timer reset.');
  };

  return (
    <div style={{ textAlign: 'center', marginTop: '50px' }}>
      <h1>{seconds}초</h1>
      <div>
        <button onClick={handleStart} style={{ marginRight: '10px' }}>
          시작
        </button>
        <button onClick={handleStop} style={{ marginRight: '10px' }}>
          정지
        </button>
        <button onClick={handleReset}>
          리셋
        </button>
      </div>
    </div>
  );
}

export default Timer;
```

### 4-2) `App.js`에서 컴포넌트 사용

`App.js`의 내용을 수정하여 `Timer` 컴포넌트를 렌더링합니다.

```javascript
import React from 'react';
import Timer from './components/Timer';
import './App.css';

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <h1>useRef로 만드는 타이머</h1>
        <Timer />
      </header>
    </div>
  );
}

export default App;
```

---

## 5) 실행 결과 확인

1.  `npm start` 또는 `yarn start`로 애플리케이션을 실행합니다.
2.  화면에 '0초'와 세 개의 버튼이 표시됩니다.
3.  **시작** 버튼을 누르면 숫자가 1초에 1씩 증가합니다. 브라우저 콘솔을 확인하면 "Timer started" 메시지와 함께 interval ID가 출력됩니다.
4.  **정지** 버튼을 누르면 숫자가 그 상태로 멈춥니다. 콘솔에는 "Timer stopped" 메시지가 출력됩니다.
5.  다시 **시작** 버튼을 누르면 멈췄던 숫자부터 다시 시간이 흐릅니다.
6.  **리셋** 버튼을 누르면 숫자가 '0초'로 초기화되고 타이머가 멈춥니다. 콘솔에는 "Timer reset" 메시지가 출력됩니다.

---

## 6) 정리

만약 `intervalId`를 `useState`로 관리했다면 어떻게 됐을까요? `handleStart` 함수 내에서 `setIntervalId(...)`가 호출될 때마다 리렌더링이 발생하여 의도치 않은 동작을 유발할 수 있습니다.

`useRef`는 이처럼 렌더링과 관련 없는 값을 컴포넌트 인스턴스 내에서 안전하게 보관하고 싶을 때 사용하는 강력한 도구입니다. `useRef`의 `.current` 프로퍼티는 일반적인 자바스크립트 객체처럼 자유롭게 읽고 수정할 수 있으며, 이 과정이 React의 렌더링 흐름에 전혀 영향을 미치지 않는다는 점을 기억하는 것이 중요합니다.

---

- [목차로 돌아가기](../README.md)
- [이전 실습으로 돌아가기](./Lab2-Theme-Switcher.md)
- [다음 실습으로 이동하기](./Lab4-Blog-with-MSW.md) 