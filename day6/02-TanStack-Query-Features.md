# 2) TanStack Query 주요 기능 활용

이전 강의에서는 `useQuery`를 사용하여 서버에서 데이터를 가져오고, 로딩 및 에러 상태를 처리하는 기본적인 방법을 배웠습니다. 이번 강의에서는 TanStack Query를 더욱 강력하게 만들어주는 핵심 기능인 **캐싱(Caching)**, **데이터 동기화 옵션**, 그리고 서버 데이터를 변경하는 **Mutation**에 대해 자세히 알아보겠습니다.

## 2-1) TanStack Query의 캐싱과 데이터 동기화

TanStack Query의 가장 큰 장점 중 하나는 바로 **자동 캐싱** 기능입니다.

<strong>캐시(Cache)</strong>란, 한 번 가져온 데이터를 나중에 다시 사용하기 위해 임시로 저장해두는 장소를 의미합니다. TanStack Query는 `useQuery`를 통해 가져온 데이터를 메모리 캐시에 저장합니다. 그리고 동일한 `queryKey`로 다시 데이터를 요청할 경우, 네트워크를 통해 서버에 또 요청하는 대신 캐시에 저장된 데이터를 먼저 보여줍니다.

이 과정 덕분에 사용자 경험이 크게 향상됩니다. 예를 들어, '게시물 목록' 페이지를 봤다가 다른 페이지로 이동한 후 다시 '게시물 목록'으로 돌아왔을 때, 화면이 즉시 나타나는 것을 볼 수 있습니다. 이는 TanStack Query가 캐시된 데이터를 먼저 보여주고, 그 사이에 백그라운드에서 최신 데이터가 있는지 확인하여 자동으로 업데이트해주기 때문입니다.

이러한 캐싱 및 데이터 동기화 동작은 두 가지 중요한 옵션인 `staleTime`과 `gcTime` (Garbage Collection Time)을 통해 제어할 수 있습니다.

### 2-1-1) <strong>`staleTime`</strong>: 데이터가 '신선함'을 유지하는 시간

`staleTime`은 **데이터가 '신선한(fresh)' 상태로 간주되는 시간**을 의미합니다.

*   `useQuery`로 데이터를 성공적으로 가져온 후, `staleTime`이 지나기 전까지는 해당 데이터가 '신선하다'고 판단합니다.
*   **신선한 데이터는 절대 리페치(refetch)되지 않습니다.** 컴포넌트가 다시 렌더링되거나, 사용자가 브라우저 창을 떠났다가 다시 돌아와도 네트워크 요청을 다시 보내지 않습니다.
*   `staleTime`의 기본값은 **0**입니다. 즉, 데이터를 가져오는 즉시 '오래된(stale)' 데이터로 간주됩니다.
*   오래된(stale) 데이터는 특정 조건(창 포커스, 컴포넌트 재마운트 등)이 만족될 때 **백그라운드에서 자동으로 리페치**됩니다. 리페치가 진행되는 동안에는 캐시된 데이터를 화면에 계속 보여줍니다.

**예시: 5분 동안 데이터를 신선하게 유지하기**

만약 자주 바뀌지 않는 데이터(예: 카테고리 목록)가 있다면 `staleTime`을 길게 설정하여 불필요한 네트워크 요청을 줄일 수 있습니다.

```jsx
import { useQuery } from '@tanstack/react-query';

const fetchCategories = async () => {
  // ... 카테고리 목록을 가져오는 API 호출
};

function Categories() {
  const { data } = useQuery({
    queryKey: ['categories'],
    queryFn: fetchCategories,
    staleTime: 1000 * 60 * 5, // 5분 (밀리초 단위)
  });

  // 이 컴포넌트는 5분 안에 다시 렌더링되어도
  // 네트워크 요청을 다시 보내지 않습니다.
  return (
    // ...
  );
}
```

> [!NOTE]
> *   `staleTime: 0` (기본값): 데이터를 항상 최신으로 유지하고 싶을 때 사용합니다. 캐시된 데이터를 즉시 보여주되, 백그라운드에서 항상 최신 데이터를 확인합니다.
> *   `staleTime: Infinity`: 데이터를 절대 자동으로 리페치하지 않으려면 `Infinity`로 설정할 수 있습니다. 수동으로만 데이터를 업데이트할 때 유용합니다.

