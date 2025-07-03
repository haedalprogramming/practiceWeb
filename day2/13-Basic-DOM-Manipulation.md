# 13. DOM(문서 객체 모델) 기본 조작

**DOM(Document Object Model)**은 웹 페이지의 모든 내용을 컴퓨터가 이해하고 JavaScript로 조작할 수 있도록 '객체 형태로 만든 구조'입니다. 마치 웹 페이지를 구성하는 모든 요소(텍스트, 이미지, 버튼, 링크 등)들이 각각의 부품처럼 객체로 표현되고, 이 객체들이 나무(트리) 구조로 연결되어 있는 것과 같습니다.

JavaScript는 이 DOM을 통해 웹 페이지의 내용을 동적으로 변경할 수 있습니다. 예를 들어, 버튼을 클릭하면 텍스트를 바꾸거나, 새로운 이미지를 추가하거나, 스타일을 변경하는 등의 작업을 할 수 있습니다.

## 1) DOM 요소 선택하기

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

---

- [목차로 돌아가기](../README.md)
- [이전 강의로 이동](12-Browser-JavaScript.md)
- [다음 강의로 이동](14-Basic-Event-Handling.md)
