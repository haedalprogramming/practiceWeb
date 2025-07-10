# 실습 1: 나만의 `useLocalStorage` Custom Hook 만들기

`useState`를 사용하면 컴포넌트 내에서 상태를 관리할 수 있지만, 브라우저를 새로고침하면 상태가 초기화됩니다. 이번 실습에서는 `useState`와 `useEffect`를 결합하여, 브라우저의 `localStorage`에 데이터를 저장함으로써 새로고침해도 값이 유지되는 `useLocalStorage`라는 Custom Hook을 직접 만들어 보겠습니다.

---

## 1) 학습 목표

- `useState`와 `useEffect` Hook을 조합하여 새로운 Hook을 만드는 방법을 이해합니다.
- Custom Hook을 통해 재사용 가능한 로직을 분리하고 캡슐화하는 방법을 배웁니다.
- 브라우저 저장소인 `localStorage`의 개념과 사용법을 익힙니다.
- 상태가 변경될 때 특정 작업을 수행(Side Effect)하기 위해 `useEffect`를 사용하는 방법을 복습합니다.

---

## 2) `localStorage`란?

`localStorage`는 웹 브라우저에서 제공하는 간단한 키-값(Key-Value) 형태의 저장소입니다. 여기에 저장된 데이터는 사용자가 직접 삭제하거나 브라우저 캐시를 지우지 않는 한 영구적으로 유지됩니다.

- **데이터 저장**: `localStorage.setItem('key', 'value');`
- **데이터 읽기**: `localStorage.getItem('key');`
- **데이터 삭제**: `localStorage.removeItem('key');`

> [!NOTE]
> `localStorage`에는 문자열(String) 형태의 데이터만 저장할 수 있습니다. 만약 객체나 배열을 저장하려면 `JSON.stringify()`를 사용해 문자열로 변환해야 하고, 다시 읽어올 때는 `JSON.parse()`를 사용해 원래 형태로 복원해야 합니다.

---

## 3) 실습 요구사항

1.  `src/hooks` 폴더를 만들고 그 안에 `useLocalStorage.js` 파일을 생성합니다.
2.  `useLocalStorage` 라는 이름의 Custom Hook을 구현합니다.
    - 이 Hook은 `key` (저장할 이름)와 `initialValue` (초기값) 두 개의 인자를 받습니다.
3.  Hook의 내부 로직은 다음과 같습니다.
    - `useState`를 사용하여 상태를 관리합니다. 이때 초기값은 `localStorage`에 `key`로 저장된 값이 있으면 그 값을 사용하고, 없으면 `initialValue`를 사용하도록 설정합니다.
    - `useEffect`를 사용하여 상태값이 변경될 때마다 `localStorage`에 자동으로 새로운 값을 저장하도록 구현합니다.
4.  `App.js`에서 `useLocalStorage` Hook을 사용하여 간단한 메모장 애플리케이션을 만듭니다.
5.  텍스트를 입력하고 브라우저를 새로고침했을 때, 입력한 내용이 그대로 유지되는지 확인합니다.

---

## 4) 코드 구현

### 4-1) 프로젝트 설정

`create-react-app`으로 생성된 프로젝트의 `src` 폴더에 `hooks` 디렉토리를 새로 만듭니다.

```bash
# 프로젝트의 src 폴더로 이동
cd src
# hooks 디렉토리 생성
mkdir hooks
```

### 4-2) `useLocalStorage.js` 작성

`src/hooks/useLocalStorage.js` 파일을 만들고 아래 코드를 작성합니다.

