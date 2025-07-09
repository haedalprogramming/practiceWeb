# 실습 1: TanStack Query로 API 연동 리팩토링

이전 `day5` 실습에서는 `useEffect`와 `useState`를 사용하여 데이터를 가져오고 로딩, 에러 상태를 직접 관리하는 블로그를 만들었습니다. 이 방식은 간단한 앱에서는 충분하지만, 애플리케이션이 복잡해질수록 상태 관리가 번거로워지고, 캐싱이나 데이터 동기화 같은 고급 기능을 구현하기 어렵습니다.

이번 실습에서는 `day5`에서 만든 블로그 프로젝트의 데이터 로직을 <strong>TanStack Query(React Query)</strong>로 리팩토링합니다. TanStack Query를 사용하면 단 몇 줄의 코드로 비동기 상태 관리를 선언적으로 처리할 수 있으며, 캐싱, 백그라운드 데이터 업데이트, 재시도 등 강력한 기능들을 손쉽게 활용할 수 있습니다.

---

## 1) 학습 목표

- TanStack Query의 핵심 `useQuery` Hook을 사용하여 서버 데이터를 가져오는 방법을 이해합니다.
- `isLoading`, `isError`, `data`, `error` 등 `useQuery`가 반환하는 상태를 활용하여 UI를 제어하는 방법을 배웁니다.
- 정적인 쿼리 키(`['posts']`)와 동적인 쿼리 키(`['post', id]`)의 사용법을 익힙니다.
- 기존 `useEffect`/`useState` 기반 데이터 로직을 TanStack Query로 전환함으로써 얻는 이점(코드 간결성, 캐싱 등)을 체감합니다.

## 2) 선수 지식

이 실습을 진행하기 전에, 아래의 `day5` 실습을 먼저 완료해야 합니다. 이번 실습은 해당 프로젝트 코드를 기반으로 진행됩니다.

- [실습: MSW로 가짜(Mock) API 서버 만들어 블로그 구현하기](../day5/Lab4-Blog-with-MSW.md)

---

## 3) 코드 구현

### 3-1) TanStack Query 설치

가장 먼저 `@tanstack/react-query` 라이브러리를 설치합니다.

```bash
npm install @tanstack/react-query
```

### 3-2) `QueryClientProvider` 설정

TanStack Query의 Hook을 사용하려면, 애플리케이션의 최상단 컴포넌트를 `QueryClientProvider`로 감싸주어야 합니다. 이렇게 하면 앱 전역에서 쿼리 클라이언트 인스턴스에 접근할 수 있게 됩니다.

**`src/index.js`** 파일을 다음과 같이 수정합니다.

```javascript
// ... 기존 import 구문들 ...
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';

// 새로운 QueryClient 인스턴스를 생성합니다.
const queryClient = new QueryClient();

async function enableMocking() {
  // ... (기존 MSW 설정 코드, 변경 없음)
}

const root = ReactDOM.createRoot(document.getElementById('root'));

enableMocking().then(() => {
  root.render(
    <React.StrictMode>
      {/* QueryClientProvider로 App 컴포넌트를 감싸줍니다. */}
      <QueryClientProvider client={queryClient}>
        <App />
      </QueryClientProvider>
    </React.StrictMode>
  );
});
```

> [!NOTE]
> `QueryClient`는 TanStack Query의 캐시를 관리하고 모든 쿼리 설정을 담당하는 핵심 클래스입니다. `QueryClientProvider`를 통해 이 클라이언트가 컴포넌트 트리에 제공됩니다.

### 3-3) `PostListPage` 리팩토링

이제 `useState`와 `useEffect`를 사용하던 `PostListPage` 컴포넌트를 `useQuery`로 리팩토링해 보겠습니다.

