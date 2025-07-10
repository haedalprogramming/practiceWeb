# 5) API 연동 (API Integration)

우리가 만드는 웹 애플리케이션은 단순히 정적인 정보만 보여주는 것이 아니라, 다양한 외부 데이터와 상호작용해야 할 때가 많습니다. 예를 들어, 실시간 날씨 정보를 보여주거나, 주식 시세를 가져오거나, 다른 서비스의 사용자 정보를 연동하는 등의 작업이 필요합니다.

이러한 외부 데이터와 소통하기 위해 사용되는 것이 바로 <strong>API (Application Programming Interface)</strong>입니다. 이번 강의에서는 API가 무엇인지, 그리고 JavaScript를 사용하여 웹 애플리케이션에서 어떻게 API와 통신하여 데이터를 가져오는지 알아보겠습니다.

## 1) API란 무엇인가?

**API**는 **A**pplication **P**rogramming **I**nterface의 약자입니다. 쉽게 말해, **서로 다른 프로그램들이 소통하기 위한 규칙 또는 방법을 정의해 놓은 것**입니다.

우리가 웹 브라우저를 통해 네이버나 구글 같은 웹사이트에 접속할 때, 브라우저는 해당 웹사이트의 서버에 "이 페이지를 보여줘"라고 요청하고 서버는 요청에 맞는 페이지를 보내줍니다. 이처럼 클라이언트(브라우저)와 서버가 데이터를 주고받는 과정에서 API가 중요한 역할을 합니다. 즉 웹에서 API는 우리가 원하는 데이터를 요청하고, 그 데이터를 받는 방법을 정해놓은 약속이라고 생각할 수 있습니다.

### 1-1) 왜 API가 필요한가?

*   **데이터 공유**: 다른 서비스가 가지고 있는 유용한 데이터를 우리 애플리케이션에서 사용할 수 있게 합니다. (예: 날씨 정보, 지도 정보, 결제 시스템)
*   **기능 확장**: 우리 애플리케이션에 없는 기능을 외부 서비스의 API를 통해 추가할 수 있습니다. (예: 소셜 로그인, 문자 메시지 발송)
*   **모듈화**: 복잡한 시스템을 여러 개의 작은 서비스로 나누어 개발하고, 이 서비스들이 API를 통해 서로 소통하게 할 수 있습니다.

우리가 웹 애플리케이션을 만들 때, 대부분의 데이터는 서버에 저장되어 있습니다. 이 데이터를 가져오거나, 새로운 데이터를 서버에 저장하거나, 기존 데이터를 수정하거나 삭제하는 등의 작업을 할 때 API를 사용하게 됩니다.

> [!IMPORTANT]
> API는 클라이언트와 서버 간의 통신에서 중요한 역할을 합니다.
> [1일차 "클라이언트-서버 구조"](../day1/02-Client-Server-Architecture.md) 강의에서 배운 클라이언트-서버 구조를 다시 한번 떠올려 보세요. API는 클라이언트가 서버에게 요청을 보내고, 서버가 응답을 보내는 그 **요청과 응답의 규칙**이라고 할 수 있습니다.

## 2) 웹 API의 종류

웹에서 가장 흔하게 사용되는 API는 **REST API**입니다.

*   **REST API**: 웹의 기본 통신 방식인 HTTP를 기반으로 데이터를 주고받는 규칙입니다. 자원(Resource)을 이름으로 구분하고, HTTP 메서드(GET, POST, PUT, DELETE)를 통해 해당 자원에 대한 CRUD(생성, 읽기, 수정, 삭제) 작업을 수행합니다. 다음 강의에서 REST API에 대해 더 자세히 다룰 예정입니다.
*   **GraphQL**: REST API의 대안으로 등장한 쿼리 언어입니다. 클라이언트가 필요한 데이터를 정확히 요청할 수 있어, 불필요한 데이터를 덜 가져오거나 여러 번 요청할 필요 없이 한 번에 원하는 데이터를 모두 가져올 수 있다는 장점이 있습니다.

## 3) API 통신 방법

