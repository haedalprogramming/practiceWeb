# 실습 2: `useContext`로 다크/라이트 모드 테마 전환 기능 만들기

React 애플리케이션의 규모가 커지면 여러 컴포넌트가 동일한 데이터를 공유해야 하는 경우가 많습니다. 예를 들어, 사용자가 선택한 "테마(Theme)" 설정은 헤더, 메인 콘텐츠, 푸터 등 앱 전체에 일관되게 적용되어야 합니다. 이때 Props를 통해 데이터를 단계별로 전달하는 것은 매우 번거롭습니다.

이번 실습에서는 `useContext` Hook을 사용하여 Props drilling 없이 전역적인 상태(테마)를 관리하고, 모든 컴포넌트가 이 상태에 쉽게 접근하여 다크 모드와 라이트 모드 간의 전환을 구현하는 방법을 배웁니다.

---

## 1) 학습 목표

- `useContext`를 사용하여 전역 상태를 관리하는 방법을 이해합니다.
- Props drilling 문제점을 인지하고 `Context API`를 통해 해결하는 방법을 배웁니다.
- `Provider`와 `Consumer`의 개념을 이해하고, `useContext` Hook으로 `Consumer`를 쉽게 구현하는 방법을 익힙니다.
- 여러 컴포넌트에서 공유되는 상태를 변경하고, 변경 사항이 앱 전체에 즉시 반영되는 원리를 파악합니다.

---

## 2) 실습 요구사항

1.  `src/contexts` 폴더를 만들고 그 안에 `ThemeContext.js` 파일을 생성합니다.
2.  `ThemeContext.js`에서 `ThemeContext`를 만들고, `ThemeProvider` 컴포넌트를 구현합니다.
    - `ThemeProvider`는 `children`을 props로 받아 렌더링해야 합니다.
    - 내부적으로 `useState`를 사용해 `theme` 상태('light' 또는 'dark')를 관리합니다.
    - `theme` 상태와 `theme`을 변경하는 함수(`toggleTheme`)를 `Context.Provider`의 `value`로 전달합니다.
3.  `App.js`를 `ThemeProvider`로 감싸서, 하위의 모든 컴포넌트가 테마 컨텍스트에 접근할 수 있도록 설정합니다.
4.  `MainPage.js`와 `Header.js`, `Footer.js` 같은 여러 컴포넌트를 만듭니다.
5.  각 컴포넌트에서 `useContext` Hook을 사용해 현재 `theme` 값과 `toggleTheme` 함수를 가져옵니다.
6.  `theme` 값에 따라 조건부 스타일링을 적용하여 다크 모드와 라이트 모드를 시각적으로 구현합니다.
7.  `Header` 컴포넌트에 테마를 전환할 수 있는 버튼을 만들고, 버튼 클릭 시 `toggleTheme` 함수를 호출하여 앱 전체의 테마가 바뀌는지 확인합니다.

---

## 3) 코드 구현

### 3-1) 프로젝트 설정

`src` 폴더에 `contexts`와 `components` 디렉토리를 새로 만듭니다.

```bash
# 프로젝트의 src 폴더로 이동
cd src
# contexts와 components 디렉토리 생성
mkdir contexts components
```

### 3-2) `ThemeContext.js` 작성

`src/contexts/ThemeContext.js` 파일을 만들고 아래 코드를 작성합니다.

```javascript
import React, { createContext, useState, useContext } from 'react';

// 1. Context 생성
// createContext의 인자는 Provider를 찾지 못했을 때 사용될 기본값입니다.
const ThemeContext = createContext();

// 2. ThemeProvider 컴포넌트 생성
export function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light'); // 기본 테마는 'light'

  // 테마를 토글하는 함수
  const toggleTheme = () => {
    setTheme((prevTheme) => (prevTheme === 'light' ? 'dark' : 'light'));
  };

  // Provider는 value prop을 통해 하위 컴포넌트에게 데이터를 전달합니다.
  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

// 3. Custom Hook 생성 (컴포넌트에서 쉽게 컨텍스트를 사용하기 위함)
export function useTheme() {
  return useContext(ThemeContext);
}
```

