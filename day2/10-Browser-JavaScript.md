# 10. 브라우저 JavaScript

## 1) 브라우저 환경이란?
- 우리가 인터넷에서 보는 웹페이지(사이트)는 **브라우저**에서 실행됩니다.
- 브라우저는 HTML, CSS, JavaScript 코드를 읽고 화면에 보여줍니다.

## 2) DOM(Document Object Model)
- **DOM**은 웹페이지의 모든 요소(텍스트, 그림, 버튼 등)를 컴퓨터가 다룰 수 있도록 객체 형태로 만든 구조입니다.
- JavaScript로 DOM을 조작하면 웹페이지 내용을 바꿀 수 있습니다.

```html
<!-- 예시 HTML -->
<div id="message">안녕하세요!</div>
<button id="btn">클릭</button>
<script>
  // JavaScript에서 요소 선택
  const msg = document.getElementById('message');
  const btn = document.getElementById('btn');

  // 버튼을 클릭하면 메시지 바꾸기
  btn.addEventListener('click', () => {
    msg.textContent = '버튼을 클릭했어요!';
  });
</script>
```

> [!TIP]
> `document.getElementById('id')` 외에도 `querySelector()`로 CSS 선택자를 사용할 수 있습니다.

## 3) 이벤트 다루기
- 이벤트는 사용자가 클릭, 입력, 마우스 이동 등 **행동**을 했을 때 발생합니다.
- `addEventListener` 메서드로 이벤트를 듣고, 함수(콜백)를 실행합니다.

```javascript
// 입력창에서 글자를 입력할 때마다 콘솔에 출력
const input = document.querySelector('input');
input.addEventListener('input', event => {
  console.log('사용자가 입력한 값:', event.target.value);
});
```

## 4) 브라우저 API
- **fetch()**: 서버에 요청을 보내고, 결과를 받을 때 사용합니다.

```javascript
fetch('https://api.example.com/data')
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error(error));
```

- **localStorage**: 사용자의 브라우저에 데이터를 저장할 수 있습니다.

```javascript
// 저장
localStorage.setItem('username', '철수');

// 불러오기
const name = localStorage.getItem('username');
console.log(name); // '철수'
```

> [!TIP]
> 많은 데이터를 저장하면 브라우저가 느려질 수 있으니, 꼭 필요한 정보만 저장하세요.

## 5) 보안 제약(CORS, Same-Origin Policy)
- 다른 도메인(주소)의 서버에 요청할 때, 브라우저는 **보안** 때문에 제한을 둡니다.
- **CORS** 설정이 되어 있어야 요청이 허용됩니다.

---