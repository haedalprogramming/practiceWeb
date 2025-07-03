# 10. CSS Position 속성

지금까지 우리는 `display` 속성과 Flexbox를 사용하여 웹 페이지의 요소들을 배치하는 방법을 배웠습니다. 하지만 때로는 요소들을 일반적인 흐름에서 벗어나 특정 위치에 정확히 배치하거나, 다른 요소 위에 겹치게 하거나, 스크롤에 따라 고정시키는 등의 고급 레이아웃 제어가 필요할 때가 있습니다. 이때 사용하는 것이 바로 CSS의 **`position` 속성**입니다.

## 목차

1. [`position` 속성이란?](#1-position-속성이란)
2. [`position: static`](#2-position-static)
3. [`position: relative`](#3-position-relative)
4. [`position: absolute`](#4-position-absolute)
5. [`position: fixed`](#5-position-fixed)
6. [`position: sticky`](#6-position-sticky)

---

## 1. `position` 속성이란?

`position` 속성은 HTML 요소가 문서 내에서 어떻게 배치될지 결정합니다. 이 속성은 주로 `top`, `right`, `bottom`, `left` 속성과 함께 사용되어 요소의 최종 위치를 지정합니다.

`position` 속성에는 다음과 같은 주요 값들이 있습니다.

*   `static` (기본값)
*   `relative`
*   `absolute`
*   `fixed`
*   `sticky`

---

## 2. `position: static`

`static`은 `position` 속성의 **기본값**입니다. 요소는 문서의 일반적인 흐름(Normal Flow)에 따라 배치됩니다. `top`, `right`, `bottom`, `left` 속성은 `static`으로 설정된 요소에는 아무런 영향을 주지 않습니다.

```css
.box {
  position: static; /* 기본값이므로 명시적으로 선언할 필요는 거의 없음 */
}
```

---

## 3. `position: relative`

`relative`는 요소를 문서의 일반적인 흐름에 따라 배치하지만, `top`, `right`, `bottom`, `left` 속성을 사용하여 **원래 자신이 있어야 할 위치를 기준으로** 상대적으로 이동시킬 수 있습니다.

-   **특징:**
    -   요소가 이동하더라도, 원래 차지하고 있던 공간은 그대로 유지됩니다. 다른 요소들은 그 공간이 비어있다고 생각하고 배치됩니다.
    -   `absolute`나 `fixed` 자식 요소의 **기준점**이 될 수 있습니다.

**예제:**

```html
<div class="container">
  <div class="box relative-box">Relative Box</div>
  <div class="box">Normal Box</div>
</div>
```

```css
.container {
  border: 2px solid gray;
  padding: 20px;
}
.box {
  width: 100px;
  height: 100px;
  background-color: lightblue;
  border: 1px solid blue;
  margin: 10px;
}
.relative-box {
  position: relative;
  top: 20px;   /* 원래 위치에서 아래로 20px 이동 */
  left: 30px;  /* 원래 위치에서 오른쪽으로 30px 이동 */
  background-color: lightgreen;
}
```

---

## 4. `position: absolute`

`absolute`는 요소를 문서의 일반적인 흐름에서 **완전히 제거**합니다. 그리고 가장 가까운 **`position` 속성이 `static`이 아닌 부모 요소**를 기준으로 위치를 지정합니다. 만약 `static`이 아닌 부모 요소가 없다면, `<body>` 태그를 기준으로 위치하게 됩니다.

-   **특징:**
    -   요소가 일반적인 흐름에서 제거되므로, 원래 차지하고 있던 공간이 사라집니다. 다른 요소들은 그 공간이 없다고 생각하고 배치됩니다.
    -   `top`, `right`, `bottom`, `left` 속성을 사용하여 정확한 위치를 지정합니다.

**예제:**

```html
<div class="container relative-parent">
  <div class="box absolute-box">Absolute Box</div>
  <div class="box">Normal Box</div>
</div>
<div class="box">Another Normal Box</div>
```

```css
.container {
  border: 2px solid gray;
  padding: 50px;
  position: relative; /* absolute 자식의 기준점이 됨 */
}
.box {
  width: 100px;
  height: 100px;
  background-color: lightblue;
  border: 1px solid blue;
  margin: 10px;
}
.absolute-box {
  position: absolute;
  top: 0;    /* 부모 요소의 상단에 붙음 */
  right: 0;  /* 부모 요소의 오른쪽에 붙음 */
  background-color: lightcoral;
}
```

> [!IMPORTANT]
> **`absolute` 사용 시 주의사항:**
> `absolute` 요소를 사용할 때는 반드시 **기준이 될 부모 요소에 `position: relative;`를 부여**하는 것이 일반적인 패턴입니다. 그렇지 않으면 `absolute` 요소가 원치 않는 `<body>`를 기준으로 움직이게 되어 레이아웃이 깨질 수 있습니다.

---

## 5. `position: fixed`

`fixed`는 요소를 문서의 일반적인 흐름에서 **완전히 제거**하고, **뷰포트(Viewport, 현재 화면)**를 기준으로 위치를 지정합니다. 스크롤을 해도 요소는 항상 같은 위치에 고정되어 있습니다.

-   **특징:**
    -   주로 상단 고정 메뉴, 하단 고정 버튼, 팝업 창 등에 사용됩니다.
    -   `top`, `right`, `bottom`, `left` 속성을 사용하여 위치를 지정합니다.

**예제:**

```html
<body>
  <div class="fixed-button">맨 위로</div>
  <!-- 페이지 내용이 길어서 스크롤이 생기도록 -->
  <div style="height: 1500px;">스크롤 가능한 내용</div>
</body>
```

```css
.fixed-button {
  position: fixed;
  bottom: 20px;  /* 화면 하단에서 20px 위 */
  right: 20px;   /* 화면 오른쪽에서 20px 왼쪽 */
  background-color: #007bff;
  color: white;
  padding: 10px 15px;
  border-radius: 5px;
  cursor: pointer;
  z-index: 1000; /* 다른 요소 위에 표시되도록 */
}
```

---

## 6. `position: sticky`

`sticky`는 `relative`와 `fixed`의 중간 형태입니다. 요소는 일반적인 흐름에 따라 배치되지만, 스크롤 위치가 특정 임계값에 도달하면 `fixed`처럼 동작하여 화면에 고정됩니다.

-   **특징:**
    -   주로 스크롤에 따라 상단에 고정되는 헤더나 사이드바 등에 사용됩니다.
    -   `top`, `right`, `bottom`, `left` 중 하나 이상을 반드시 지정해야 합니다.

**예제:**

```html
<body>
  <header style="height: 100px; background-color: #eee;">페이지 상단</header>
  <nav class="sticky-nav">
    <ul>
      <li>메뉴 1</li>
      <li>메뉴 2</li>
      <li>메뉴 3</li>
    </ul>
  </nav>
  <div style="height: 1000px; background-color: lightyellow;">스크롤 가능한 내용</div>
  <div style="height: 500px; background-color: lightblue;">더 많은 내용</div>
</body>
```

```css
.sticky-nav {
  position: sticky;
  top: 0; /* 뷰포트 상단에 닿으면 고정 */
  background-color: #333;
  color: white;
  padding: 10px;
  z-index: 999;
}
.sticky-nav ul {
  list-style: none;
  display: flex;
  justify-content: space-around;
}
```

---

## 정리

`position` 속성은 웹 페이지의 요소들을 정교하게 배치하고 동적인 효과를 주는 데 필수적인 도구입니다. 각 값(`static`, `relative`, `absolute`, `fixed`, `sticky`)의 특징과 기준점을 명확히 이해하는 것이 중요합니다. 특히 `relative`와 `absolute`의 관계, 그리고 `fixed`와 `sticky`의 차이점을 잘 파악하여 적절한 상황에 활용할 수 있도록 연습해야 합니다.

---
- [목차로 돌아가기](README.md)
- [이전 강의로 이동](09-Layout-with-Flexbox.md)
- [다음 강의로 이동](11-CSS-Specificity-and-Inheritance.md)
