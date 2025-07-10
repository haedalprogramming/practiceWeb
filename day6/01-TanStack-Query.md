# 1) TanStack Query (React Query) 소개

Day 5에서 우리는 `fetch` API와 `useEffect` Hook을 사용하여 서버로부터 데이터를 가져오는 방법을 배웠습니다. 이 방법은 간단한 데이터를 한두 번 가져올 때는 유용하지만, 실제 애플리케이션에서는 다음과 같은 여러 가지 복잡한 상황들을 마주하게 됩니다.

*   **로딩 상태 관리**: 데이터를 가져오는 동안 사용자에게 로딩 중임을 알려줘야 합니다.
*   **에러 처리**: 네트워크 오류나 서버 문제 발생 시, 사용자에게 에러 상황을 알려주고 재시도할 수 있는 기능을 제공해야 합니다.
*   **캐싱(Caching)**: 동일한 데이터를 여러 번 요청할 경우, 불필요한 네트워크 요청을 줄이기 위해 이전에 받아온 데이터를 잠시 저장해두고 재사용해야 합니다.
*   **데이터 동기화**: 사용자가 앱을 사용하지 않다가 다시 돌아왔을 때, 화면에 보이는 데이터가 최신 데이터인지 확인하고, 아니라면 새로운 데이터를 가져와 업데이트해야 합니다.
*   **페이지네이션, 무한 스크롤**: 한 번에 수많은 데이터를 가져오는 대신, 여러 페이지로 나누어 보여주거나 사용자가 스크롤할 때마다 새로운 데이터를 불러와야 합니다.

이 모든 것을 `useState`와 `useEffect`만으로 직접 구현하는 것은 매우 번거롭고 반복적인 코드를 많이 만들어냅니다. 코드 구조는 복잡해지고, 상태 관리는 어려워지며, 예상치 못한 버그가 발생할 가능성도 커집니다.

이러한 **서버 상태 관리**의 어려움을 해결하기 위해 등장한 라이브러리가 바로 **TanStack Query (구 React Query)**입니다.

> [!NOTE]
> **서버 상태(Server State) vs 클라이언트 상태(Client State)**
>
> React 애플리케이션의 상태는 크게 두 가지로 나눌 수 있습니다.
>
> *   **클라이언트 상태(Client State)**: 클라이언트(브라우저)에서만 관리되며, UI와 직접적으로 관련된 상태입니다. 예를 들어, 다크 모드 여부, 모달 창 열림/닫힘 상태, 폼 입력 값 등이 해당됩니다. 이 상태는 오직 클라이언트만이 소유하고 제어합니다.
> *   **서버 상태(Server State)**: 서버 데이터베이스에 저장되어 있으며, 클라이언트가 직접 소유하거나 제어하지 않는 상태입니다. 우리는 API를 통해 이 데이터를 가져오거나 업데이트할 뿐입니다. 서버 상태는 비동기적으로 가져와야 하고, 다른 사용자에 의해 언제든지 변경될 수 있으며, 우리가 모르는 사이에 '오래된(stale)' 데이터가 될 수 있습니다.
>
> `useState`, `useReducer`, `Context API` 등은 주로 **클라이언트 상태**를 관리하는 데 적합합니다. 반면, **TanStack Query**는 **서버 상태**를 관리하는 데 특화된 강력한 도구입니다.

## 1-1) TanStack Query란?

**TanStack Query**는 React 애플리케이션에서 **서버 상태를 가져오고, 캐싱하고, 동기화하고, 업데이트하는 작업을 쉽게 만들어주는 데이터 페칭(Data-Fetching) 및 상태 관리 라이브러리**입니다.

복잡한 `useEffect`와 `useState` 조합 대신, TanStack Query가 제공하는 몇 가지 Hook을 사용하면 위에서 언급했던 로딩, 에러 처리, 캐싱 등의 문제를 매우 선언적이고 간결한 코드로 해결할 수 있습니다.

### 1-2) 왜 TanStack Query를 사용해야 할까?