API 통신은 기본적으로 [1일차 "HTTP 프로토콜"](../day1/03-HTTP.md) 강의에서 배운 **HTTP 프로토콜**을 사용합니다. 클라이언트가 HTTP 요청을 서버에 보내면, 서버는 HTTP 응답을 클라이언트에게 보냅니다.

주로 사용되는 HTTP 메서드는 다음과 같습니다.

*   **GET**: 서버로부터 데이터를 **조회**할 때 사용합니다. (예: 게시물 목록 가져오기, 특정 사용자 정보 가져오기)
*   **POST**: 서버에 새로운 데이터를 **생성**하거나 전송할 때 사용합니다. (예: 새 게시물 작성, 회원가입)
*   **PUT**: 서버의 데이터를 **전체 수정**할 때 사용합니다.
*   **PATCH**: 서버의 데이터를 **부분 수정**할 때 사용합니다.
*   **DELETE**: 서버의 데이터를 **삭제**할 때 사용합니다.

우리가 웹 애플리케이션에서 API와 통신한다는 것은, 결국 JavaScript 코드를 통해 이러한 HTTP 요청을 보내고 응답을 처리하는 것을 의미합니다.

## 4) JavaScript에서 API 데이터 가져오기: `fetch` API

JavaScript에서는 웹 API와 통신하기 위한 여러 방법이 있지만, 가장 기본적으로 내장되어 있는 `fetch` API를 많이 사용합니다. `fetch` API는 네트워크 요청을 보내고 응답을 받는 기능을 제공합니다.

### 4-1) `fetch` 함수의 기본 사용법

`fetch` 함수는 첫 번째 인자로 요청을 보낼 **URL 주소**를 받습니다. 이 함수는 **Promise**라는 특별한 객체를 반환합니다.

> [!NOTE]
> **Promise(프로미스) 리마인드**
> 
> [2일차 "비동기 패턴"](../day2/10-ES6-Async-Patterns.md) 강의에서 배운 Promise에 대해 다시 한번 떠올려 보세요.
> 
> Promise는 JavaScript에서 **비동기 작업**을 다룰 때 사용되는 객체입니다. 비동기 작업이란, 특정 작업(예: 네트워크 요청)이 완료될 때까지 기다리지 않고 다음 코드를 먼저 실행하는 것을 말합니다.
> 
> `fetch`와 같은 네트워크 요청은 시간이 오래 걸릴 수 있습니다. Promise는 "미래에 어떤 결과(성공 또는 실패)를 돌려주겠다"는 약속과 같습니다.
> 
> *   `.then()`: Promise가 성공적으로 완료되었을 때 실행될 코드를 정의합니다.
> *   `.catch()`: Promise가 실패(에러 발생)했을 때 실행될 코드를 정의합니다.
> 
> Promise에 대한 자세한 내용은 추후 비동기 패턴 강의에서 더 깊이 다룰 예정입니다. 지금은 "네트워크 요청이 끝나면 `.then()` 안의 코드가 실행된다" 정도로 이해하면 충분합니다.

`fetch`를 사용하여 데이터를 가져오는 기본적인 흐름은 다음과 같습니다.

1.  `fetch(URL)`: 지정된 URL로 네트워크 요청을 보냅니다.
2.  `.then(response => ...)`: 요청이 성공하면 서버로부터 받은 **응답(Response) 객체**를 받습니다. 이 응답 객체에는 HTTP 상태 코드, 헤더 등 다양한 정보가 포함되어 있습니다.
3.  `response.json()`: 대부분의 웹 API는 데이터를 **JSON (JavaScript Object Notation)** 형식으로 보냅니다. `response.json()` 메서드는 응답 본문을 JSON 형태로 파싱(해석)하여 JavaScript 객체로 변환해주는 Promise를 반환합니다.
4.  `.then(data => ...)`: `response.json()`이 성공하면, 최종적으로 우리가 원하는 **데이터(JavaScript 객체)**를 받습니다. 이 데이터를 가지고 화면에 표시하거나 다른 작업을 수행합니다.
5.  `.catch(error => ...)`: 네트워크 요청 중 에러가 발생하면 이 부분이 실행됩니다.

