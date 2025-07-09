# 실습 2: Zustand로 전역 상태 관리하기

React에서 여러 컴포넌트가 공유하는 상태, 예를 들어 다크 모드/라이트 모드 같은 테마 설정을 관리하는 것은 까다로울 수 있습니다. Context API를 사용할 수도 있지만, 코드가 복잡해지고 불필요한 리렌더링이 발생하기 쉽습니다.

이번 실습에서는 **Zustand**를 사용하여 간단하고 효율적으로 **전역 테마 상태를 관리하는 방법**을 배웁니다. 별도의 Provider 설정 없이도 어떤 컴포넌트에서든 쉽게 상태를 읽고 업데이트할 수 있는 Zustand의 편리함을 직접 경험해 보세요.

---

## 1) 학습 목표

- Zustand를 사용하여 전역 상태 스토어(Store)를 생성하고 관리할 수 있습니다.
- `create` 함수를 사용하여 상태(state)와 상태 변경 함수(action)를 정의하는 방법을 이해합니다.
- 여러 컴포넌트에서 스토어의 상태를 구독하고, 액션을 호출하여 상태를 변경하는 방법을 익힙니다.
- Provider로 감싸주지 않아도 전역 상태에 접근할 수 있는 Zustand의 특징을 이해합니다.
- 실제 앱에서 흔히 사용되는 테마 전환 기능을 구현하며 Zustand의 실용적인 사용법을 배웁니다.

---

## 2) 실습 요구사항

1.  **Zustand 설치**: `zustand` 라이브러리를 프로젝트에 설치합니다.
2.  **테마 스토어 생성**:
    - `src/stores/themeStore.js` 파일을 생성합니다.
    - 스토어에는 `theme`이라는 상태가 있어야 하며, 초기값은 `'light'` 입니다.
    - `toggleTheme`이라는 액션(함수)이 있어야 하며, 호출될 때마다 `theme` 상태를 `'light'`에서 `'dark'`로, 또는 `'dark'`에서 `'light'`로 전환해야 합니다.
3.  **컴포넌트 구현**:
    - **`ThemeSwitcher.js`**: "테마 전환" 버튼을 포함하는 컴포넌트입니다. 이 버튼을 클릭하면 `themeStore`의 `toggleTheme` 액션을 호출해야 합니다.
    - **`App.js`**:
        - `themeStore`를 구독하여 현재 `theme` 값을 가져옵니다.
        - 가져온 `theme` 값에 따라 동적으로 `className`을 `App`의 최상위 `div`에 적용합니다. (예: `App light` 또는 `App dark`)
        - `ThemeSwitcher` 컴포넌트를 렌더링합니다.
4.  **CSS 스타일링**:
    - `App.css` 파일에 `.light`와 `.dark` 클래스에 대한 스타일을 정의하여 실제 테마 전환이 시각적으로 보이도록 만듭니다.

---

## 3) 코드 구현

### 3-1) Zustand 설치

아직 설치하지 않았다면, 터미널에서 아래 명령어를 실행하여 Zustand를 설치합니다.

```bash
npm install zustand
```

### 3-2) 테마 스토어(`themeStore`) 생성

`src` 폴더에 `stores`라는 새 폴더를 만들고, 그 안에 `themeStore.js` 파일을 생성합니다.

**`src/stores/themeStore.js`**
```javascript
import { create } from 'zustand';

const useThemeStore = create((set) => ({
  theme: 'light', // 초기 상태: 'light'
  toggleTheme: () => 
    set((state) => ({ 
      theme: state.theme === 'light' ? 'dark' : 'light' 
    })),
}));

export default useThemeStore;
```

> [!NOTE]
> `set((state) => ({...}))` 구문을 사용하면 항상 최신 상태를 기반으로 안전하게 상태를 업데이트할 수 있습니다. `toggleTheme` 함수는 현재 `theme`이 `'light'`이면 `'dark'`로, 그렇지 않으면 `'light'`로 변경하는 간단한 로직입니다.

