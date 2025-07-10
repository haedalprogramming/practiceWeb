# 7) API 모킹 (Mock Service Worker)

실제 서비스 개발에서는 언제나 백엔드 서버가 완벽히 준비된 상태에서 프론트엔드를 개발할 수 있는 것은 아닙니다. 이런 상황에서 <strong>API 모킹(Mock API)</strong>은 큰 도움이 됩니다. 이번 강의에서는 대표적인 모킹 도구인 <strong>Mock Service Worker(MSW)</strong>를 사용하여 가짜 API를 만드는 방법을 배웁니다. 실제 백엔드 API를 준비해서 테스트하는 데에는 적지 않은 시간과 노력이 소모되므로, 프론트엔드에서의 백엔드 연동에 집중하기 위해 모킹을 사용하도록 합시다.

## 1) 왜 API 모킹이 필요한가?

### 1-1) 프론트엔드와 백엔드의 개발 속도 차이
- 백엔드 개발이 지연되면 프론트엔드 작업도 멈추게 됩니다.
- 모킹을 통해 <strong>가상의 응답</strong>을 만들어 두면, 실제 서버가 없어도 UI를 자유롭게 개발‧테스트할 수 있습니다.

### 1-2) 일관된 테스트 환경
- 외부 서버 상태(다운, 네트워크 지연 등)에 상관없이 **항상 동일한 데이터**를 받으므로 테스트가 안정적입니다.
- 오류 상황(404, 500 등)을 마음대로 만들 수 있어 <strong>예외 처리</strong>를 쉽게 점검할 수 있습니다.

> [!CAUTION]
> 백엔드가 배포된 후에도 모킹 데이터를 계속 사용하면 실제 서비스와 다른 동작을 할 수 있습니다. 배포 전에는 반드시 실제 API로 전환하세요.

## 2) API 모킹 방법 살펴보기

| 방법 | 특징 | 비고 |
|------|------|------|
| 하드코딩(JavaScript 파일) | 간단하지만 유지 보수 어려움 | 규모가 작은 프로젝트용 |
| <code>json-server</code> | JSON 파일을 REST API로 노출 | 별도 서버가 필요 │
| <strong>Mock Service Worker (MSW)</strong> | <strong>Service Worker</strong>가 네트워크 요청을 가로챔 | 브라우저‧테스트 환경 모두 지원 |

이 강의에서는 <strong>MSW</strong>를 사용합니다. 이유는:
1. **네트워크 계층**에서 동작해 실제 요청 흐름과 동일하게 테스트할 수 있습니다.
2. 환경(vite, CRA, Next.js 등)에 상관없이 쉽게 도입할 수 있습니다.
3. 브라우저뿐 아니라 <strong>Node 환경(Jest, Vitest)</strong> 테스트에서도 동일한 핸들러를 재사용할 수 있습니다.

## 3) Mock Service Worker(MSW)란?

### 3-1) 동작 원리
- 브라우저의 <strong>Service Worker</strong> API를 이용하여 네트워크 요청을 **가로채서**(intercept) 응답을 주입합니다.
- 실제 HTTP 요청이 네트워크로 나가기 전에 가짜 응답을 돌려주므로, <code>fetch</code> / <code>axios</code> 코드를 **수정할 필요가 없습니다.**

> [!TIP]
> Service Worker는 브라우저가 백그라운드에서 실행하는 스크립트입니다. 오프라인 캐싱, 푸시 알림 등을 구현할 때 사용합니다. 여기서는 "요청 가로채기" 기능만 활용합니다.

### 3-2) 주요 장점
1. <strong>실제 요청 로직 그대로</strong> 사용하므로 프론트엔드 코드 수정이 불필요합니다.
2. CORS, HTTPS 인증서 등 복잡한 설정을 따로 하지 않습니다.
3. 테스트 프레임워크(jest, vitest 등)와 통합이 간단합니다.

## 4) 프로젝트에 MSW 설치 및 초기화

### 4-1) 패키지 설치
```bash
npm install msw --save-dev
# 또는
pnpm add -D msw
```

### 4-2) 서비스 워커 파일 생성
```bash
npx msw init public/ --save
```
- <code>public/mockServiceWorker.js</code> 파일이 생성됩니다.
- 이 파일은 **절대 수정하지 않습니다.** MSW가 버전 업될 때마다 다시 생성하면 됩니다.

> [!NOTE]
> Vite 프로젝트에서는 <code>public/</code> 폴더가 정적 파일 루트입니다. CRA(Create React App)도 동일한 위치를 사용합니다.

### 4-3) 핸들러(Handler) 템플릿 만들기
프로젝트 루트에 <code>src/mocks/handlers.js</code>를 생성하고 다음과 같이 작성합니다.

```javascript
// src/mocks/handlers.js
import { rest } from 'msw';

export const handlers = [
  // GET /todos 요청을 가로채서 가짜 할 일 목록을 반환
  rest.get('/todos', (req, res, ctx) => {
    return res(
      ctx.status(200),
      ctx.json([
        { id: 1, title: 'React 공부하기', completed: false },
        { id: 2, title: '운동하기', completed: true },
      ])
    );
  }),

  // POST /todos 요청 처리 (새 할 일 추가)
  rest.post('/todos', async (req, res, ctx) => {
    const newTodo = await req.json();
    return res(
      ctx.status(201),
      ctx.json({ ...newTodo, id: Date.now() })
    );
  }),
];
```

