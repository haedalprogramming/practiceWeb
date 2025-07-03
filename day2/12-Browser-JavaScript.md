# 11. 브라우저 JavaScript

## 1) 브라우저 환경이란?

우리가 인터넷에서 보는 모든 웹 페이지(사이트)는 **웹 브라우저**라는 특별한 프로그램 안에서 실행됩니다. 웹 브라우저(예: Chrome, Firefox, Safari, Edge)는 단순히 웹 페이지를 보여주는 것을 넘어, HTML, CSS, JavaScript 코드를 해석하고 실행하여 우리가 보고 상호작용할 수 있는 형태로 만들어주는 역할을 합니다.

웹 페이지는 크게 세 가지 요소로 구성됩니다:

*   **HTML (HyperText Markup Language)**: 웹 페이지의 뼈대와 내용을 구성합니다. 제목, 문단, 이미지, 링크 등 웹 페이지에 들어갈 모든 요소들을 정의합니다.
*   **CSS (Cascading Style Sheets)**: 웹 페이지의 디자인과 스타일을 담당합니다. 글자 색깔, 크기, 배경 이미지, 레이아웃 등 웹 페이지를 예쁘게 꾸미는 역할을 합니다.
*   **JavaScript**: 웹 페이지에 동적인 기능과 상호작용을 추가합니다. 버튼을 클릭했을 때 메뉴가 열리거나, 이미지가 슬라이드처럼 움직이는 등 사용자의 행동에 반응하는 모든 기능은 JavaScript를 통해 구현됩니다.

브라우저는 이 세 가지 언어를 모두 이해하고 실행하여, 정적인 문서였던 웹 페이지를 살아있는 애플리케이션처럼 만들어줍니다. JavaScript는 브라우저 안에서 웹 페이지의 HTML과 CSS를 조작하고, 사용자 이벤트를 처리하며, 서버와 통신하는 등 다양한 작업을 수행할 수 있습니다.

## 2) DOM(Document Object Model)

**DOM(Document Object Model)**은 웹 페이지의 모든 내용을 컴퓨터가 이해하고 JavaScript로 조작할 수 있도록 '객체 형태로 만든 구조'입니다. 마치 웹 페이지를 구성하는 모든 요소(텍스트, 이미지, 버튼, 링크 등)들이 각각의 부품처럼 객체로 표현되고, 이 객체들이 나무(트리) 구조로 연결되어 있는 것과 같습니다.

JavaScript는 이 DOM을 통해 웹 페이지의 내용을 동적으로 변경할 수 있습니다. 예를 들어, 버튼을 클릭하면 텍스트를 바꾸거나, 새로운 이미지를 추가하거나, 스타일을 변경하는 등의 작업을 할 수 있습니다.

### DOM 요소 선택하기

DOM을 조작하려면 먼저 조작하고 싶은 웹 페이지의 요소를 선택해야 합니다. JavaScript는 다양한 방법으로 DOM 요소를 선택할 수 있는 기능을 제공합니다.

*   **`document.getElementById('id')`**: HTML 요소의 `id` 속성을 사용하여 요소를 선택합니다. `id`는 웹 페이지에서 유일해야 합니다.
*   **`document.querySelector('CSS 선택자')`**: CSS 선택자를 사용하여 요소를 선택합니다. 가장 먼저 일치하는 요소 하나를 반환합니다.
*   **`document.querySelectorAll('CSS 선택자')`**: CSS 선택자와 일치하는 모든 요소를 배열과 비슷한 형태로 반환합니다.

```html
<!-- index.html 파일 -->
<div id="message">안녕하세요!</div>
<button class="my-button" id="btn">클릭</button>
<p class="item">첫 번째 아이템</p>
<p class="item">두 번째 아이템</p>

<script>
  // JavaScript에서 요소 선택
  const messageDiv = document.getElementById('message'); // id가 message인 div 요소 선택
  const clickButton = document.querySelector('#btn');    // id가 btn인 버튼 요소 선택
  const firstItem = document.querySelector('.item');     // class가 item인 첫 번째 p 요소 선택
  const allItems = document.querySelectorAll('.item');   // class가 item인 모든 p 요소 선택

  console.log(messageDiv.textContent); // 출력: 안녕하세요!
  console.log(firstItem.textContent);  // 출력: 첫 번째 아이템
  console.log(allItems.length);      // 출력: 2

  // 버튼을 클릭하면 메시지 바꾸기
  clickButton.addEventListener('click', () => {
    messageDiv.textContent = '버튼을 클릭했어요!'; // div의 텍스트 내용을 변경합니다.
    messageDiv.style.color = 'blue';           // div의 글자 색상을 파란색으로 변경합니다.
    firstItem.classList.add('highlight');      // 첫 번째 p 요소에 highlight 클래스를 추가합니다.
  });
</script>
```

> [!TIP]
> `textContent`는 요소 안의 텍스트 내용을 가져오거나 설정할 때 사용하고, `innerHTML`은 요소 안의 HTML 내용을 가져오거나 설정할 때 사용합니다. `innerHTML`은 HTML 태그를 포함하여 내용을 변경할 수 있지만, 보안에 취약할 수 있으므로 주의해서 사용해야 합니다.

