
# Lab1. React로 자기소개 페이지 만들기

`day3`에서 HTML과 CSS만으로 만들었던 자기소개 페이지를 이제는 **Vite 기반의 React** 환경에서 완전히 새로운 애플리케이션으로 재탄생시켜 보겠습니다. 이 실습에서는 `day4`에서 배운 **React Router**, **컴포넌트 기반 설계**, 그리고 **스타일링** 기법을 모두 활용하여, 여러분만의 동적인 자기소개 웹사이트를 직접 설계하고 만들어봅니다.

이 실습의 목표는 정해진 코드를 따라 치는 것이 아니라, 배운 개념을 응용하여 스스로 문제를 해결하고 창의적으로 결과물을 만드는 것입니다.

## 실습 목표

*   Vite를 사용하여 새로운 React 프로젝트를 시작하고 기본 환경을 설정합니다.
*   UI를 여러 컴포넌트(예: `Header`, `Footer`, `Home`, `About`)로 직접 분리하여 설계하고 관리합니다.
*   React Router를 사용하여 페이지 간의 이동(라우팅)을 스스로 구현합니다.
*   CSS Modules를 사용하여 컴포넌트별로 스타일을 직접 적용하고 충돌을 방지합니다.
*   완성된 React 애플리케이션의 구조를 이해하고 자신만의 포트폴리오 페이지의 기반을 다집니다.

---

## Part 1: 기본 프로젝트 설정

먼저, 실습을 진행할 React 프로젝트의 뼈대를 만듭니다.

### Step 1: 새로운 Vite + React 프로젝트 생성

터미널을 열고 다음 명령어를 실행하여 `my-profile-app`이라는 이름의 새로운 Vite 기반 React 프로젝트를 생성하세요.

```bash
npm create vite@latest my-profile-app -- --template react
```

> [!IMPORTANT]
> Vite는 매우 빠른 빌드 속도와 즉각적인 모듈 리로딩(HMR)을 제공하는 최신 프론트엔드 개발 도구입니다. Create React App보다 훨씬 가볍고 빠르게 개발을 시작할 수 있습니다.

프로젝트 생성이 완료되면, 해당 폴더로 이동하여 필요한 패키지를 설치합니다.

```bash
cd my-profile-app
npm install
```

### Step 2: React Router 설치

페이지 간 이동(라우팅) 기능을 구현하기 위해 React Router 라이브러리를 설치합니다.

```bash
npm install react-router-dom
```

### Step 3: 프로젝트 폴더 구조 설계

`src` 폴더 안에 있는 기본 파일들을 정리하고, 앞으로 만들 컴포넌트와 페이지들을 담을 폴더를 직접 생성합니다.

1.  **`src` 폴더 정리**: `src/assets` 폴더의 `react.svg` 파일을 삭제해도 좋습니다. `App.css`와 `index.css`의 기본 스타일은 대부분 지우고 시작하는 것이 편리합니다.
2.  **폴더 생성**: `src` 폴더 안에 `components`와 `pages`라는 두 개의 폴더를 만듭니다.
    *   `components`: 여러 페이지에서 공통으로 사용될 재사용 가능한 컴포넌트(예: `Header`, `Footer`)를 보관할 곳입니다.
    *   `pages`: 각 페이지를 나타내는 컴포넌트(예: `Home`, `About`)를 보관할 곳입니다.

정리 후 `src` 폴더 구조는 다음과 같아집니다.

```
src/
├── components/
├── pages/
├── App.css
├── App.jsx
├── index.css
└── main.jsx
```

> [!NOTE]
> Vite 기반의 React 프로젝트는 일반적으로 `.js` 대신 `.jsx` 확장자를 사용하며, 진입점 파일은 `index.js`가 아닌 `main.jsx` 입니다.

---

## Part 2: 나만의 자기소개 페이지 구현하기 (Mission!)

이제 여러분의 차례입니다! 아래 가이드라인을 참고하여 자신만의 자기소개 페이지를 자유롭게 만들어보세요. 정답은 없습니다. `day4`에서 배운 지식을 최대한 활용하여 창의적으로 구현해보세요.

### Mission 1: 공통 레이아웃 컴포넌트 제작

모든 페이지에서 공통으로 표시될 `Header`와 `Footer` 컴포넌트를 `src/components` 폴더 안에 만들어보세요.

*   **`Header.jsx` 컴포넌트**:
    *   웹사이트의 제목 (예: "OOO의 프로필")을 `<h1>` 태그로 보여주세요.
    *   '홈'과 '소개' 페이지로 이동할 수 있는 내비게이션 메뉴를 만드세요.
    *   내비게이션 링크는 `react-router-dom`의 `<Link>` 컴포넌트를 사용해야 합니다.
*   **`Footer.jsx` 컴포넌트**:
    *   간단한 저작권 정보 (예: `© 2025 OOO`)를 표시하세요.