### 2-1-2) <strong>`gcTime`</strong> (Garbage Collection Time): 캐시가 메모리에서 유지되는 시간

`gcTime`은 **데이터가 '비활성(inactive)' 상태가 된 후, 캐시에서 제거되기까지의 시간**을 의미합니다. 여기서 '비활성'이란 해당 데이터를 사용하는 `useQuery`가 하나도 없을 때를 말합니다.

*   `gcTime`은 `staleTime`과 관계없이 동작합니다. 데이터가 `stale` 상태이더라도, 화면에서 사용되고 있다면 `active` 상태입니다.
*   페이지 이동 등으로 인해 특정 `queryKey`를 사용하는 모든 컴포넌트가 언마운트되면, 해당 쿼리는 `inactive` 상태로 전환되고 타이머가 시작됩니다.
*   `gcTime`이 지나기 전에 동일한 `queryKey`를 사용하는 컴포넌트가 다시 마운트되면, 캐시된 데이터를 즉시 재사용합니다.
*   `gcTime`이 지나면, 해당 쿼리 데이터는 메모리에서 완전히 삭제(Garbage Collection)됩니다.
*   `gcTime`의 기본값은 **5분**입니다.

**요약:**

*   `staleTime`: "이 데이터, 최신 데이터로 봐도 될까?" (리페치 빈도 결정)
*   `gcTime`: "이 데이터, 이제 아무도 안 쓰는데 계속 메모리에 들고 있을까?" (캐시 데이터 삭제 시점 결정)

> [!IMPORTANT]
> 일반적으로 `gcTime`은 `staleTime`보다 길게 설정합니다. 만약 `gcTime`이 `staleTime`보다 짧다면, 데이터가 `stale` 상태가 되기도 전에 캐시에서 삭제될 수 있어 캐싱의 이점을 제대로 누리지 못할 수 있습니다. (`gcTime` > `staleTime`)

## 2-2) 서버 데이터 변경: `useMutation`

지금까지는 `useQuery`를 통해 서버 데이터를 <strong>읽어오는(Read)</strong> 작업만 다뤘습니다. 하지만 실제 애플리케이션에서는 데이터를 <strong>생성(Create)하거나, 수정(Update)하거나, 삭제(Delete)</strong>해야 할 때가 많습니다. 이러한 <strong>데이터 변경 작업</strong>을 처리하기 위해 TanStack Query는 `useMutation`이라는 Hook을 제공합니다.

`useMutation`은 `useQuery`와 달리 데이터를 캐싱하지 않습니다. 대신, 데이터 변경 작업을 수행하는 과정에서 발생하는 상태(로딩, 에러, 성공 등)를 손쉽게 관리할 수 있도록 도와줍니다.

### 2-2-1) `useMutation`의 기본 사용법

`useMutation`은 주로 하나의 인자, 즉 **mutation 함수**를 받습니다.

*   **Mutation Function (변경 함수)**:
    *   서버 데이터를 변경하는 비동기 함수입니다. `POST`, `PUT`, `DELETE` 등의 HTTP 요청을 보내는 로직이 여기에 들어갑니다.
    *   이 함수는 보통 변경에 필요한 <strong>변수(variables)</strong>를 인자로 받습니다. 예를 들어, 새로운 게시물을 생성한다면 게시물의 제목과 내용을 인자로 받을 수 있습니다.

`useMutation`은 `mutate` 함수와 다양한 상태 값을 반환합니다.

*   `mutate`: 데이터 변경을 실제로 실행하는 함수입니다. 이 함수에 변경에 필요한 변수를 전달할 수 있습니다.
*   `isPending`: Mutation이 현재 실행 중일 때 `true`가 됩니다. (`useQuery`의 `isLoading`과 유사)
*   `isError`, `error`: Mutation 실행 중 에러가 발생했을 때의 상태와 에러 객체입니다.
*   `isSuccess`: Mutation이 성공적으로 완료되었을 때 `true`가 됩니다.

### 2-2-2) `useMutation` 예제: 새 게시물 추가하기

새로운 게시물을 생성하는 폼을 만든다고 가정해봅시다.