### 3-3) 컴포넌트 작성

`src/components` 폴더 안에 `Header.js`, `MainContent.js`, `Footer.js` 파일을 만듭니다.

**`Header.js`**
```javascript
import React from 'react';
import { useTheme } from '../contexts/ThemeContext';

function Header() {
  const { theme, toggleTheme } = useTheme(); // Custom Hook 사용

  const style = {
    background: theme === 'light' ? '#eee' : '#333',
    color: theme === 'light' ? '#333' : '#eee',
    padding: '1rem',
    textAlign: 'center',
  };

  return (
    <header style={style}>
      <h1>My App Header</h1>
      <button onClick={toggleTheme}>
        Switch to {theme === 'light' ? 'Dark' : 'Light'} Mode
      </button>
    </header>
  );
}

export default Header;
```

**`MainContent.js`**
```javascript
import React from 'react';
import { useTheme } from '../contexts/ThemeContext';

function MainContent() {
  const { theme } = useTheme(); // 테마 값만 필요하므로 theme만 가져옴

  const style = {
    background: theme === 'light' ? '#fff' : '#555',
    color: theme === 'light' ? '#333' : '#fff',
    padding: '2rem',
    flexGrow: 1, // 남은 공간을 모두 차지하도록 설정
  };

  return (
    <main style={style}>
      <h2>Main Content Area</h2>
      <p>This is the main content of the application. The current theme is <strong>{theme}</strong>.</p>
    </main>
  );
}

export default MainContent;
```

**`Footer.js`**
```javascript
import React from 'react';
import { useTheme } from '../contexts/ThemeContext';

function Footer() {
  const { theme } = useTheme();

  const style = {
    background: theme === 'light' ? '#eee' : '#333',
    color: theme === 'light' ? '#333' : '#eee',
    padding: '1rem',
    textAlign: 'center',
    marginTop: 'auto', // 메인 컨텐츠가 짧아도 푸터를 하단에 고정
  };

  return (
    <footer style={style}>
      <p>© 2024 My App. All rights reserved.</p>
    </footer>
  );
}

export default Footer;
```

### 3-4) `App.js`에 적용하기

마지막으로 `App.js`에서 `ThemeProvider`로 컴포넌트들을 감싸줍니다.

```javascript
import React from 'react';
import { ThemeProvider } from './contexts/ThemeContext';
import Header from './components/Header';
import MainContent from './components/MainContent';
import Footer from './components/Footer';

function App() {
  return (
    <ThemeProvider>
      <div style={{ display: 'flex', flexDirection: 'column', minHeight: '100vh' }}>
        <Header />
        <MainContent />
        <Footer />
      </div>
    </ThemeProvider>
  );
}

export default App;
```

---

## 4) 실행 결과 확인

1.  `npm start` 또는 `yarn start`로 애플리케이션을 실행합니다.
2.  초기 화면은 라이트 모드로 표시됩니다. 헤더, 메인, 푸터가 밝은 배경색을 가집니다.
3.  헤더에 있는 "Switch to Dark Mode" 버튼을 클릭합니다.
4.  앱 전체의 배경색과 글자색이 어둡게 바뀌면서 다크 모드로 즉시 전환되는 것을 확인할 수 있습니다. 버튼의 텍스트도 "Switch to Light Mode"로 변경됩니다.
5.  버튼을 다시 클릭하면 라이트 모드로 돌아옵니다.

---

## 5) 정리

`useContext`를 사용하면 애플리케이션의 어느 컴포넌트에서든 Props를 여러 단계에 걸쳐 전달할 필요 없이 전역 데이터에 직접 접근할 수 있습니다. 이는 코드 구조를 단순하게 만들고, 유지보수를 쉽게 해줍니다. 테마, 사용자 인증 정보, 언어 설정 등 앱 전반에서 공유되어야 하는 상태를 관리할 때 `useContext`는 매우 강력한 도구입니다.

---

- [목차로 돌아가기](../README.md)
- [이전 실습으로 돌아가기](./Lab1-useLocalStorage-Hook.md)
- [다음 실습으로 이동하기](./Lab3-Timer-with-useRef.md) 