*   **스타일링**:
    *   각 컴포넌트별로 `*.module.css` 파일을 만들어 CSS Modules 방식으로 스타일을 입혀보세요. (예: `Header.module.css`)

### Mission 2: 페이지 컴포넌트 제작

'홈' 페이지와 '소개' 페이지를 `src/pages` 폴더 안에 만들어보세요.

*   **`Home.jsx` (홈 페이지)**:
    *   웹사이트 방문자를 환영하는 간단한 메시지를 보여주세요.
    *   자유롭게 내용을 구성해보세요. (예: "저의 포트폴리오에 오신 것을 환영합니다!")
*   **`About.jsx` (소개 페이지)**:
    *   `day3` 실습에서 HTML로 만들었던 자기소개 페이지의 내용을 React 컴포넌트 형식으로 재구성해보세요.
    *   '나의 관심사', '나의 프로필 사진', '연락처' 등 여러 개의 `<section>`으로 나누어 정보를 제공하세요.
    *   이미지, 링크(`<a>`), 목록(`<ul>`, `<li>`) 등 다양한 태그를 활용하세요.
*   **스타일링**:
    *   마찬가지로 각 페이지 컴포넌트별로 CSS Modules를 사용하여 개성있는 스타일을 적용해보세요.

### Mission 3: 라우팅 설정 및 최종 조립

모든 조각들을 `App.jsx`에서 하나로 합치고, URL에 따라 적절한 페이지가 보이도록 라우팅을 설정합니다.

*   **`main.jsx` 설정**:
    *   `BrowserRouter`를 사용하여 `<App />` 전체를 감싸주세요.
*   **`App.jsx` 설정**:
    *   `Header`와 `Footer` 컴포넌트를 항상 표시되도록 배치하세요.
    *   `react-router-dom`의 `<Routes>`와 `<Route>`를 사용하여 URL 경로에 따라 `Home`과 `About` 컴포넌트가 표시되도록 설정하세요.
        *   `path="/"` 에서는 `<Home />` 컴포넌트가 보여야 합니다.
        *   `path="/about"` 에서는 `<About />` 컴포넌트가 보여야 합니다.

### Step 4: 개발 서버 실행 및 확인

모든 구현이 완료되었다고 생각되면, 터미널에서 다음 명령어를 실행하여 결과를 확인하세요.

```bash
npm run dev
```

웹 브라우저에서 터미널에 표시된 주소(보통 `http://localhost:5173`)로 접속하여 여러분이 만든 페이지를 확인합니다. '홈'과 '소개' 링크를 클릭하며 페이지 이동이 잘 되는지, 스타일이 의도한 대로 적용되었는지 꼼꼼히 살펴보세요. 오류가 발생한다면, 콘솔의 에러 메시지를 읽고 스스로 해결해보는 과정을 거치는 것이 중요합니다!

---

## 🚀 도전 과제 (Optional)

기본 미션을 완료했다면, 다음 기능들을 추가하여 자기소개 페이지를 더 발전시켜 보세요.

1.  **새로운 페이지 추가**: '나의 프로젝트'나 '블로그'와 같은 새로운 페이지 컴포넌트를 `pages` 폴더에 만들고, `App.jsx`에 라우팅 규칙을 추가해 보세요. `Header`에도 해당 페이지로 가는 `Link`를 추가하는 것을 잊지 마세요.
2.  **중첩 라우팅 (Nested Routing)**: '소개' 페이지 안에 '나의 이력'과 '나의 기술 스택'과 같은 하위 페이지를 만들고, `Outlet`을 사용하여 중첩된 UI를 구현해 보세요.
3.  **동적 라우팅 (Dynamic Routing)**: 여러 개의 프로젝트를 소개하는 페이지를 만들고, `projects/1`, `projects/2` 와 같이 URL 파라미터를 사용하여 각 프로젝트의 상세 내용을 보여주는 동적 라우팅을 구현해 보세요. (`useParams` 훅 사용)
4.  **다른 스타일링 기법 적용**: CSS Modules 대신 Styled-Components나 Tailwind CSS를 프로젝트에 설치하고, 일부 컴포넌트의 스타일을 새로운 방식으로 변경해보는 것은 어떨까요?

---

## ✅ 최종 결과물 예시

간단한 예제 코드입니다.

<details>
<summary><strong>최종 폴더 구조</strong></summary>

```
src/
├── components/
│   ├── Footer.jsx
│   ├── Footer.module.css
│   ├── Header.jsx
│   └── Header.module.css
├── pages/
│   ├── About.jsx
│   ├── About.module.css
│   ├── Home.jsx
│   └── Home.module.css
├── App.css
├── App.jsx
├── index.css
└── main.jsx
```

</details>

<details>
<summary><strong>각 파일 별 예시 코드</strong></summary>

