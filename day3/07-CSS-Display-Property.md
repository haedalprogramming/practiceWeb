# 07. Display 속성: block, inline, inline-block

우리가 웹 페이지에서 보는 모든 HTML 요소는 각자 화면에 표시되는 방식이 정해져 있습니다. 어떤 요소는 한 줄을 전부 차지하고, 어떤 요소는 자기 내용만큼의 공간만 차지하고 옆으로 다른 요소가 올 수 있습니다. 이처럼 요소가 화면에 어떻게 보여지고 배치되는지를 결정하는 핵심적인 속성이 바로 `display`입니다. `display` 속성을 이해하는 것은 CSS 레이아웃의 기초를 다지는 데 매우 중요합니다.

## 목차

1. [`display` 속성이란?](#1-display-속성이란)
2. [`display: block`](#2-display-block)
3. [`display: inline`](#3-display-inline)
4. [`display: inline-block`](#4-display-inline-block)

---

## 1. `display` 속성이란?

`display` 속성은 HTML 요소를 브라우저가 어떻게 렌더링(화면에 그리는 것)할지, 그리고 주변 요소들과 어떻게 상호작용할지를 결정합니다. 이 속성의 가장 기본적이고 중요한 값은 `block`, `inline`, `inline-block` 세 가지입니다.

---

## 2. `display: block`

`block`은 "벽돌"을 생각하면 이해하기 쉽습니다. 벽돌처럼 한 줄의 공간을 모두 차지하고, 다른 요소들을 아래로 밀어냅니다.

-   **특징:**
    1.  **항상 새로운 줄에서 시작**하며, 양옆에 다른 요소가 올 수 없습니다.
    2.  **너비(`width`)와 높이(`height`)를 지정할 수 있습니다.** 지정하지 않으면 너비는 부모 요소의 100%를 차지합니다.
    3.  **모든 `margin`과 `padding` 속성을 적용할 수 있습니다.** (상, 하, 좌, 우 모두 가능)

-   **대표적인 `block` 요소:**
    -   `<div>`, `<p>`, `<h1>`~`<h6>`, `<ul>`, `<li>`, `<form>`, `<header>`, `<footer>`, `<section>` 등

**예제:**
```html
<div style="background-color: lightblue; border: 1px solid blue;">
  저는 block 요소입니다. 한 줄을 모두 차지하죠.
</div>
<div style="background-color: lightcoral; border: 1px solid red; width: 200px;">
  저도 block 요소입니다. 너비를 200px로 지정했습니다.
</div>
```

---

## 3. `display: inline`

`inline`은 "글자"처럼 동작합니다. 문장 안의 단어들처럼, 자기 내용만큼의 공간만 차지하고 줄을 바꾸지 않으며 옆으로 다른 요소가 올 수 있습니다.

-   **특징:**
    1.  **줄을 바꾸지 않고** 다른 인라인 요소들과 같은 줄에 나란히 배치됩니다.
    2.  **너비(`width`)와 높이(`height`)를 지정할 수 없습니다.** 콘텐츠의 크기가 곧 요소의 크기가 됩니다.
    3.  **`margin`은 좌우만, `padding`은 상하좌우 모두 적용되지만,** 상하 `padding`은 다른 요소의 레이아웃에 영향을 주지 않을 수 있습니다.

-   **대표적인 `inline` 요소:**
    -   `<span>`, `<a>`, `<img>`, `<strong>`, `<em>`, `<input>`, `<button>` 등

**예제:**
```html
<span style="background-color: lightgreen; border: 1px solid green;">
  저는 inline 요소입니다.
</span>
<strong style="background-color: yellow; border: 1px solid orange;">
  저도 inline 요소라서 옆에 붙을 수 있죠.
</strong>
<a href="#">이것도 inline 요소입니다.</a>
```

> [!CAUTION]
> **`inline` 요소의 한계**
> `inline` 요소는 너비와 높이를 가질 수 없고, 상하 `margin`이 제대로 동작하지 않기 때문에 레이아웃을 제어하는 데 한계가 있습니다. 단순히 텍스트의 일부에 스타일을 주거나, 문장 중간에 링크를 넣는 등의 제한적인 용도로 사용됩니다.

---

## 4. `display: inline-block`

`inline-block`은 이름 그대로 `inline`과 `block`의 특징을 모두 가진 하이브리드입니다. 레이아웃을 만들 때 매우 유용하게 사용됩니다.

-   **특징:**
    1.  **`inline`처럼** 줄을 바꾸지 않고 다른 요소와 한 줄에 나란히 배치됩니다.
    2.  **`block`처럼** 너비(`width`)와 높이(`height`)를 지정할 수 있습니다.
    3.  **`block`처럼** 모든 `margin`과 `padding` 속성을 완벽하게 적용할 수 있습니다.

**예제:**
```html
<div style="display: inline-block; background-color: pink; border: 1px solid hotpink; width: 150px; height: 100px; margin: 10px;">
  저는 inline-block 입니다. 너비와 높이를 가질 수 있고, 옆에 다른 요소도 올 수 있죠.
</div>
<div style="display: inline-block; background-color: violet; border: 1px solid purple; width: 150px; height: 100px; margin: 10px;">
  저도 inline-block 입니다.
</div>
```

> [!TIP]
> **언제 `inline-block`을 사용할까?**
> 여러 개의 박스를 가로로 나란히 배치하고 싶지만, 각 박스의 크기와 여백을 자유롭게 제어하고 싶을 때 `inline-block`은 훌륭한 선택입니다. (물론, 더 복잡하고 유연한 레이아웃을 위해서는 다음에 배울 Flexbox가 더 강력합니다.)

---

## 정리

`display` 속성은 CSS 레이아웃의 기본입니다. 각 요소가 화면에 어떻게 그려지는지를 결정하며, 이들의 차이점을 명확히 아는 것이 중요합니다.

-   **`block`**: 한 줄 전체를 차지하는 벽돌.
-   **`inline`**: 줄을 바꾸지 않는 글자.
-   **`inline-block`**: 글자처럼 배치되지만, 크기와 여백을 가질 수 있는 만능 박스.

이 세 가지 `display` 값의 특징을 잘 이해하고 나면, 왜 어떤 요소는 크기가 변하고 어떤 요소는 변하지 않는지에 대한 의문이 풀릴 것입니다. 이제 다음 시간에는 이 박스들의 구체적인 크기를 계산하는 방법인 **CSS 박스 모델**에 대해 알아보겠습니다.

---
- [목차로 돌아가기](README.md)
- [이전 강의로 이동](06-CSS-Core-Properties.md)
- [다음 강의로 이동](08-CSS-Box-Model.md)
