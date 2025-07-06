# 1) 라우팅

## 1-2) Route, Link, Outlet 사용하기

이전 강의에서 React Router v6를 설치했습니다. 이제 React Router의 핵심 요소인 `Route`, `Link`, `Outlet`을 사용하여 웹 애플리케이션의 화면을 구성하고 페이지를 이동하는 방법을 알아보겠습니다.

### 1-2-1) `BrowserRouter`로 애플리케이션 감싸기

React Router를 사용하려면, 애플리케이션의 가장 상위 컴포넌트를 `BrowserRouter`로 감싸야 합니다. `BrowserRouter`는 웹 브라우저의 주소 변화를 감지하고, 그에 따라 적절한 화면을 보여줄 수 있도록 React Router의 기능을 활성화합니다.

일반적으로 `src/index.js` 또는 `src/main.jsx` 파일에서 다음과 같이 설정합니다.

```jsx
// src/index.js 또는 src/main.jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import { BrowserRouter } from 'react-router-dom'; // BrowserRouter 임포트
import App from './App';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <BrowserRouter> {/* App 컴포넌트를 BrowserRouter로 감싸기 */}
      <App />
    </BrowserRouter>
  </React.StrictMode>
);
```

[!IMPORTANT]
`BrowserRouter`는 웹 브라우저의 주소(URL)를 사용하여 라우팅을 관리합니다. 이 컴포넌트가 있어야 React Router가 정상적으로 동작할 수 있습니다.

### 1-2-2) `Routes`와 `Route`로 경로 설정하기

`Routes`와 `Route` 컴포넌트는 특정 주소(URL)에 어떤 컴포넌트를 보여줄지 정의할 때 사용합니다.

*   **`Routes`**: 여러 개의 `Route`들을 묶어주는 역할을 합니다. `Routes` 안에서는 오직 `Route` 컴포넌트만 사용할 수 있습니다.
*   **`Route`**: 특정 `path` (경로)에 접속했을 때 `element` (보여줄 컴포넌트)를 연결합니다.

예를 들어, `src/App.js` 파일에서 다음과 같이 설정할 수 있습니다.

```jsx
// src/App.js
import React from 'react';
import { Routes, Route } from 'react-router-dom'; // Routes와 Route 임포트

// 각 경로에 보여줄 컴포넌트들을 미리 정의합니다.
function Home() {
  return <h2>홈 페이지</h2>;
}

function About() {
  return <h2>소개 페이지</h2>;
}

function Contact() {
  return <h2>연락처 페이지</h2>;
}

function App() {
  return (
    <div>
      <h1>나의 웹사이트</h1>
      <Routes> {/* 여러 Route들을 Routes로 묶습니다. */}
        <Route path="/" element={<Home />} /> {/* '/' 경로에 접속하면 Home 컴포넌트 표시 */}
        <Route path="/about" element={<About />} /> {/* '/about' 경로에 접속하면 About 컴포넌트 표시 */}
        <Route path="/contact" element={<Contact />} /> {/* '/contact' 경로에 접속하면 Contact 컴포넌트 표시 */}
      </Routes>
    </div>
  );
}

export default App;
```

[!NOTE]
`path` 속성은 웹 브라우저의 주소창에 나타나는 경로를 의미합니다. `element` 속성에는 해당 경로에 보여줄 React 컴포넌트를 넣어줍니다.

### 1-2-3) `Link`로 페이지 이동하기

`Link` 컴포넌트는 웹 애플리케이션 내에서 다른 경로로 이동할 때 사용합니다. HTML의 `<a>` 태그와 비슷하지만, `Link`는 페이지를 새로고침하지 않고 SPA 방식의 부드러운 페이지 전환을 가능하게 합니다.

```jsx
// src/App.js (이어서)
import React from 'react';
import { Routes, Route, Link } from 'react-router-dom'; // Link 임포트

// ... (Home, About, Contact 컴포넌트 정의는 동일)

function App() {
  return (
    <div>
      <h1>나의 웹사이트</h1>
      <nav> {/* 내비게이션 메뉴를 만듭니다. */}
        <ul>
          <li>
            <Link to="/">홈</Link> {/* '/' 경로로 이동하는 링크 */}
          </li>
          <li>
            <Link to="/about">소개</Link> {/* '/about' 경로로 이동하는 링크 */}
          </li>
          <li>
            <Link to="/contact">연락처</Link> {/* '/contact' 경로로 이동하는 링크 */}
          </li>
        </ul>
      </nav>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/contact" element={<Contact />} />
      </Routes>
    </div>
  );
}

export default App;
```