**`src/pages/PostListPage.js`** (리팩토링 후)
```javascript
import React from 'react';
import { Link } from 'react-router-dom';
import { useQuery } from '@tanstack/react-query';

// 데이터를 가져오는 비동기 함수
const fetchPosts = async () => {
  const response = await fetch('/posts');
  if (!response.ok) {
    throw new Error('서버에서 데이터를 가져오는 데 실패했습니다.');
  }
  return response.json();
};

function PostListPage() {
  // useQuery를 사용하여 데이터 로딩, 성공, 에러 상태를 한번에 관리합니다.
  const { 
    data: posts, 
    isLoading, 
    isError, 
    error 
  } = useQuery({
    queryKey: ['posts'], // 이 쿼리를 식별하는 고유한 키
    queryFn: fetchPosts, // 데이터를 가져오는 함수
  });

  if (isLoading) return <p>로딩 중...</p>;
  if (isError) return <p>에러: {error.message}</p>;

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

> [!TIP]
> **리팩토링 전과 비교해보세요!**
> 기존에는 `posts`, `loading`, `error`라는 3개의 `useState`와 이들을 관리하는 `useEffect` 로직이 필요했습니다. `useQuery`를 사용하니 이 모든 것이 단 하나의 Hook 호출로 대체되었습니다. 코드가 훨씬 간결하고 직관적으로 바뀌었습니다.

### 3-4) `PostDetailPage` 리팩토링

상세 페이지도 동일한 방식으로 리팩토링합니다. 여기서는 URL 파라미터 `id`가 변경될 때마다 새로운 데이터를 가져와야 하므로, 쿼리 키에 `id`를 포함시켜 동적으로 관리하는 것이 중요합니다.

**`src/pages/PostDetailPage.js`** (리팩토링 후)
```javascript
import React from 'react';
import { useParams, Link } from 'react-router-dom';
import { useQuery } from '@tanstack/react-query';

// id를 인자로 받아 특정 게시글을 가져오는 비동기 함수
const fetchPostById = async (id) => {
  const response = await fetch(`/posts/${id}`);
  if (!response.ok) {
    const errorData = await response.json().catch(() => ({})); // JSON 파싱 실패 대비
    throw new Error(errorData.errorMessage || '게시글을 불러오는 데 실패했습니다.');
  }
  return response.json();
};

function PostDetailPage() {
  const { id } = useParams();

  const { 
    data: post, 
    isLoading, 
    isError, 
    error 
  } = useQuery({
    // 쿼리 키에 id를 포함시켜, id가 다른 게시글은 별개의 쿼리로 캐싱되도록 합니다.
    queryKey: ['post', id], 
    // queryFn에 인자를 넘겨주어야 할 때는 아래와 같이 익명 함수로 감싸줍니다.
    queryFn: () => fetchPostById(id),
  });

  if (isLoading) return <p>로딩 중...</p>;
  if (isError) return <p>에러: {error.message}</p>;

  return (
    <div>
      <h2>{post.title}</h2>
      <p>{post.content}</p>
      <Link to="/">목록으로 돌아가기</Link>
    </div>
  );
}

export default PostDetailPage;
```

> [!IMPORTANT]
> TanStack Query는 `queryKey`를 기반으로 데이터를 캐싱합니다. `queryKey`가 `['post', '1']`인 데이터와 `['post', '2']`인 데이터는 서로 다른 데이터로 취급되어 각각 캐싱됩니다. 덕분에 사용자가 상세 페이지를 여러 번 오가더라도 이미 방문했던 페이지는 API 요청 없이 캐시에서 즉시 보여줄 수 있습니다.

---

## 4) 결론

이번 실습을 통해 `useState`와 `useEffect`로 구현했던 비동기 데이터 로직을 `TanStack Query`의 `useQuery` Hook으로 리팩토링했습니다. 복잡하고 장황했던 코드가 어떻게 간결하고 선언적인 코드로 바뀌었는지 확인할 수 있었습니다.

TanStack Query는 단순히 코드를 줄여주는 것을 넘어, 캐싱, 자동 재요청, 데이터 동기화 등 현대 웹 애플리케이션에 필수적인 기능들을 기본으로 제공하여 개발자가 비즈니스 로직에 더 집중할 수 있도록 도와줍니다.

---

[메인으로 돌아가기](../README.md)
[이전 강의로 돌아가기](./12-Web-Accessibility.md)
[다음 강의로 이동하기](./Lab2-Global-State-with-Zustand.md)