### 4-2) JSON (JavaScript Object Notation)

1일차 강의에서 간단하게 다루고 넘어간 JSON에 대해 다시 한번 떠올려 보세요.

JSON은 데이터를 주고받을 때 가장 널리 사용되는 텍스트 기반의 데이터 형식입니다. JavaScript 객체와 매우 유사하게 생겼습니다.

**예시:**
```json
{
  "name": "홍길동",
  "age": 17,
  "isStudent": true,
  "hobbies": ["코딩", "독서", "운동"],
  "address": {
    "city": "서울",
    "zipCode": "01234"
  }
}
```
*   데이터는 `키(key)`와 `값(value)`의 쌍으로 이루어집니다.
*   `{}`는 객체를, `[]`는 배열을 나타냅니다.
*   문자열은 큰따옴표(`""`)로 묶어야 합니다.

### 4-3) `fetch`를 사용한 데이터 가져오기 예제

가상의 사용자 데이터를 제공하는 `JSONPlaceholder`라는 공개 API를 사용하여 사용자 목록을 가져오는 예제를 살펴보겠습니다.

```javascript
// fetch를 사용하여 사용자 데이터를 가져오는 함수
function fetchUsers() {
  // 1. fetch 함수로 API 요청 보내기
  fetch('https://jsonplaceholder.typicode.com/users')
    // 2. 첫 번째 .then(): 응답(Response) 객체를 받아서 JSON 형태로 파싱
    .then(response => {
      // 응답이 성공적인지 확인 (HTTP 상태 코드 200번대)
      if (!response.ok) {
        // 에러가 발생하면 에러 메시지를 던집니다.
        throw new Error(`HTTP error! status: ${response.status}`);
      }
      return response.json(); // 응답 본문을 JSON으로 변환
    })
    // 3. 두 번째 .then(): 파싱된 JSON 데이터(JavaScript 객체)를 받아서 처리
    .then(data => {
      console.log('가져온 사용자 데이터:', data);
      // 여기에서 데이터를 화면에 표시하거나 다른 작업을 수행할 수 있습니다.
      data.forEach(user => {
        console.log(`이름: ${user.name}, 이메일: ${user.email}`);
      });
    })
    // 4. .catch(): 네트워크 요청 또는 데이터 처리 중 에러가 발생하면 처리
    .catch(error => {
      console.error('데이터를 가져오는 중 오류 발생:', error);
    });
}

// 함수 호출
fetchUsers();
```

이 코드를 웹 브라우저의 개발자 도구(Console 탭)에서 실행하면, `https://jsonplaceholder.typicode.com/users`에서 사용자 데이터를 가져와 콘솔에 출력하는 것을 볼 수 있습니다.

### 4-4) `fetch` 요청 옵션 설정하기

`fetch` 함수는 URL만 인자로 받는 GET 요청 외에도, 다양한 옵션을 설정하여 POST, PUT, DELETE 등 다른 종류의 HTTP 요청을 보낼 수 있습니다. 이러한 옵션들은 `fetch` 함수의 두 번째 인자인 **객체** 형태로 전달합니다.

주요 옵션들은 다음과 같습니다.

*   `method`: HTTP 요청 메서드를 지정합니다. (기본값: `'GET'`)
    *   `'GET'`, `'POST'`, `'PUT'`, `'PATCH'`, `'DELETE'` 등
*   `headers`: HTTP 요청 헤더를 객체 형태로 지정합니다. 헤더는 요청에 대한 부가적인 정보를 담고 있습니다.
    *   `'Content-Type'`: 보내는 데이터의 형식을 서버에 알려줍니다. 예를 들어 JSON 데이터를 보낼 때는 `'application/json'`으로 설정합니다.
*   `body`: 요청에 담아 보낼 데이터를 지정합니다. 주로 `POST`, `PUT`, `PATCH` 요청에서 사용됩니다.
    *   `body`에 담아 보내는 데이터는 보통 문자열 형태여야 합니다. JavaScript 객체를 JSON 형식으로 보내려면 `JSON.stringify()` 함수를 사용하여 객체를 문자열로 변환해야 합니다.