#### `main.jsx`
```jsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import { BrowserRouter } from 'react-router-dom'
import App from './App.jsx'
import './index.css'

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </React.StrictMode>,
)
```

#### `App.jsx`
```jsx
import { Routes, Route } from 'react-router-dom';
import Header from './components/Header';
import Footer from './components/Footer';
import Home from './pages/Home';
import About from './pages/About';
import './App.css';

function App() {
  return (
    <div className="App">
      <Header />
      <main>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/about" element={<About />} />
        </Routes>
      </main>
      <Footer />
    </div>
  );
}

export default App;
```

#### `components/Header.jsx`
```jsx
import React from 'react';
import { Link } from 'react-router-dom';
import styles from './Header.module.css';

function Header() {
  return (
    <header className={styles.header}>
      <h1>나의 자기소개</h1>
      <nav>
        <ul>
          <li>
            <Link to="/">홈</Link>
          </li>
          <li>
            <Link to="/about">소개</Link>
          </li>
        </ul>
      </nav>
    </header>
  );
}

export default Header;
```

#### `components/Header.module.css`
```css
.header {
  background-color: #2c3e50;
  color: white;
  padding: 20px;
  text-align: center;
}

.header nav ul {
  list-style: none;
  padding: 0;
  display: flex;
  justify-content: center;
  gap: 20px;
}

.header nav a {
  color: white;
  text-decoration: none;
  font-weight: bold;
}

.header nav a:hover {
  text-decoration: underline;
}
```

#### `components/Footer.jsx`
```jsx
import React from 'react';
import styles from './Footer.module.css';

function Footer() {
  return (
    <footer className={styles.footer}>
      <p>&copy; 2025 OOO. 모든 권리 보유.</p>
    </footer>
  );
}

export default Footer;
```

#### `components/Footer.module.css`
```css
.footer {
  background-color: #34495e;
  color: white;
  text-align: center;
  padding: 15px 0;
  position: fixed;
  bottom: 0;
  width: 100%;
}
```

#### `pages/Home.jsx`
```jsx
import React from 'react';
import styles from './Home.module.css';

function Home() {
  return (
    <div className={styles.home}>
      <h2>웹사이트에 오신 것을 환영합니다!</h2>
      <p>이곳은 Vite와 React로 만든 저의 자기소개 웹사이트입니다.</p>
      <p>상단의 '소개' 메뉴를 클릭하여 더 많은 정보를 확인하세요.</p>
    </div>
  );
}

export default Home;
```

#### `pages/Home.module.css`
```css
.home {
  padding: 40px;
  text-align: center;
  background-color: #ecf0f1;
  border-radius: 8px;
  min-height: 60vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
```

#### `pages/About.jsx`
```jsx
import React from 'react';
import styles from './About.module.css';

function About() {
  return (
    <div className={styles.about}>
      <section>
        <h2>나의 관심사</h2>
        <ul>
          <li>Vite와 React 프론트엔드 개발</li>
          <li>JavaScript와 TypeScript</li>
          <li>깨끗한 코드 작성하기</li>
        </ul>
      </section>
      <section>
        <h2>나의 프로필 사진</h2>
        <img src="https://via.placeholder.com/150" alt="프로필" />
        <p>Vite와 함께하는 즐거운 코딩 라이프!</p>
      </section>
      <section>
        <h2>연락처</h2>
        <p>
          더 많은 정보를 원하시면{' '}
          <a href="https://github.com" target="_blank" rel="noopener noreferrer">
            나의 GitHub
          </a>
          를 방문해주세요.
        </p>
      </section>
    </div>
  );
}

export default About;
```

#### `pages/About.module.css`
```css
.about section {
  background-color: #ffffff;
  padding: 20px;
  margin-bottom: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.about h2 {
  color: #2980b9;
  margin-bottom: 15px;
}

.about img {
  max-width: 150px;
  border-radius: 50%;
  display: block;
  margin: 15px auto;
}

.about a {
  color: #2980b9;
  text-decoration: none;
}

.about a:hover {
  text-decoration: underline;
}
```

#### `index.css`
```css
:root {
  font-family: Inter, system-ui, Avenir, Helvetica, Arial, sans-serif;
  line-height: 1.5;
  font-weight: 400;

  color-scheme: light dark;
  color: rgba(255, 255, 255, 0.87);
  background-color: #242424;

  font-synthesis: none;
  text-rendering: optimizeLegibility;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

body {
  margin: 0;
  background-color: #f0f2f5;
  color: #333;
}

main {
  padding: 20px;
  /* Footer 높이만큼 아래쪽 여백을 주어 내용이 가려지지 않게 함 */
  padding-bottom: 80px; 
}
```

</details>

---
- [목차로 돌아가기](README.md)
- [이전 강의로 이동](04-React-Styling.md)
- [다음 강의로 이동](../README.md)
