# 14. 이벤트 처리 기초

웹 페이지는 사용자의 행동에 반응하여 동적으로 변화합니다. 이때 사용자의 행동(클릭, 키보드 입력, 마우스 이동 등)이나 브라우저에서 발생하는 특정 상황을 '이벤트(Event)'라고 합니다. JavaScript는 이러한 이벤트를 감지하고, 이벤트가 발생했을 때 특정 코드를 실행하도록 할 수 있습니다.

## 1) 이벤트 리스너 등록하기

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

- [목차로 돌아가기](../README.md)
- [이전 강의로 이동](13-Basic-DOM-Manipulation.md)
- [다음 강의로 이동](15-Node.js-JavaScript.md)