## 3) 이벤트 다루기

웹 페이지는 사용자의 행동에 반응하여 동적으로 변화합니다. 이때 사용자의 행동(클릭, 키보드 입력, 마우스 이동 등)이나 브라우저에서 발생하는 특정 상황을 '이벤트(Event)'라고 합니다. JavaScript는 이러한 이벤트를 감지하고, 이벤트가 발생했을 때 특정 코드를 실행하도록 할 수 있습니다.

### 이벤트 리스너 등록하기

이벤트를 감지하고 처리하려면 `addEventListener()` 메서드를 사용합니다. 이 메서드는 특정 HTML 요소에 '이벤트 리스너(Event Listener)'를 등록하여, 해당 이벤트가 발생했을 때 미리 정의된 함수(콜백 함수)를 실행하도록 합니다.

```javascript
// HTML 요소 가져오기
const myButton = document.querySelector('#myButton');
const myInput = document.querySelector('#myInput');

// 1. 클릭(click) 이벤트
// 버튼을 클릭했을 때 메시지를 출력합니다.
myButton.addEventListener('click', () => {
  console.log('버튼이 클릭되었습니다!');
  alert('버튼 클릭!');
});

// 2. 입력(input) 이벤트
// 입력창에 글자를 입력할 때마다 콘솔에 현재 입력된 값을 출력합니다.
myInput.addEventListener('input', event => {
  console.log('사용자가 입력한 값:', event.target.value);
});

// 3. 마우스 오버(mouseover) / 마우스 아웃(mouseout) 이벤트
// 특정 요소에 마우스를 올리거나 내릴 때 스타일을 변경합니다.
const hoverDiv = document.querySelector('#hoverDiv');
hoverDiv.addEventListener('mouseover', () => {
  hoverDiv.style.backgroundColor = 'yellow';
});
hoverDiv.addEventListener('mouseout', () => {
  hoverDiv.style.backgroundColor = 'lightgray';
});
```

```html
<!-- 위 JavaScript 코드와 함께 사용할 HTML 예시 -->
<button id="myButton">클릭하세요</button>
<input type="text" id="myInput" placeholder="여기에 입력하세요">
<div id="hoverDiv" style="width: 100px; height: 50px; background-color: lightgray; margin-top: 10px;">마우스를 올려보세요</div>
```

> [!NOTE]
> `event` 객체는 이벤트에 대한 다양한 정보를 담고 있습니다. 예를 들어, `event.target`은 이벤트가 발생한 HTML 요소를 가리키고, `event.target.value`는 입력 요소의 현재 값을 가져올 때 사용됩니다. 이벤트는 웹 페이지를 동적이고 상호작용적으로 만드는 핵심 요소입니다.

---

## 4) 브라우저 API

브라우저는 웹 페이지에서 다양한 기능을 수행할 수 있도록 여러 가지 내장된 API(Application Programming Interface)를 제공합니다. API는 프로그램들이 서로 통신하고 기능을 주고받을 수 있도록 정의된 규칙들의 집합입니다. 여기서는 웹 개발에서 자주 사용되는 몇 가지 브라우저 API를 소개합니다.

### 4-1) `fetch()` API (네트워크 요청)

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

### 4-2) `localStorage`와 `sessionStorage` (웹 스토리지)

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

### 4-3) Geolocation API (위치 정보)

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

## 5) 보안 제약(CORS, Same-Origin Policy)

웹 브라우저는 사용자의 보안과 개인 정보 보호를 위해 여러 가지 보안 제약을 두고 있습니다. 이 중 가장 중요하고 자주 접하게 되는 것이 '동일 출처 정책(Same-Origin Policy)'과 'CORS(Cross-Origin Resource Sharing)'입니다.

### 5-1) 동일 출처 정책 (Same-Origin Policy, SOP)

**동일 출처 정책(SOP)**은 웹 브라우저의 가장 기본적인 보안 정책입니다. 이 정책은 한 '출처(Origin)'에서 로드된 문서나 스크립트가 다른 '출처'의 리소스와 상호작용하는 것을 제한합니다. 여기서 '출처'는 프로토콜(http/https), 호스트(도메인 이름), 포트 번호의 세 가지 요소가 모두 같을 때 동일하다고 판단합니다.

예를 들어, `http://www.example.com:8080`에서 로드된 웹 페이지는:

*   `http://www.example.com:8080/api/data` (동일 출처) 에는 접근 가능
*   `http://www.anothersite.com` (다른 호스트) 에는 접근 불가능
*   `https://www.example.com` (다른 프로토콜) 에는 접근 불가능
*   `http://www.example.com:9000` (다른 포트) 에는 접근 불가능

> [!NOTE]
> SOP가 필요한 이유는 악의적인 웹사이트가 여러분의 브라우저를 통해 다른 웹사이트(예: 은행 사이트)에 로그인된 정보를 몰래 가져가는 것을 막기 위함입니다. 이는 사용자의 개인 정보와 보안을 지키는 데 매우 중요한 역할을 합니다.

### 5-2) CORS (Cross-Origin Resource Sharing)

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
- [이전 강의로 이동](10-ES6-Advanced-Features.md)
- [다음 강의로 이동](12-Node.js-JavaScript.md)