*   **보일러플레이트 코드 감소**: `useState`로 로딩, 데이터, 에러 상태를 각각 만들고, `useEffect` 안에서 `fetch`를 호출하고, `try...catch`로 에러를 잡는 등의 반복적인 코드를 획기적으로 줄여줍니다.
*   **자동 캐싱**: 한 번 가져온 데이터는 메모리에 캐싱되어, 동일한 데이터를 다시 요청할 때 네트워크 요청 없이 캐시된 데이터를 즉시 반환합니다. 이를 통해 사용자 경험을 개선하고 불필요한 API 호출을 줄일 수 있습니다.
*   **자동 리페칭(Refetching)**: 사용자가 다른 창을 봤다가 다시 돌아오거나, 네트워크가 재연결되는 등의 특정 상황에서 데이터가 오래되었을 가능성이 있다고 판단하여 자동으로 최신 데이터를 다시 가져옵니다.
*   **선언적인 코드**: "어떻게" 데이터를 가져올지(How)가 아니라, "무엇을" 보여줄지(What)에 집중하여 코드를 작성할 수 있습니다.
*   **강력한 개발 도구**: `React Query Devtools`를 통해 데이터 캐시 상태, 쿼리 진행 상황 등을 시각적으로 확인하고 디버깅할 수 있습니다.

## 1-3) TanStack Query 시작하기

### 1-3-1) 설치

먼저 프로젝트에 TanStack Query를 설치합니다.

```bash
npm install @tanstack/react-query
```

개발 중에 유용하게 사용할 `React Query Devtools`도 함께 설치해 줍니다.

```bash
npm install @tanstack/react-query-devtools
```

### 1-3-2) `QueryClientProvider` 설정

TanStack Query를 사용하기 위해서는 애플리케이션의 최상단(보통 `index.js`나 `App.js`)을 `QueryClientProvider`로 감싸주어야 합니다. 이를 통해 앱의 모든 컴포넌트에서 TanStack Query의 기능과 캐시를 공유할 수 있게 됩니다.

1.  `QueryClient` 인스턴스를 생성합니다.
2.  `QueryClientProvider` 컴포넌트로 전체 앱을 감싸고, `client` prop으로 생성한 인스턴스를 전달합니다.
3.  `ReactQueryDevtools`를 추가하여 개발 환경에서 쿼리 상태를 확인할 수 있도록 합니다.

**`App.js` 또는 `main.jsx` 예시:**

```jsx
import React from 'react';
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { ReactQueryDevtools } from '@tanstack/react-query-devtools';
import MyComponent from './MyComponent'; // 데이터를 사용할 컴포넌트

// 1. QueryClient 인스턴스 생성
const queryClient = new QueryClient();

function App() {
  return (
    // 2. QueryClientProvider로 앱 감싸기
    <QueryClientProvider client={queryClient}>
      <MyComponent />
      
      {/* 3. ReactQueryDevtools 추가 */}
      <ReactQueryDevtools initialIsOpen={false} />
    </QueryClientProvider>
  );
}

export default App;
```

> [!IMPORTANT]
> `QueryClientProvider`는 **한 번만** 설정하면 됩니다. 앱 전체에서 `queryClient` 인스턴스가 공유되며, 이 인스턴스가 모든 쿼리의 캐시를 관리합니다.

## 1-4) 핵심 Hook: `useQuery`

TanStack Query에서 데이터를 **가져오기(GET 요청)** 위해 사용하는 가장 기본적인 Hook이 바로 `useQuery`입니다. `useQuery`는 서버에서 데이터를 가져오는 비동기 로직을 캡슐화하고, 그 과정에서 발생하는 다양한 상태(로딩, 에러, 성공)를 자동으로 관리해 줍니다.

### 1-4-1) `useQuery`의 기본 사용법

`useQuery`는 두 개의 주요 인자를 받습니다.

1.  **Query Key (쿼리 키)**:
    *   쿼리를 식별하는 **고유한 키**입니다. TanStack Query는 이 키를 사용하여 데이터를 캐싱하고 관리합니다.
    *   주로 **배열** 형태로 지정합니다. 배열의 첫 번째 요소는 데이터의 종류를 나타내는 문자열, 이후 요소들은 해당 쿼리를 고유하게 만드는 변수나 식별자를 넣습니다.
    *   예: `['posts']`, `['posts', 10]`, `['todos', { status: 'done' }]`

