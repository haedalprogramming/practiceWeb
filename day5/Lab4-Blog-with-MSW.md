# 실습 4: MSW로 가짜(Mock) API 서버 만들어 블로그 구현하기

프론트엔드 개발을 할 때 백엔드 API가 아직 준비되지 않은 경우가 많습니다. 이럴 때 개발을 멈추고 기다리는 대신, **API 모킹(Mocking)**을 통해 실제 서버가 있는 것처럼 개발을 진행할 수 있습니다. MSW(Mock Service Worker)는 서비스 워커를 이용하여 네트워크 요청을 가로채고, 우리가 정의한 가짜 응답을 내려주는 강력한 라이브러리입니다.

이번 실습에서는 MSW를 사용하여 블로그 게시글 목록과 상세 내용을 제공하는 가짜 API를 만들고, `React Router`와 `fetch` API를 결합하여 간단한 블로그 애플리케이션을 완성해 보겠습니다.

---

## 1) 학습 목표

- MSW를 사용하여 REST API를 모킹하는 방법을 이해합니다.
- `useEffect` 내에서 `fetch`를 사용하여 비동기 데이터를 가져오는 흐름을 완벽히 숙지합니다.
- 로딩, 성공, 에러와 같은 비동기 요청의 3가지 상태를 관리하는 방법을 배웁니다.
- `React Router`의 `useParams`를 사용하여 URL 파라미터 값을 가져오고, 이를 API 요청에 활용하는 방법을 복습합니다.
- 실제 백엔드 없이도 완전한 기능을 갖춘 프론트엔드 애플리케이션을 개발하는 전체 과정을 경험합니다.

---

## 2) 실습 요구사항

1.  **MSW 설치 및 설정**:
    - `msw` 라이브러리를 개발 의존성으로 설치합니다.
    - `npx msw init src/ --save` 명령어로 서비스 워커 스크립트를 생성합니다.
    - `src/mocks` 폴더를 만들고 `handlers.js`와 `browser.js` 파일을 설정합니다.
2.  **API 핸들러 정의**:
    - `GET /posts`: 게시글 전체 목록을 배열 형태로 반환하는 핸들러를 작성합니다.
    - `GET /posts/:id`: 특정 `id`에 해당하는 게시글 객체를 반환하는 핸들러를 작성합니다. 존재하지 않는 `id`일 경우 404 에러를 반환합니다.
3.  **React Router 설정**:
    - `react-router-dom`을 설치합니다.
    - `/` 경로에는 `PostListPage` 컴포넌트를, `/posts/:id` 경로에는 `PostDetailPage` 컴포넌트를 렌더링하도록 라우터를 설정합니다.
4.  **컴포넌트 구현**:
    - **`PostListPage.js`**:
        - 컴포넌트 마운트 시 `useEffect`를 사용하여 `/posts` API를 호출합니다.
        - 데이터를 불러오는 동안 "로딩 중..." 메시지를 표시합니다.
        - 데이터 로딩 성공 시, 게시글 목록을 화면에 렌더링합니다. 각 게시글 제목은 상세 페이지로 이동하는 링크(`Link` 컴포넌트)여야 합니다.
        - API 요청 실패 시 에러 메시지를 표시합니다.
    - **`PostDetailPage.js`**:
        - `useParams` Hook을 사용하여 URL의 `:id` 값을 가져옵니다.
        - `useEffect`에서 해당 `id`를 사용하여 `/posts/:id` API를 호출합니다.
        - 로딩, 성공, 에러 상태를 관리하여 화면에 적절한 UI를 보여줍니다.

---

## 3) 코드 구현

### 3-1) 라이브러리 설치

```bash
# MSW와 React Router 설치
npm install msw react-router-dom --save-dev
```

> [!WARNING]
> MSW는 개발 중에만 사용하는 도구이므로 `--save-dev` 옵션으로 설치하는 것이 일반적입니다. 하지만 이 프로젝트에서는 빌드 후에도 모킹이 동작하는 것을 보여주기 위해 일반 의존성으로 설치할 수도 있습니다. `npm install msw react-router-dom`

### 3-2) MSW 설정

1.  **서비스 워커 스크립트 생성**
    ```bash
    npx msw init public/ --save
    ```
    이 명령은 `public` 폴더에 `mockServiceWorker.js` 파일을 생성합니다. 이 파일은 수정할 필요 없습니다.

2.  **Mock 핸들러 및 서버 설정**

    `src` 폴더에 `mocks` 디렉토리를 생성합니다.

    **`src/mocks/handlers.js`**
    ```javascript
    import { rest } from 'msw';

    const posts = [
      { id: '1', title: '첫 번째 블로그 글', content: 'useEffect와 fetch로 데이터를 가져옵니다.' },
      { id: '2', title: 'React Router 사용법', content: 'useParams로 URL 파라미터를 읽어봅시다.' },
      { id: '3', title: 'MSW는 편리해요', content: '백엔드 없이 프론트엔드 개발을 할 수 있습니다.' },
    ];

    export const handlers = [
      // GET /posts 요청을 처리하는 핸들러
      rest.get('/posts', (req, res, ctx) => {
        return res(
          ctx.status(200),
          ctx.json(posts)
        );
      }),

      // GET /posts/:id 요청을 처리하는 핸들러
      rest.get('/posts/:id', (req, res, ctx) => {
        const { id } = req.params;
        const post = posts.find((p) => p.id === id);

        if (post) {
          return res(
            ctx.status(200),
            ctx.json(post)
          );
        } else {
          return res(
            ctx.status(404),
            ctx.json({ errorMessage: '게시글을 찾을 수 없습니다.' })
          );
        }
      }),
    ];
    ```

    **`src/mocks/browser.js`**
    ```javascript
    import { setupWorker } from 'msw';
    import { handlers } from './handlers';

    // 모든 핸들러를 사용하여 워커를 설정합니다.
    export const worker = setupWorker(...handlers);
    ```