#### `fetch`를 사용한 POST 요청 예제

새로운 게시물을 생성하는 `POST` 요청을 보내는 예제입니다.

```javascript
// 서버에 전송할 새로운 게시물 데이터
const newPost = {
  title: '새로운 게시물',
  body: '이것은 fetch를 사용하여 만든 새로운 게시물입니다.',
  userId: 1,
};

// fetch를 사용하여 POST 요청 보내는 함수
async function createPost(postData) {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/posts', {
      method: 'POST', // HTTP 메서드를 'POST'로 지정
      headers: {
        'Content-Type': 'application/json', // 보내는 데이터가 JSON 형식임을 명시
      },
      body: JSON.stringify(postData), // JavaScript 객체를 JSON 문자열로 변환하여 body에 담기
    });

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    const data = await response.json();
    console.log('서버로부터 받은 응답:', data);
    console.log('새로운 게시물이 성공적으로 생성되었습니다!');

  } catch (error) {
    console.error('게시물을 생성하는 중 오류 발생:', error);
  }
}

// 함수 호출
createPost(newPost);
```

이 예제에서는 `fetch` 함수의 두 번째 인자로 옵션 객체를 전달했습니다.
- `method`를 `'POST'`로 설정하여 새로운 데이터를 생성하겠다는 것을 서버에 알립니다.
- `headers`의 `Content-Type`을 `'application/json'`으로 설정하여, 우리가 보내는 `body`의 데이터가 JSON 형식임을 명시합니다.
- `body`에는 `JSON.stringify()`를 사용하여 JavaScript 객체인 `newPost`를 JSON 문자열로 변환하여 담았습니다.

서버는 이 요청을 받고 새로운 게시물 데이터를 생성한 후, 생성된 결과를 다시 응답으로 보내줍니다.

## 5) `async/await`를 사용한 비동기 처리

`Promise`의 `.then()` 체인은 여러 비동기 작업이 연속될 때 코드가 복잡해지고 가독성이 떨어질 수 있습니다. 이를 개선하기 위해 JavaScript에서는 `async`와 `await` 키워드를 제공합니다.

*   **`async`**: 함수 앞에 붙여서 해당 함수가 비동기 함수임을 선언합니다. `async` 함수는 항상 `Promise`를 반환합니다.
*   **`await`**: `async` 함수 내부에서만 사용할 수 있습니다. `await` 키워드가 붙은 `Promise`가 완료될 때까지 함수의 실행을 **일시 중지**합니다. `Promise`가 성공적으로 완료되면 그 결과를 반환하고, 실패하면 에러를 던집니다.

`async/await`를 사용하면 비동기 코드를 마치 동기 코드처럼 순차적으로 작성할 수 있어 가독성이 훨씬 좋아집니다.

### 5-1) `async/await`를 사용한 예제

위의 `fetchUsers` 함수를 `async/await`로 다시 작성해 보겠습니다.

```javascript
async function fetchUsersAsync() {
  try {
    // 1. await: fetch 요청이 완료될 때까지 기다립니다.
    const response = await fetch('https://jsonplaceholder.typicode.com/users');

    // 응답이 성공적인지 확인
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    // 2. await: 응답 본문을 JSON으로 파싱하는 작업이 완료될 때까지 기다립니다.
    const data = await response.json();

    console.log('가져온 사용자 데이터 (async/await):', data);
    data.forEach(user => {
      console.log(`이름: ${user.name}, 이메일: ${user.email}`);
    });

  } catch (error) {
    // try 블록 안에서 발생한 모든 에러를 여기서 처리합니다.
    console.error('데이터를 가져오는 중 오류 발생 (async/await):', error);
  }
}

// 함수 호출
fetchUsersAsync();
```

`async/await`를 사용하면 `fetch` 요청과 JSON 파싱이 순차적으로 일어나는 것처럼 코드를 작성할 수 있습니다. 에러 처리는 `try...catch` 블록을 사용하여 동기 코드와 유사하게 처리할 수 있습니다.

## 6) React 컴포넌트에서 API 연동하기

