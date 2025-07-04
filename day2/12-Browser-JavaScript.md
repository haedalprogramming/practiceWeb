# 12. 브라우저 JavaScript

## 목차
1. [브라우저 환경이란?](#1-브라우저-환경이란)
2. [사용자에게 메시지를 보여주고 입력받기](#2-사용자에게-메시지를-보여주고-입력받기)
   - [2-1) alert(): 경고 메시지 보여주기](#2-1-alert-경고-메시지-보여주기)
   - [2-2) prompt(): 사용자로부터 입력받기](#2-2-prompt-사용자로부터-입력받기)
   - [2-3) confirm(): 사용자에게 확인/취소 질문하기](#2-3-confirm-사용자에게-확인취소-질문하기)
3. [브라우저 API](#3-브라우저-api)
   - [3-1) fetch() API (네트워크 요청)](#3-1-fetch-api-네트워크-요청)
   - [3-2) localStorage와 sessionStorage (웹 스토리지)](#3-2-localstorage와-sessionstorage-웹-스토리지)
   - [3-3) Geolocation API (위치 정보)](#3-3-geolocation-api-위치-정보)
4. [보안 제약(CORS, Same-Origin Policy)](#4-보안-제약cors-same-origin-policy)
   - [4-1) 동일 출처 정책 (Same-Origin Policy, SOP)](#4-1-동일-출처-정책-same-origin-policy-sop)
   - [4-2) CORS (Cross-Origin Resource Sharing)](#4-2-cors-cross-origin-resource-sharing)

---

## 1) 브라우저 환경이란?

우리가 인터넷에서 보는 모든 웹 페이지(사이트)는 **웹 브라우저**라는 특별한 프로그램 안에서 실행됩니다. 웹 브라우저(예: Chrome, Firefox, Safari, Edge)는 단순히 웹 페이지를 보여주는 것을 넘어, HTML, CSS, JavaScript 코드를 해석하고 실행하여 우리가 보고 상호작용할 수 있는 형태로 만들어주는 역할을 합니다.

웹 페이지는 크게 세 가지 요소로 구성됩니다:

*   **HTML (HyperText Markup Language)**: 웹 페이지의 뼈대와 내용을 구성합니다. 제목, 문단, 이미지, 링크 등 웹 페이지에 들어갈 모든 요소들을 정의합니다.
*   **CSS (Cascading Style Sheets)**: 웹 페이지의 디자인과 스타일을 담당합니다. 글자 색깔, 크기, 배경 이미지, 레이아웃 등 웹 페이지를 예쁘게 꾸미는 역할을 합니다.
*   **JavaScript**: 웹 페이지에 동적인 기능과 상호작용을 추가합니다. 버튼을 클릭했을 때 메뉴가 열리거나, 이미지가 슬라이드처럼 움직이는 등 사용자의 행동에 반응하는 모든 기능은 JavaScript를 통해 구현됩니다.

브라우저는 이 세 가지 언어를 모두 이해하고 실행하여, 정적인 문서였던 웹 페이지를 살아있는 애플리케이션처럼 만들어줍니다. JavaScript는 브라우저 안에서 웹 페이지의 HTML과 CSS를 조작하고, 사용자 이벤트를 처리하며, 서버와 통신하는 등 다양한 작업을 수행할 수 있습니다.

## 2) 사용자에게 메시지를 보여주고 입력받기

JavaScript는 웹 브라우저를 통해 사용자에게 간단한 메시지를 보여주거나, 사용자로부터 입력을 받을 수 있는 기능을 제공합니다. 이 기능들은 주로 웹 페이지에서 사용자에게 어떤 정보를 알리거나, 간단한 질문을 할 때 사용됩니다.

### 2-1) `alert()`: 경고 메시지 보여주기

`alert()` 함수는 사용자에게 경고나 알림 메시지를 보여주는 작은 팝업 창을 띄웁니다. 이 팝업 창에는 메시지와 함께 '확인' 버튼이 하나 있습니다. 사용자가 '확인' 버튼을 누르기 전까지는 웹 페이지의 다른 작업을 할 수 없습니다.

```javascript
alert("안녕하세요! 웹 페이지에 오신 것을 환영합니다.");
alert("이것은 중요한 알림입니다!");
```

> [!NOTE]
> `alert()`는 사용자에게 정보를 전달하는 가장 간단한 방법이지만, 사용자가 '확인'을 누르기 전까지는 페이지가 멈추기 때문에 너무 자주 사용하면 사용자 경험을 해칠 수 있습니다.

### 2-2) `prompt()`: 사용자로부터 입력받기

`prompt()` 함수는 사용자에게 질문을 하고, 사용자가 텍스트를 입력할 수 있는 작은 팝업 창을 띄웁니다. 이 팝업 창에는 질문 메시지, 텍스트 입력란, 그리고 '확인'과 '취소' 버튼이 있습니다. 사용자가 입력한 값은 JavaScript 코드에서 사용할 수 있습니다.

```javascript
const userName = prompt("이름이 무엇인가요?");
if (userName) { // 사용자가 '확인'을 누르고 이름을 입력했을 경우
  alert("환영합니다, " + userName + "님!");
} else { // 사용자가 '취소'를 누르거나 아무것도 입력하지 않고 '확인'을 눌렀을 경우
  alert("이름을 입력하지 않으셨군요.");
}

const userAge = prompt("나이를 입력해주세요.", "0"); // 두 번째 인자는 기본값입니다.
if (userAge !== null && userAge !== "") { // 사용자가 '취소'를 누르지 않고 값을 입력했을 경우
  alert("당신의 나이는 " + userAge + "세 입니다.");
} else {
  alert("나이를 입력하지 않으셨습니다.");
}
```

> [!TIP]
> `prompt()`의 두 번째 인자는 입력란에 미리 채워질 기본값을 설정할 수 있습니다. 사용자가 아무것도 입력하지 않고 '확인'을 누르면 이 기본값이 반환됩니다. 사용자가 '취소' 버튼을 누르면 `null`이 반환됩니다.

### 2-3) `confirm()`: 사용자에게 확인/취소 질문하기

`confirm()` 함수는 사용자에게 '예/아니오'와 같은 확인 또는 취소 질문을 하고, 그 결과를 `true`(확인) 또는 `false`(취소)로 반환하는 작은 팝업 창을 띄웁니다. 이 팝업 창에는 질문 메시지와 함께 '확인'과 '취소' 버튼이 있습니다.

```javascript
const isConfirmed = confirm("정말로 이 페이지를 떠나시겠습니까?");
if (isConfirmed) {
  alert("안녕히 가세요!");
  // 여기에 페이지를 떠나는 등의 추가 동작을 넣을 수 있습니다.
} else {
  alert("계속 머물러 주셔서 감사합니다.");
}
```

> [!CAUTION]
> `confirm()`은 사용자의 중요한 결정을 확인받을 때 유용합니다. 예를 들어, 데이터를 삭제하기 전에 사용자에게 한 번 더 확인을 요청하는 경우에 사용할 수 있습니다.

---

## 3) 브라우저 API

브라우저는 웹 페이지에서 다양한 기능을 수행할 수 있도록 여러 가지 내장된 API(Application Programming Interface)를 제공합니다. API는 프로그램들이 서로 통신하고 기능을 주고받을 수 있도록 정의된 규칙들의 집합입니다. 여기서는 웹 개발에서 자주 사용되는 몇 가지 브라우저 API를 소개합니다.

### 3-1) `fetch()` API (네트워크 요청)

`fetch()` API는 웹 페이지에서 서버로 네트워크 요청을 보내고, 서버로부터 데이터를 받아올 때 사용합니다. 웹 페이지에서 새로운 게시물을 불러오거나, 사용자 정보를 업데이트하는 등의 작업에 활용됩니다. `fetch()`는 Promise를 반환하므로 비동기적으로 동작합니다.

```javascript
// 예시: 공공 API에서 데이터를 가져오기
fetch('https://jsonplaceholder.typicode.com/todos/1') // 가상의 할 일 목록 API
  .then(response => response.json()) // 서버 응답을 JSON 형태로 변환합니다.
  .then(data => {
    console.log('가져온 데이터:', data);
    // 출력 예시: 가져온 데이터: { userId: 1, id: 1, title: 'delectus aut autem', completed: false }
  })
  .catch(error => {
    console.error('데이터 가져오기 실패:', error);
  });

// async/await와 함께 사용하면 더 깔끔하게 작성할 수 있습니다.
async function getPost() {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/posts/1');
    const post = await response.json();
    console.log('게시물:', post);
  } catch (error) {
    console.error('게시물 가져오기 오류:', error);
  }
}
getPost();
```

> [!NOTE]
> `fetch()`는 웹 페이지와 서버 간의 통신을 담당하는 매우 중요한 API입니다. 이를 통해 웹 페이지는 동적으로 필요한 데이터를 가져와서 화면에 표시할 수 있습니다.

### 3-2) `localStorage`와 `sessionStorage` (웹 스토리지)

`localStorage`와 `sessionStorage`는 웹 브라우저에 데이터를 저장할 수 있는 공간을 제공합니다. 이를 '웹 스토리지(Web Storage)'라고 부르며, 웹 페이지가 닫히거나 새로고침되어도 데이터를 유지할 수 있게 해줍니다.

*   **`localStorage`**: 브라우저를 닫았다가 다시 열어도 데이터가 유지됩니다. 영구적인 데이터 저장에 적합합니다. (예: 사용자 설정, 로그인 상태 유지)
*   **`sessionStorage`**: 브라우저 탭이나 창이 닫히면 데이터가 사라집니다. 일시적인 데이터 저장에 적합합니다. (예: 일회성 로그인 정보, 장바구니 내용)

```javascript
// localStorage 사용 예시
// 데이터 저장: setItem(key, value)
localStorage.setItem('username', '철수');
localStorage.setItem('theme', 'dark');

// 데이터 불러오기: getItem(key)
const username = localStorage.getItem('username');
console.log('저장된 사용자 이름:', username); // 출력: 저장된 사용자 이름: 철수

// 데이터 삭제: removeItem(key)
// localStorage.removeItem('theme');

// 모든 데이터 삭제: clear()
// localStorage.clear();

// sessionStorage 사용 예시 (localStorage와 사용법은 동일합니다.)
sessionStorage.setItem('lastVisitedPage', '/products');
const lastPage = sessionStorage.getItem('lastVisitedPage');
console.log('마지막 방문 페이지:', lastPage); // 출력: 마지막 방문 페이지: /products
```

> [!TIP]
> 웹 스토리지는 쿠키(Cookie)보다 더 많은 데이터를 저장할 수 있고, 서버로 전송되지 않아 보안상 이점이 있습니다. 하지만 민감한 정보(비밀번호 등)를 직접 저장하는 것은 피해야 합니다.

### 3-3) Geolocation API (위치 정보)

Geolocation API는 사용자의 동의를 얻어 웹 페이지에서 사용자의 현재 위치 정보(위도, 경도)를 가져올 수 있게 해줍니다. 지도 서비스나 위치 기반 서비스에서 활용됩니다.

```javascript
function getLocation() {
  if (navigator.geolocation) { // 브라우저가 Geolocation을 지원하는지 확인
    navigator.geolocation.getCurrentPosition(
      (position) => { // 위치 정보를 성공적으로 가져왔을 때 실행되는 콜백 함수
        const latitude = position.coords.latitude;
        const longitude = position.coords.longitude;
        console.log(`현재 위치: 위도 ${latitude}, 경도 ${longitude}`);
      },
      (error) => { // 위치 정보를 가져오는 데 실패했을 때 실행되는 콜백 함수
        console.error('위치 정보를 가져오는 데 실패했습니다:', error.message);
      }
    );
  } else {
    console.log('이 브라우저는 Geolocation을 지원하지 않습니다.');
  }
}

// 버튼 클릭 시 위치 정보 가져오기
// <button onclick="getLocation()">내 위치 가져오기</button> 와 함께 사용
// getLocation();
```

> [!NOTE]
> Geolocation API는 사용자의 개인 정보와 관련된 기능이므로, 브라우저는 반드시 사용자에게 위치 정보 접근 권한을 요청하고 동의를 받아야만 위치 정보를 가져올 수 있습니다.

## 4) 보안 제약(CORS, Same-Origin Policy)

웹 브라우저는 사용자의 보안과 개인 정보 보호를 위해 여러 가지 보안 제약을 두고 있습니다. 이 중 가장 중요하고 자주 접하게 되는 것이 '동일 출처 정책(Same-Origin Policy)'과 'CORS(Cross-Origin Resource Sharing)'입니다.

### 4-1) 동일 출처 정책 (Same-Origin Policy, SOP)

**동일 출처 정책(SOP)**은 웹 브라우저의 가장 기본적인 보안 정책입니다. 이 정책은 한 '출처(Origin)'에서 로드된 문서나 스크립트가 다른 '출처'의 리소스와 상호작용하는 것을 제한합니다. 여기서 '출처'는 프로토콜(http/https), 호스트(도메인 이름), 포트 번호의 세 가지 요소가 모두 같을 때 동일하다고 판단합니다.

예를 들어, `http://www.example.com:8080`에서 로드된 웹 페이지는:

*   `http://www.example.com:8080/api/data` (동일 출처) 에는 접근 가능
*   `http://www.anothersite.com` (다른 호스트) 에는 접근 불가능
*   `https://www.example.com` (다른 프로토콜) 에는 접근 불가능
*   `http://www.example.com:9000` (다른 포트) 에는 접근 불가능

> [!NOTE]
> SOP가 필요한 이유는 악의적인 웹사이트가 여러분의 브라우저를 통해 다른 웹사이트(예: 은행 사이트)에 로그인된 정보를 몰래 가져가는 것을 막기 위함입니다. 이는 사용자의 개인 정보와 보안을 지키는 데 매우 중요한 역할을 합니다.

### 4-2) CORS (Cross-Origin Resource Sharing)

**CORS(Cross-Origin Resource Sharing)**는 동일 출처 정책 때문에 발생하는 제약을 완화하여, 다른 출처의 리소스에 안전하게 접근할 수 있도록 허용하는 메커니즘입니다. 즉, 서버가 '나는 이 출처에서 오는 요청을 허용할게'라고 명시적으로 허락해주는 방식입니다.

웹 페이지에서 다른 도메인(주소)의 서버에 데이터를 요청할 때, 브라우저는 기본적으로 SOP에 따라 요청을 차단합니다. 하지만 서버가 응답 헤더에 `Access-Control-Allow-Origin`과 같은 CORS 관련 정보를 포함하여 특정 출처의 요청을 허용한다고 알려주면, 브라우저는 해당 요청을 허용합니다.

```javascript
// 웹 페이지 (출처: http://localhost:8000)
fetch('http://api.example.com/data') // 다른 출처의 서버에 요청
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('CORS 오류 또는 네트워크 문제:', error));

// 서버 (출처: http://api.example.com)
// 서버는 응답 헤더에 다음을 포함해야 합니다.
// Access-Control-Allow-Origin: http://localhost:8000
// 또는 모든 출처를 허용하려면:
// Access-Control-Allow-Origin: *
```

> [!TIP]
> CORS 오류는 웹 개발을 하면서 자주 만나게 되는 문제입니다. 주로 프론트엔드(브라우저)에서 백엔드(서버)로 데이터를 요청할 때 발생하며, 이 경우 서버 개발자와 협력하여 서버에서 CORS 설정을 올바르게 해주어야 문제를 해결할 수 있습니다.

---

- [목차로 돌아가기](../README.md)
- [이전 강의로 이동](11-ES6-Advanced-Features.md)
- [다음 강의로 이동](13-Basic-DOM-Manipulation.md)