2.  **Query Function (쿼리 함수)**:
    *   실제로 데이터를 가져오는 비동기 함수입니다.
    *   이 함수는 반드시 **Promise**를 반환해야 합니다. `fetch`나 `axios`와 같은 라이브러리를 사용하여 API를 호출하는 로직이 여기에 들어갑니다.

### 1-4-2) `useQuery` 예제

`useEffect`와 `useState`를 사용했던 사용자 목록 가져오기 예제를 `useQuery`로 바꿔보겠습니다.

```jsx
import React from 'react';
import { useQuery } from '@tanstack/react-query';

// 데이터를 가져오는 비동기 함수 (Query Function)
const fetchUsers = async () => {
  const response = await fetch('https://jsonplaceholder.typicode.com/users');
  if (!response.ok) {
    throw new Error('네트워크에 문제가 있습니다.');
  }
  return response.json();
};

function UserList() {
  // useQuery를 사용하여 데이터 가져오기
  const { data, isLoading, isError, error } = useQuery({
    queryKey: ['users'], // Query Key
    queryFn: fetchUsers,  // Query Function
  });

  // 로딩 중일 때
  if (isLoading) {
    return <span>로딩 중...</span>;
  }

  // 에러 발생 시
  if (isError) {
    return <span>에러가 발생했습니다: {error.message}</span>;
  }

  // 데이터 로딩 성공 시
  return (
    <ul>
      {data.map(user => (
        <li key={user.id}>{user.name} ({user.email})</li>
      ))}
    </ul>
  );
}

export default UserList;
```

### 1-4-3) `useQuery`가 반환하는 값

`useQuery`는 쿼리의 현재 상태를 나타내는 다양한 정보를 담은 객체를 반환합니다. 가장 핵심적인 값들은 다음과 같습니다.

*   `data`: 쿼리가 성공적으로 완료되었을 때 가져온 **데이터**입니다. (초기값: `undefined`)
*   `isLoading`: 쿼리가 현재 실행 중이며 아직 데이터가 없을 때 `true`가 됩니다. (초기 로딩 상태)
*   `isFetching`: 쿼리가 실행 중일 때(초기 로딩 포함, 백그라운드 리페칭 포함) `true`가 됩니다.
*   `isError`: 쿼리 실행 중 에러가 발생했을 때 `true`가 됩니다.
*   `error`: 에러가 발생했을 때의 **에러 객체**입니다.
*   `status`: 쿼리의 현재 상태를 나타내는 문자열입니다. `'pending'`, `'error'`, `'success'` 중 하나의 값을 가집니다. (`isLoading`은 `status === 'pending'`의 단축 표현입니다.)
*   `refetch`: 해당 쿼리를 수동으로 다시 실행하는 함수입니다.

> [!TIP]
> **`isLoading` vs `isFetching`**
>
> *   `isLoading`: **처음** 데이터를 가져올 때만 `true`가 됩니다. 캐시된 데이터가 없는 상태에서의 로딩을 의미합니다.
> *   `isFetching`: 초기 로딩을 포함하여, **캐시된 데이터가 있더라도** 백그라운드에서 데이터를 새로고침(리페칭)하는 모든 경우에 `true`가 됩니다.
>
> 일반적으로 사용자에게 보여주는 로딩 UI는 `isLoading` 상태를 사용하고, 백그라운드 업데이트 인디케이터 등은 `isFetching`을 사용할 수 있습니다.

`useQuery` 하나만으로도 로딩, 에러, 성공 상태를 모두 관리할 수 있어 코드가 훨씬 간결하고 직관적으로 변한 것을 볼 수 있습니다. `useState`와 `useEffect`를 직접 조합할 때보다 훨씬 적은 코드로 더 많은 기능을 구현할 수 있습니다.

다음 강의에서는 TanStack Query의 캐싱, 데이터 동기화, 데이터 업데이트(Mutation) 등 더 강력한 기능들을 자세히 알아보겠습니다.

---

- [목차로 돌아가기](./README.md)
- [이전 강의로 이동](../day5/README.md)
- [다음 강의로 이동](./02-TanStack-Query-Features.md)
