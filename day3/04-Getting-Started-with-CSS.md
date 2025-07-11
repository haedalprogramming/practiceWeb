# 04. CSS 시작하기

지금까지 우리는 HTML을 사용해 웹 페이지의 뼈대와 내용을 구성하는 방법을 배웠습니다. 하지만 내용만으로는 충분하지 않습니다. 사용자에게 더 매력적이고 읽기 쉬운 페이지를 제공하려면 디자인이 필요합니다. 이번 시간부터는 웹 페이지에 옷을 입히고 꾸미는 기술, <strong>CSS(Cascading Style Sheets)</strong>에 대해 알아보겠습니다.

## 목차

1. [CSS란 무엇일까요?](#1-css란-무엇일까요)
2. [CSS 기본 문법](#2-css-기본-문법)
3. [HTML에 CSS를 적용하는 세 가지 방법](#3-html에-css를-적용하는-세-가지-방법)

---

## 1. CSS란 무엇일까요?

CSS는 **Cascading Style Sheets**의 약자로, HTML로 만들어진 문서에 스타일을 적용하기 위한 언어입니다.

- **HTML**이 웹 페이지의 **구조와 내용**을 담당한다면,
- **CSS**는 웹 페이지의 **시각적인 표현**을 담당합니다.

글자의 색상, 크기, 폰트, 배경색, 요소들의 간격과 배치 등 웹 페이지의 거의 모든 디자인적인 요소를 CSS를 통해 제어할 수 있습니다.

> [!IMPORTANT]
> **구조와 디자인의 분리**
> CSS를 사용하는 가장 큰 이유 중 하나는 **관심사의 분리(Separation of Concerns)**입니다. HTML은 내용과 구조에만 집중하고, CSS는 디자인에만 집중하도록 역할을 나누는 것입니다. 이렇게 하면 코드를 이해하고 관리하기가 훨씬 쉬워지며, 여러 페이지에 일관된 디자인을 적용하기도 용이합니다.

---

## 2. CSS 기본 문법

CSS 코드는 매우 간단하고 직관적인 규칙으로 작성됩니다. CSS의 기본 구조는 <strong>선택자(Selector)</strong>와 <strong>선언 블록(Declaration Block)</strong>으로 구성됩니다.

```css
/* h1 태그의 글자 색을 빨간색으로, 글자 크기를 24px로 변경하는 CSS 규칙 */
h1 {
  color: red;
  font-size: 24px;
}
```

- **선택자 (Selector)**: 스타일을 적용하고 싶은 HTML 요소를 지정합니다. 위 예제에서는 `h1`이 선택자입니다.
- **선언 블록 (Declaration Block)**: `{}` 중괄호로 감싸진 영역으로, 하나 이상의 스타일 선언이 들어갑니다.
- **선언 (Declaration)**: `속성: 값;` 형태로 구성되며, 세미콜론(`;`)으로 끝납니다.
  - **속성 (Property)**: 변경하고 싶은 스타일의 종류를 나타냅니다. (예: `color`, `font-size`)
  - **값 (Value)**: 속성에 적용할 구체적인 값을 나타냅니다. (예: `red`, `24px`)

---

## 3. HTML에 CSS를 적용하는 세 가지 방법

HTML 문서에 CSS 스타일을 적용하는 방법은 세 가지가 있습니다.

### 1) 인라인 스타일 (Inline Style)

HTML 태그 안에 `style` 속성을 직접 추가하여 스타일을 적용하는 방식입니다.

```html
<h1 style="color: blue; font-size: 20px;">이것은 인라인 스타일입니다.</h1>
```

- **장점**: 특정 요소 하나에만 스타일을 빠르게 적용하고 싶을 때 편리합니다.
- **단점**: 스타일을 재사용하기 어렵고, HTML 코드와 스타일 코드가 섞여서 가독성이 떨어집니다. **특별한 경우가 아니면 권장하지 않습니다.**

### 2) 내부 스타일 시트 (Internal Style Sheet)

HTML 문서의 `<head>` 태그 안에 `<style>` 태그를 만들어 그 안에 CSS 코드를 작성하는 방식입니다.

```html
<!DOCTYPE html>
<html>
<head>
  <title>내부 스타일 시트</title>
  <style>
    h1 {
      color: green;
    }
    p {
      font-size: 16px;
    }
  </style>
</head>
<body>
  <h1>이것은 내부 스타일 시트입니다.</h1>
  <p>이 스타일은 이 문서 안에서만 적용됩니다.</p>
</body>
</html>
```

- **장점**: 해당 HTML 문서 전체에 일관된 스타일을 적용할 수 있습니다.
- **단점**: 다른 HTML 문서에는 이 스타일을 적용할 수 없습니다.

### 3) 외부 스타일 시트 (External Style Sheet)

CSS 코드를 별도의 `.css` 확장자를 가진 파일로 분리하여 저장하고, HTML 문서에서 `<link>` 태그로 연결하여 사용하는 방식입니다.

> [!TIP]
> **가장 권장되는 방법입니다.** 코드의 재사용성과 유지보수 측면에서 가장 효율적입니다.

**1단계: CSS 파일 생성 (`styles.css`)**

```css
/* styles.css 파일 */
h1 {
  color: navy;
}
p {
  font-size: 18px;
  font-weight: bold;
}
```

**2단계: HTML 파일에서 CSS 파일 연결 (`index.html`)**

`<head>` 태그 안에 `<link>` 태그를 사용하여 외부 CSS 파일을 연결합니다.

```html
<!DOCTYPE html>
<html>
<head>
  <title>외부 스타일 시트</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <h1>이것은 외부 스타일 시트입니다.</h1>
  <p>이 스타일은 styles.css 파일에서 가져왔습니다.</p>
</body>
</html>
```

- `rel="stylesheet"`: 연결할 파일이 스타일 시트임을 명시합니다.
- `href="styles.css"`: CSS 파일의 경로를 지정합니다.

---

## 정리

이번 시간에는 웹 페이지를 꾸미기 위한 언어인 CSS의 기본 개념과 HTML에 적용하는 세 가지 방법에 대해 배웠습니다. 앞으로 우리는 주로 **외부 스타일 시트** 방식을 사용하여 HTML의 구조와 CSS의 디자인을 분리하여 코드를 작성하게 될 것입니다. 다음 시간에는 스타일을 적용할 요소를 더 정교하게 선택하는 방법인 **CSS 선택자**에 대해 자세히 알아보겠습니다.

---
- [목차로 돌아가기](README.md)
- [이전 강의로 이동](03-Semantic-HTML.md)
- [다음 강의로 이동](05-CSS-Selectors.md)