```jsx
import React, { useState } from 'react';
import { useMutation } from '@tanstack/react-query';

// 새로운 게시물을 생성하는 비동기 함수 (Mutation Function)
const createPost = async (newPost) => {
  const response = await fetch('https://jsonplaceholder.typicode.com/posts', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(newPost),
  });
  if (!response.ok) {
    throw new Error('게시물을 생성하는 데 실패했습니다.');
  }
  return response.json();
};

function CreatePostForm() {
  const [title, setTitle] = useState('');
  const [body, setBody] = useState('');

  const { mutate, isPending, isError, error } = useMutation({
    mutationFn: createPost
  });

  const handleSubmit = (e) => {
    e.preventDefault();
    // mutate 함수를 호출하여 데이터 변경 실행
    mutate({ title, body });
  };

  return (
    <form onSubmit={handleSubmit}>
      {isError && <p>에러: {error.message}</p>}
      
      <input
        type="text"
        value={title}
        onChange={(e) => setTitle(e.target.value)}
        placeholder="제목"
        disabled={isPending}
      />
      <textarea
        value={body}
        onChange={(e) => setBody(e.target.value)}
        placeholder="내용"
        disabled={isPending}
      />
      
      <button type="submit" disabled={isPending}>
        {isPending ? '생성 중...' : '게시물 생성'}
      </button>
    </form>
  );
}
```

이 예제에서 `mutate({ title, body })`가 호출되면, `createPost` 함수가 `{ title, body }` 객체를 인자로 받아 실행됩니다. `useMutation`은 이 비동기 작업이 진행되는 동안 `isPending` 상태를 `true`로 만들어 버튼과 입력을 비활성화하는 등 UI를 적절하게 제어할 수 있도록 도와줍니다.

### 2-2-3) Mutation 성공 후 `Query` 무효화하기

게시물을 성공적으로 생성했다면, '게시물 목록'을 보여주는 `useQuery(['posts'])`는 어떻게 최신 데이터를 반영할 수 있을까요? 사용자가 직접 새로고침해야 할까요?

`useMutation`의 콜백 옵션을 사용하면 이 문제를 아주 간단하게 해결할 수 있습니다. 가장 일반적인 패턴은 Mutation이 성공했을 때 관련된 `Query`를 <strong>무효화(invalidate)</strong>시키는 것입니다.

<strong>쿼리 무효화(Query Invalidation)</strong>란, 특정 `queryKey`를 가진 데이터를 '오래된(stale)' 상태로 강제로 만드는 것을 의미합니다. 해당 데이터가 현재 화면에 보이고 있다면, 즉시 리페치됩니다.

`onSuccess` 콜백과 `QueryClient`를 사용해 이 기능을 구현할 수 있습니다.

```jsx
import { useMutation, useQueryClient } from '@tanstack/react-query';

// ... (createPost 함수는 동일)

function CreatePostForm() {
  // ... (useState, handleSubmit 등은 동일)
  
  // 1. QueryClient 인스턴스 가져오기
  const queryClient = useQueryClient();

  const { mutate, isPending } = useMutation({
    mutationFn: createPost,
    // 2. Mutation 성공 시 실행될 콜백
    onSuccess: () => {
      // 3. 'posts' 쿼리를 무효화하여 리페치 유도
      console.log('게시물 생성 성공! "posts" 쿼리를 무효화합니다.');
      queryClient.invalidateQueries({ queryKey: ['posts'] });
    },
    // 에러 발생 시 콜백
    onError: (error) => {
      console.error('게시물 생성 실패:', error);
    },
    // 성공/실패 여부와 관계없이 항상 실행
    onSettled: () => {
      console.log('Mutation이 종료되었습니다.');
    }
  });

  // ... (JSX는 동일)
}
```

이제 사용자가 새 게시물을 성공적으로 생성하면, `onSuccess` 콜백이 실행됩니다. `queryClient.invalidateQueries({ queryKey: ['posts'] })`는 `['posts']`라는 키를 가진 모든 쿼리를 무효화시킵니다. 만약 다른 곳에서 `useQuery({ queryKey: ['posts'], ... })`를 통해 게시물 목록을 보여주고 있었다면, 그 쿼리는 즉시 최신 데이터를 다시 가져와 화면을 자동으로 업데이트하게 됩니다.

이처럼 `useMutation`과 쿼리 무효화 패턴을 사용하면, 서버 데이터 변경과 UI 동기화를 매우 선언적이고 효율적으로 관리할 수 있습니다.

---

- [목차로 돌아가기](./README.md)
- [이전 강의로 이동](./01-TanStack-Query.md)
- [다음 강의로 이동](./03-State-Management-Libs.md)