[!TIP]
`Link` 컴포넌트의 `to` 속성은 이동할 경로를 지정합니다. `<a>` 태그의 `href` 속성과 유사하다고 생각하면 됩니다.

### 1-2-4) `Outlet`으로 중첩된 라우팅 구현하기

때로는 특정 경로 아래에 여러 하위 경로를 만들고 싶을 때가 있습니다. 예를 들어, `www.example.com/users` 경로 아래에 `www.example.com/users/1` (사용자 1의 상세 정보), `www.example.com/users/settings` (사용자 설정) 등과 같이 중첩된 구조를 만들 수 있습니다. 이때 `Outlet` 컴포넌트를 사용합니다.

`Outlet`은 부모 `Route` 컴포넌트가 렌더링될 때, 그 안에 중첩된 자식 `Route` 컴포넌트들이 렌더링될 위치를 지정해줍니다.

먼저, `User` 컴포넌트를 생성하고 그 안에 `Outlet`을 배치합니다.

```jsx
// src/App.js (이어서)
import React from 'react';
import { Routes, Route, Link, Outlet } from 'react-router-dom'; // Outlet 임포트

// ... (Home, About, Contact 컴포넌트 정의는 동일)

// 사용자 목록 페이지
function Users() {
  return (
    <div>
      <h2>사용자 목록</h2>
      <ul>
        <li><Link to="/users/1">사용자 1</Link></li>
        <li><Link to="/users/2">사용자 2</Link></li>
      </ul>
      <Outlet /> {/* 자식 Route 컴포넌트들이 여기에 렌더링됩니다. */}
    </div>
  );
}

// 사용자 상세 페이지
function UserDetail() {
  // 실제 애플리케이션에서는 URL에서 사용자 ID를 추출하여 해당 사용자 정보를 표시합니다.
  return <h3>사용자 상세 정보</h3>;
}

function App() {
  return (
    <div>
      <h1>나의 웹사이트</h1>
      <nav>
        <ul>
          <li><Link to="/">홈</Link></li>
          <li><Link to="/about">소개</Link></li>
          <li><Link to="/contact">연락처</Link></li>
          <li><Link to="/users">사용자</Link></li> {/* 사용자 페이지 링크 추가 */}
        </ul>
      </nav>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/contact" element={<Contact />} />
        {/* 중첩된 라우팅 설정 */}
        <Route path="/users" element={<Users />}> {/* 부모 경로 */}
          <Route path=":userId" element={<UserDetail />} /> {/* 자식 경로. :userId는 동적인 값(파라미터)을 의미합니다. */}
        </Route>
      </Routes>
    </div>
  );
}

export default App;
```

[!CAUTION]
`Outlet`이 없으면 중첩된 자식 `Route` 컴포넌트들은 화면에 렌더링되지 않습니다. `Outlet`은 자식 `Route`의 내용을 "끼워 넣을" 위치를 지정하는 역할을 합니다.

### 1-2-5) 정리

*   `BrowserRouter`: React Router를 사용하기 위해 애플리케이션을 감싸는 최상위 컴포넌트입니다.
*   `Routes`: 여러 `Route`들을 묶어주는 컨테이너입니다.
*   `Route`: 특정 `path`에 `element` (컴포넌트)를 연결하여 화면에 보여줍니다.
*   `Link`: 페이지를 새로고침하지 않고 다른 경로로 이동할 수 있게 해주는 컴포넌트입니다.
*   `Outlet`: 중첩된 라우팅에서 자식 `Route` 컴포넌트들이 렌더링될 위치를 지정합니다.

이러한 React Router의 핵심 컴포넌트들을 이해하고 사용하면, 복잡한 SPA를 효율적으로 구성하고 관리할 수 있습니다.

---

- [목차로 돌아가기](../README.md)
- [이전 강의로 이동](01-Introducing-React-Router.md)
- [다음 강의로 이동](03-Component-Design-Patterns.md)