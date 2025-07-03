# 12. 가상 클래스로 동적인 효과 주기 (Pseudo-classes)

지금까지 우리는 HTML 요소를 선택하여 스타일을 적용하는 다양한 방법을 배웠습니다. 하지만 사용자가 마우스를 올리거나, 링크를 방문했거나, 입력 필드에 초점을 맞추는 등 **특정 상태**에 있을 때만 스타일을 변경하고 싶다면 어떻게 해야 할까요? 이때 사용하는 것이 바로 **가상 클래스(Pseudo-classes)**입니다. 가상 클래스는 요소의 특별한 상태나 위치에 따라 스타일을 적용할 수 있게 해주는 강력한 CSS 기능입니다.

## 목차

1. [가상 클래스란 무엇일까요?](#1-가상-클래스란-무엇일까요)
2. [자주 사용하는 가상 클래스](#2-자주-사용하는-가상-클래스)

---

## 1. 가상 클래스란 무엇일까요?

가상 클래스는 선택자에 `:` (콜론)을 붙여 사용하며, 요소의 **특정 상태**나 **특정 위치**에 따라 스타일을 적용할 수 있도록 해줍니다. 실제 HTML 문서에는 존재하지 않지만, CSS가 가상으로 만들어낸 클래스라고 생각할 수 있습니다.

**기본 문법:**

```css
선택자:가상클래스명 {
  속성: 값;
}
```

---

## 2. 자주 사용하는 가상 클래스

### 1) 링크 관련 가상 클래스

링크(`<a>` 태그)는 사용자의 행동에 따라 다양한 상태를 가질 수 있으며, 각 상태에 따라 다른 스타일을 적용할 수 있습니다.

-   `:link`: 아직 방문하지 않은 링크
-   `:visited`: 이미 방문한 링크
-   `:hover`: 마우스 커서가 요소 위에 올라갔을 때
-   `:active`: 요소를 클릭하는 순간 (마우스를 누르고 있는 동안)

**예제:**

```html
<a href="#">링크 테스트</a>
```

```css
/* 방문하지 않은 링크는 파란색 */
a:link {
  color: blue;
}

/* 방문한 링크는 보라색 */
a:visited {
  color: purple;
}

/* 마우스를 올리면 빨간색으로 변하고 밑줄 제거 */
a:hover {
  color: red;
  text-decoration: none;
}

/* 클릭하는 순간 초록색 */
a:active {
  color: green;
}
```

> [!TIP]
> **`:hover` 활용:**
> `:hover`는 링크뿐만 아니라 거의 모든 HTML 요소에 적용하여 마우스를 올렸을 때 시각적인 피드백을 줄 수 있습니다. 버튼의 배경색을 바꾸거나, 이미지에 그림자를 넣는 등 다양한 인터랙티브 효과를 만들 때 유용합니다.

### 2) 사용자 입력 관련 가상 클래스

폼(`form`) 요소나 입력 필드(`input`)의 상태에 따라 스타일을 변경할 수 있습니다.

-   `:focus`: 요소에 초점(커서)이 맞춰졌을 때 (클릭하거나 탭 키로 이동했을 때)
-   `:checked`: 체크박스나 라디오 버튼이 선택되었을 때
-   `:disabled`: 요소가 비활성화되었을 때

**예제:**

```html
<input type="text" placeholder="여기에 입력하세요">
<input type="checkbox" id="agree" checked>
<label for="agree">동의합니다.</label>
<button disabled>비활성화 버튼</button>
```

```css
/* 입력 필드에 초점이 맞춰지면 테두리 색 변경 */
input:focus {
  border-color: blue;
  outline: none; /* 기본 파란색 테두리 제거 */
}

/* 체크박스가 선택되면 라벨 글자색 변경 */
input[type="checkbox"]:checked + label {
  color: green;
}

/* 비활성화된 버튼의 배경색 변경 */
button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}
```

### 3) 구조적 가상 클래스

요소의 위치나 순서에 따라 스타일을 적용할 수 있습니다.

-   `:first-child`: 부모 요소의 첫 번째 자식 요소
-   `:last-child`: 부모 요소의 마지막 자식 요소
-   `:nth-child(n)`: 부모 요소의 n번째 자식 요소 (n은 숫자, `odd`, `even` 등)

**예제:**

```html
<ul>
  <li>첫 번째 아이템</li>
  <li>두 번째 아이템</li>
  <li>세 번째 아이템</li>
</ul>
```

```css
/* 첫 번째 li 아이템의 글자색 변경 */
li:first-child {
  color: red;
}

/* 마지막 li 아이템의 글자색 변경 */
li:last-child {
  color: blue;
}

/* 홀수 번째 li 아이템의 배경색 변경 */
li:nth-child(odd) {
  background-color: #f0f0f0;
}

/* 2번째 li 아이템의 글자 크기 변경 */
li:nth-child(2) {
  font-size: 20px;
}
```

---

## 정리

가상 클래스는 CSS만으로도 웹 페이지에 동적이고 인터랙티브한 효과를 줄 수 있게 해주는 강력한 도구입니다. 사용자의 행동에 반응하거나, 요소의 특정 상태에 따라 스타일을 변경할 때 매우 유용하게 사용됩니다. `:hover`, `:focus`, `:nth-child` 등 자주 사용되는 가상 클래스들을 익혀두면 웹 페이지를 더욱 풍부하게 만들 수 있습니다.

---
- [목차로 돌아가기](README.md)
- [이전 강의로 이동](11-CSS-Specificity-and-Inheritance.md)
- [다음 강의로 이동](Lab2-Styling-Profile-CSS.md)