3.  **애플리케이션에 MSW 적용**

    **`src/index.js`** 파일을 수정하여 애플리케이션 시작 시 MSW를 활성화합니다.

    ```javascript
    import React from 'react';
    import ReactDOM from 'react-dom/client';
    import './index.css';
    import App from './App';

    async function enableMocking() {
      // 개발 환경에서만 MSW를 활성화합니다.
      if (process.env.NODE_ENV !== 'development') {
        return;
      }

      const { worker } = await import('./mocks/browser');

      // 서비스 워커를 시작합니다.
      return worker.start();
    }

    const root = ReactDOM.createRoot(document.getElementById('root'));

    enableMocking().then(() => {
      root.render(
        <React.StrictMode>
          <App />
        </React.StrictMode>
      );
    });
    ```

### 3-3) 라우터 및 페이지 컴포넌트 구현

**`App.js`** (라우터 설정)
```javascript
import React from 'react';
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';
import PostListPage from './pages/PostListPage';
import PostDetailPage from './pages/PostDetailPage';
import './App.css';

function App() {
  return (
    <Router>
      <div className="App">
        <header className="App-header">
          <h1>MSW 블로그</h1>
        </header>
        <main>
          <Routes>
            <Route path="/" element={<PostListPage />} />
            <Route path="/posts/:id" element={<PostDetailPage />} />
          </Routes>
        </main>
      </div>
    </Router>
  );
}

export default App;
```

`src` 폴더에 `pages` 디렉토리를 만들고, 그 안에 페이지 컴포넌트들을 작성합니다.

**`src/pages/PostListPage.js`**
```javascript
import React, { useState, useEffect } from 'react';
import { Link } from 'react-router-dom';

function PostListPage() {
  const [posts, setPosts] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchPosts = async () => {
      try {
        const response = await fetch('/posts');
        if (!response.ok) {
          throw new Error('서버에서 데이터를 가져오는 데 실패했습니다.');
        }
        const data = await response.json();
        setPosts(data);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };

    fetchPosts();
  }, []);

  if (loading) return <p>로딩 중...</p>;
  if (error) return <p>에러: {error}</p>;

  return (
    <div>
      <h2>블로그 글 목록</h2>
      <ul>
        {posts.map((post) => (
          <li key={post.id}>
            <Link to={`/posts/${post.id}`}>{post.title}</Link>
          </li>
        ))}
      </ul>
    </div>
  );
}

export default PostListPage;
```

**`src/pages/PostDetailPage.js`**
```javascript
import React, { useState, useEffect } from 'react';
import { useParams, Link } from 'react-router-dom';

function PostDetailPage() {
  const { id } = useParams();
  const [post, setPost] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchPost = async () => {
      try {
        const response = await fetch(`/posts/${id}`);
        if (!response.ok) {
          const errorData = await response.json();
          throw new Error(errorData.errorMessage || '게시글을 불러오는 데 실패했습니다.');
        }
        const data = await response.json();
        setPost(data);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };

    fetchPost();
  }, [id]);

  if (loading) return <p>로딩 중...</p>;
  if (error) return <p>에러: {error}</p>;
  if (!post) return <p>게시글을 찾을 수 없습니다.</p>;

  return (
    <div>
      <Link to="/">&larr; 목록으로 돌아가기</Link>
      <article style={{ marginTop: '20px' }}>
        <h2>{post.title}</h2>
        <p>{post.content}</p>
      </article>
    </div>
  );
}

export default PostDetailPage;
```

---

## 4) 실행 결과 확인

1.  애플리케이션을 다시 시작(`npm start`)합니다. 브라우저 개발자 도구의 콘솔에 `[MSW] Mocking enabled.` 메시지가 나타나는지 확인합니다.
2.  메인 페이지(`http://localhost:3000/`)에 접속하면 "로딩 중..." 메시지가 잠시 보인 후, `handlers.js`에 정의한 3개의 블로그 글 제목이 목록으로 나타납니다.
3.  목록에서 아무 글 제목이나 클릭하면 해당 글의 상세 페이지(예: `/posts/1`)로 이동합니다.
4.  상세 페이지에는 글의 제목과 내용이 표시되고, 목록으로 돌아가는 링크가 보입니다.
5.  주소창에 존재하지 않는 ID(예: `/posts/99`)를 입력하면, "에러: 게시글을 찾을 수 없습니다."라는 메시지가 표시됩니다.
6.  개발자 도구의 `Network` 탭을 확인하면, 실제로 네트워크 요청이 외부로 나가지 않고 서비스 워커에 의해 가로채져 처리되는 것을 볼 수 있습니다.

---

## 5) 정리

이번 실습을 통해 우리는 MSW를 사용하여 실제와 거의 동일한 개발 환경을 구축하고, 비동기 데이터 통신을 포함한 복합적인 기능을 가진 애플리케이션을 만드는 전 과정을 경험했습니다. API 모킹은 프론트엔드와 백엔드 개발팀 간의 의존성을 낮춰 개발 생산성을 크게 향상시키는 핵심 기술 중 하나입니다. 이제 어떤 프로젝트에서든 API 명세만 있다면 백엔드 개발을 기다리지 않고 프론트엔드 개발을 자신있게 시작할 수 있습니다.

---

- [목차로 돌아가기](../README.md)
- [이전 실습으로 돌아가기](./Lab3-Timer-with-useRef.md) 