### 4-4) 브라우저에서 서비스 워커 시작
<code>src/mocks/browser.js</code> 파일을 만들고 다음 코드를 작성합니다.

```javascript
// src/mocks/browser.js
import { setupWorker } from 'msw';
import { handlers } from './handlers';

// Service Worker + 핸들러 조합
export const worker = setupWorker(...handlers);
```

### 4-5) 애플리케이션 진입점에 워커 등록
`main.jsx`(또는 `index.tsx`) 최상단에 다음 코드를 추가합니다.

```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';

// 개발 환경에서만 MSW 실행
if (process.env.NODE_ENV === 'development') {
  const { worker } = await import('./mocks/browser');
  await worker.start();
}

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

> [!WARNING]
> `await import` 구문을 사용하려면 상위 함수가 <code>async</code>여야 합니다. 불가능하다면 <code>worker.start()</code> 호출 후 <code>.then()</code> 체인을 이용하세요.

## 5) 요청 흐름 살펴보기

```mermaid
graph TD
  A[컴포넌트 useEffect] -->|fetch('/todos')| B(브라우저 네트워크 계층)
  B -->|Service Worker| C{MSW 핸들러 매칭}
  C -->|매치 O| D[가짜 JSON 응답]
  C -->|매치 X| E[실제 서버 요청]
```

- **1단계**: 컴포넌트가 <code>fetch('/todos')</code>를 호출합니다.
- **2단계**: Service Worker가 요청을 가로채고 <code>handlers</code> 배열에서 일치하는 엔드포인트를 찾습니다.
- **3단계**: 핸들러가 존재하면 <strong>가짜 응답</strong>을 반환하고, 없으면 실제 네트워크로 요청이 전송됩니다.

## 6) 예제: Todo 리스트 컴포넌트

아래 예제는 앞서 5일차 과제로 만들었던 <code>UserList</code> 대신 <code>TodoList</code>를 표시합니다.

```jsx
// src/components/TodoList.jsx
import React, { useEffect, useState } from 'react';

function TodoList() {
  const [todos, setTodos] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    async function fetchTodos() {
      const res = await fetch('/todos'); // 실제 서버 주소가 아님!
      const data = await res.json();
      setTodos(data);
      setLoading(false);
    }
    fetchTodos();
  }, []);

  if (loading) return <p>로딩 중...</p>;

  return (
    <ul>
      {todos.map((todo) => (
        <li key={todo.id}>
          {todo.title} {todo.completed ? '✅' : '⬜'}
        </li>
      ))}
    </ul>
  );
}

export default TodoList;
```

페이지를 실행하면 실제 서버가 없어도 Todo 목록이 화면에 나타납니다.

## 7) 테스트 환경에서의 MSW

- Jest/Vitest 테스트 파일에서는 <code>setupServer</code> API를 사용합니다.
- <code>handlers</code>는 브라우저와 **공통**으로 재사용 가능합니다.

```javascript
// src/mocks/server.js
import { setupServer } from 'msw/node';
import { handlers } from './handlers';

export const server = setupServer(...handlers);
```

테스트 파일 예시:
```javascript
import { server } from '../mocks/server';
import { rest } from 'msw';

beforeAll(() => server.listen());
afterEach(() => server.resetHandlers());
afterAll(() => server.close());
```

> [!IMPORTANT]
> 테스트에서도 **동일한 핸들러**를 사용하므로, 브라우저와 테스트 간에 데이터 정의를 한 곳에서 관리할 수 있습니다.

## 8) 모킹 데이터 관리 전략

1. <strong>fixtures</strong> 폴더에 JSON 파일로 분리해두면 재사용과 버전 관리를 쉽게 할 수 있습니다.
2. 실제 API 스펙 변화에 따라 모킹 데이터도 **주기적으로 업데이트**해야 합니다.
3. "해피 케이스"뿐 아니라 4xx, 5xx 오류 응답도 미리 정의해 두면 예외 처리 테스트에 유리합니다.

## 9) 실제 API로 전환하기

- 개발 막바지 또는 백엔드가 준비되면 <strong>`NODE_ENV`</strong>를 <code>production</code>으로 변경해 MSW를 비활성화합니다.
- 또는 <code>VITE_USE_MSW=false</code> 같은 환경 변수를 만들어 조건부로 워커를 실행합니다.

```javascript
if (import.meta.env.DEV && import.meta.env.VITE_USE_MSW === 'true') {
  const { worker } = await import('./mocks/browser');
  await worker.start();
}
```

## 10) 정리

*   <strong>API 모킹</strong>은 백엔드 의존성을 제거해 개발 속도와 테스트 안정성을 높여 줍니다.
*   Mock Service Worker(MSW)는 **Service Worker**를 이용해 네트워크 수준에서 요청을 가로채므로, 애플리케이션 코드를 수정할 필요가 없습니다.
*   브라우저와 Node 테스트 환경 모두에서 **동일한 핸들러**를 재사용할 수 있어 유지 보수가 쉽습니다.
*   실제 서버가 준비되면 환경 변수 또는 빌드 모드로 MSW를 비활성화하여 실제 API로 전환합니다.

---
- [목차로 돌아가기](./README.md)
- [이전 강의로 이동](./06-REST-API.md)
- [다음 강의로 이동](Lab1-Mock-API-and-Fetch.md)