```javascript
import { useState, useEffect } from 'react';

// useLocalStorage Custom Hook 정의
export function useLocalStorage(key, initialValue) {
  // 1. useState를 사용하여 상태를 초기화합니다.
  // 초기값 함수를 사용하여 localStorage에서 값을 한 번만 읽어오도록 최적화합니다.
  const [storedValue, setStoredValue] = useState(() => {
    try {
      // localStorage에서 key에 해당하는 값을 가져옵니다.
      const item = window.localStorage.getItem(key);
      // 값이 있으면 JSON으로 파싱하고, 없으면 initialValue를 반환합니다.
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      // 에러가 발생하면 콘솔에 에러를 출력하고 initialValue를 반환합니다.
      console.log(error);
      return initialValue;
    }
  });

  // 2. useEffect를 사용하여 storedValue가 변경될 때마다 localStorage를 업데이트합니다.
  useEffect(() => {
    try {
      // 상태 값을 JSON 문자열로 변환하여 localStorage에 저장합니다.
      window.localStorage.setItem(key, JSON.stringify(storedValue));
    } catch (error) {
      // 에러가 발생하면 콘솔에 에러를 출력합니다.
      console.log(error);
    }
  }, [key, storedValue]); // key나 storedValue가 변경될 때만 이 effect를 실행합니다.

  // 3. useState와 동일한 인터페이스 [값, 설정함수]를 반환합니다.
  return [storedValue, setStoredValue];
}
```

> [!TIP]
> **초기값 함수 사용의 이점**
> `useState`에 그냥 `localStorage.getItem(...)`을 넣으면 컴포넌트가 리렌더링될 때마다 `localStorage`를 계속 읽게 됩니다. 하지만 `useState(() => { ... })` 처럼 함수를 전달하면, 이 함수는 오직 컴포넌트가 처음 마운트될 때 **한 번만** 실행됩니다. 이는 불필요한 연산을 줄여 성능을 최적화하는 좋은 방법입니다.

### 4-3) `App.js`에서 Hook 사용하기

이제 직접 만든 `useLocalStorage` Hook을 `App.js`에서 사용해 보겠습니다.

```javascript
import React from 'react';
import { useLocalStorage } from './hooks/useLocalStorage';
import './App.css';

function App() {
  // useLocalStorage Hook 사용
  // 'memo'라는 키로 localStorage에 저장하고, 초기값은 빈 문자열입니다.
  const [memo, setMemo] = useLocalStorage('memo', '');

  const handleMemoChange = (e) => {
    setMemo(e.target.value);
  };

  return (
    <div className="App">
      <header className="App-header">
        <h1>새로고침해도 사라지지 않는 메모장</h1>
        <textarea
          style={{ width: '300px', height: '200px', fontSize: '16px' }}
          value={memo}
          onChange={handleMemoChange}
          placeholder="여기에 메모를 입력하세요..."
        />
      </header>
    </div>
  );
}

export default App;
```

---

## 5) 실행 결과 확인

1.  `npm start` 또는 `yarn start`로 애플리케이션을 실행합니다.
2.  메모장에 아무 텍스트나 입력합니다.
3.  브라우저를 새로고침(F5 또는 Ctrl/Cmd + R)합니다.
4.  새로고침 후에도 이전에 입력했던 텍스트가 그대로 남아있는 것을 확인할 수 있습니다.
5.  브라우저 개발자 도구(F12)를 열고 `Application` 탭(Chrome 기준)으로 이동하여 `Local Storage`를 살펴보면, `memo`라는 키와 함께 입력한 내용이 저장된 것을 볼 수 있습니다.

---

## 6) 정리

이번 실습을 통해 우리는 여러 React Hook을 조합하여 자신만의 **Custom Hook**을 만드는 방법을 배웠습니다. `useLocalStorage`는 상태 관리 로직을 재사용 가능한 단위로 만든 좋은 예시입니다. 이제 다른 프로젝트에서도 이 Hook을 가져와 간편하게 영속적인 상태를 관리할 수 있게 되었습니다.

---

- [목차로 돌아가기](../README.md)
- [이전 강의로 돌아가기](./01-Custom-Hooks.md)
- [다음 실습으로 이동하기](./Lab2-Theme-Switcher.md) 