React 컴포넌트에서 API를 연동할 때는 주로 `useEffect` Hook을 사용합니다. `useEffect`는 컴포넌트가 렌더링된 후에 특정 작업을 수행하도록 해줍니다. API 요청은 보통 컴포넌트가 처음 화면에 나타날 때 (마운트될 때) 한 번만 보내는 경우가 많습니다.

```jsx
import React, { useState, useEffect } from 'react';

function UserList() {
  const [users, setUsers] = useState([]); // 사용자 데이터를 저장할 상태
  const [loading, setLoading] = useState(true); // 로딩 상태
  const [error, setError] = useState(null); // 에러 상태

  // 컴포넌트가 처음 마운트될 때만 API 요청을 보냅니다.
  useEffect(() => {
    async function fetchUsers() {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users');
        if (!response.ok) {
          throw new Error(`HTTP error! status: ${response.status}`);
        }
        const data = await response.json();
        setUsers(data); // 가져온 데이터를 상태에 저장
      } catch (err) {
        setError(err); // 에러 발생 시 에러 상태 업데이트
      } finally {
        setLoading(false); // 로딩 완료
      }
    }

    fetchUsers(); // 함수 호출
  }, []); // 빈 배열을 의존성으로 전달하여 컴포넌트 마운트 시 한 번만 실행

  if (loading) {
    return <div>사용자 데이터를 불러오는 중...</div>;
  }

  if (error) {
    return <div>오류 발생: {error.message}</div>;
  }

  return (
    <div>
      <h2>사용자 목록</h2>
      <ul>
        {users.map(user => (
          <li key={user.id}>
            <strong>{user.name}</strong> ({user.email})
          </li>
        ))}
      </ul>
    </div>
  );
}

export default UserList;
```

이 예제에서는:
1.  `useState`를 사용하여 `users` (사용자 데이터), `loading` (데이터 로딩 중인지 여부), `error` (에러 발생 여부) 상태를 관리합니다.
2.  `useEffect` Hook 내부에서 `fetchUsers`라는 비동기 함수를 정의하고 호출합니다.
3.  `fetchUsers` 함수는 `async/await`를 사용하여 API 요청을 보내고 데이터를 가져옵니다.
4.  데이터를 성공적으로 가져오면 `setUsers`로 `users` 상태를 업데이트하고, `setLoading(false)`로 로딩 상태를 해제합니다.
5.  에러가 발생하면 `setError`로 에러 상태를 업데이트합니다.
6.  컴포넌트의 렌더링 부분에서는 `loading`과 `error` 상태에 따라 다른 메시지를 보여주고, 데이터가 성공적으로 로드되면 사용자 목록을 표시합니다.
7.  `useEffect`의 두 번째 인자로 빈 배열(`[]`)을 전달하여, 이 효과(effect)가 컴포넌트가 처음 마운트될 때 **한 번만** 실행되도록 합니다.

## 7) 정리

*   <strong>API (Application Programming Interface)</strong>는 서로 다른 프로그램들이 소통하기 위한 규칙 또는 방법입니다. 웹 애플리케이션에서는 주로 서버와 데이터를 주고받기 위해 사용됩니다.
*   웹 API 통신은 **HTTP 프로토콜**을 기반으로 하며, `GET`, `POST` 등의 HTTP 메서드를 사용하여 데이터를 조회하거나 생성, 수정, 삭제합니다.
*   JavaScript에서 API와 통신하는 가장 기본적인 방법은 내장된 **`fetch` API**를 사용하는 것입니다. `fetch`는 `Promise`를 반환하여 비동기 작업을 처리합니다.
*   `async/await`는 `Promise` 기반의 비동기 코드를 더 읽기 쉽고 동기 코드처럼 작성할 수 있게 해주는 문법입니다. `try...catch`를 사용하여 에러를 처리합니다.
*   React 컴포넌트에서 API를 연동할 때는 주로 **`useEffect` Hook**을 사용하여 컴포넌트의 생명주기에 맞춰 데이터를 가져옵니다.

---

- [목차로 돌아가기](./README.md)
- [이전 강의로 이동](./04-useContext-Hook.md)
- [다음 강의로 이동](./06-REST-API.md)