### 3-3) 컴포넌트 및 CSS 구현

이제 스토어를 사용하는 컴포넌트들을 만들어 보겠습니다.

**`src/components/ThemeSwitcher.js`**
```javascript
import React from 'react';
import useThemeStore from '../stores/themeStore';

function ThemeSwitcher() {
  // 스토어에서 `toggleTheme` 액션만 가져옵니다.
  const toggleTheme = useThemeStore((state) => state.toggleTheme);

  return (
    <button onClick={toggleTheme}>
      테마 전환
    </button>
  );
}

export default ThemeSwitcher;
```
> [!TIP]
> `ThemeSwitcher` 컴포넌트는 오직 테마를 '변경'하는 역할만 합니다. 현재 테마가 무엇인지 알 필요가 없으므로 `theme` 상태는 구독하지 않고 `toggleTheme` 함수만 가져옵니다. 이렇게 필요한 것만 선택(select)하는 것이 Zustand의 최적화 핵심입니다.


**`App.js`**
```javascript
import React from 'react';
import useThemeStore from './stores/themeStore';
import ThemeSwitcher from './components/ThemeSwitcher';
import './App.css';

function App() {
  // 스토어에서 `theme` 상태만 가져옵니다.
  const theme = useThemeStore((state) => state.theme);

  return (
    // 현재 theme 값에 따라 className을 동적으로 설정합니다.
    <div className={`App ${theme}`}>
      <header className="App-header">
        <h1>Zustand 테마 전환 예제</h1>
        <ThemeSwitcher />
        <p>현재 테마는 <strong>{theme}</strong> 입니다.</p>
      </header>
    </div>
  );
}

export default App;
```

**`App.css`**
```css
.App {
  text-align: center;
  transition: background-color 0.3s, color 0.3s;
}

.App-header {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-size: calc(10px + 2vmin);
  min-height: 100vh;
}

/* 라이트 모드 스타일 */
.App.light {
  background-color: #ffffff;
  color: #282c34;
}

/* 다크 모드 스타일 */
.App.dark {
  background-color: #282c34;
  color: #ffffff;
}

button {
  margin-top: 20px;
  padding: 10px 20px;
  font-size: 1rem;
  cursor: pointer;
  border: 1px solid;
  background-color: transparent;
}

.light button {
  border-color: #282c34;
  color: #282c34;
}

.dark button {
  border-color: #ffffff;
  color: #ffffff;
}
```

### 3-4) 실행 및 확인

이제 애플리케이션을 실행하고 "테마 전환" 버튼을 클릭해 보세요. 화면 전체의 배경색과 글자색이 부드럽게 변하는 것을 확인할 수 있습니다.

- `ThemeSwitcher` 컴포넌트는 상태를 변경하는 `toggleTheme` 액션을 호출합니다.
- `App` 컴포넌트는 변경된 `theme` 상태를 감지하고 리렌더링되어 `className`을 업데이트합니다.
- CSS는 변경된 `className`(.light 또는 .dark)에 따라 적절한 스타일을 적용합니다.

---

## 4) 결론

이번 실습을 통해 Zustand를 사용하여 전역 상태(테마)를 얼마나 간단하게 관리할 수 있는지 배웠습니다. `Provider`로 컴포넌트 트리를 감쌀 필요도 없고, 복잡한 `useSelector`나 `useDispatch` 훅 없이 직관적인 커스텀 훅 하나로 모든 것이 해결되었습니다.

Zustand는 가볍고 빠르면서도 강력한 기능을 제공하여, 중소규모 애플리케이션의 전역 상태 관리를 위한 훌륭한 선택지가 될 수 있습니다.

---

[메인으로 돌아가기](../README.md)
[이전 실습으로 돌아가기](./Lab1-Refactor-to-TanStack-Query.md)
[다음 실습으로 이동하기](./Lab3-Signup-Form-with-RHF.md)
