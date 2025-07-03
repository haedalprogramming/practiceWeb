# 06. CSS 핵심 속성 (Core Properties)

이전 시간에는 CSS 선택자를 사용하여 HTML 요소를 정확하게 선택하는 방법을 배웠습니다. 이제 선택된 요소에 실제로 스타일을 적용할 때 사용하는 **핵심 속성(Core Properties)**들에 대해 자세히 알아보겠습니다. 이 속성들은 웹 페이지의 시각적인 모습을 결정하는 데 가장 기본적이면서도 중요한 역할을 합니다.

## 목차

1. [텍스트 관련 속성](#1-텍스트-관련-속성)
2. [배경 관련 속성](#2-배경-관련-속성)
3. [테두리 관련 속성](#3-테두리-관련-속성)
4. [크기 관련 속성](#4-크기-관련-속성)
5. [여백 관련 속성](#5-여백-관련-속성)

---

## 1. 텍스트 관련 속성

웹 페이지의 글자 모양을 제어하는 속성들입니다.

### 1) `color`: 글자 색상

-   글자의 색상을 지정합니다.
-   다양한 방식으로 색상을 표현할 수 있습니다. (색상 이름, 16진수 코드, RGB, RGBA 등)

```css
p {
  color: red; /* 색상 이름 */
  color: #FF0000; /* 16진수 코드 */
  color: rgb(255, 0, 0); /* RGB (Red, Green, Blue) */
  color: rgba(255, 0, 0, 0.5); /* RGBA (Red, Green, Blue, Alpha - 투명도) */
}
```

### 2) `font-size`: 글자 크기

-   글자의 크기를 지정합니다.
-   주로 `px` (픽셀), `em`, `rem` 단위를 사용합니다.
    -   `px`: 고정된 크기. 화면 해상도에 따라 상대적으로 작거나 크게 보일 수 있습니다.
    -   `em`: 부모 요소의 `font-size`에 비례하는 상대적인 크기. (예: `1.2em`은 부모 글자 크기의 1.2배)
    -   `rem`: HTML 문서의 최상위 요소인 `<html>`의 `font-size`에 비례하는 상대적인 크기. 반응형 웹 디자인에 유용합니다.

```css
h1 {
  font-size: 32px;
}
p {
  font-size: 16px;
}
.small-text {
  font-size: 0.8em; /* 부모 요소의 폰트 크기보다 20% 작게 */
}
```

### 3) `font-family`: 글꼴

-   글자에 적용할 글꼴을 지정합니다.
-   여러 개의 글꼴을 쉼표(`,`)로 구분하여 나열할 수 있습니다. 브라우저는 앞에서부터 순서대로 글꼴을 찾고, 없으면 다음 글꼴을 사용합니다.
-   마지막에는 항상 `serif`, `sans-serif`, `monospace`와 같은 일반 글꼴 계열을 지정하여, 사용자의 시스템에 지정된 글꼴이 없을 경우에도 적절한 대체 글꼴이 적용되도록 합니다.

```css
body {
  font-family: 'Noto Sans KR', 'Malgun Gothic', sans-serif;
}
```

### 4) `font-weight`: 글자 두께

-   글자의 두께(굵기)를 지정합니다.
-   `normal` (기본값), `bold`, `bolder`, `lighter` 또는 `100`부터 `900`까지의 숫자 값(100단위)을 사용합니다.

```css
h1 {
  font-weight: bold;
}
.thin-text {
  font-weight: 300;
}
```

### 5) `text-align`: 텍스트 정렬

-   텍스트를 요소 내에서 어떻게 정렬할지 지정합니다.
-   `left` (기본값), `right`, `center`, `justify` (양쪽 정렬) 값을 사용합니다.

```css
.center-text {
  text-align: center;
}
```

### 6) `line-height`: 줄 간격

-   텍스트 줄 사이의 간격을 지정합니다.
-   숫자 값(폰트 크기의 배수), `px`, `em`, `%` 등을 사용할 수 있습니다.

```css
p {
  line-height: 1.5; /* 폰트 크기의 1.5배 */
}
```

---

## 2. 배경 관련 속성

요소의 배경을 꾸미는 속성들입니다.

### 1) `background-color`: 배경 색상

-   요소의 배경 색상을 지정합니다.
-   `color` 속성과 동일한 색상 표현 방식을 사용합니다.

```css
.box {
  background-color: #f0f0f0;
}
```

### 2) `background-image`: 배경 이미지

-   요소의 배경으로 이미지를 삽입합니다.
-   `url()` 함수 안에 이미지 파일의 경로를 지정합니다.

```css
.hero-section {
  background-image: url('../images/hero.jpg');
}
```

### 3) `background-repeat`: 배경 이미지 반복

-   배경 이미지를 반복할지 여부와 방법을 지정합니다.
-   `repeat` (기본값), `no-repeat`, `repeat-x` (가로 반복), `repeat-y` (세로 반복) 값을 사용합니다.

```css
.pattern {
  background-image: url('pattern.png');
  background-repeat: repeat-x;
}
```

### 4) `background-position`: 배경 이미지 위치

-   배경 이미지의 시작 위치를 지정합니다.
-   `top`, `bottom`, `left`, `right`, `center` 키워드나 `px`, `%` 값을 조합하여 사용합니다.

```css
.banner {
  background-image: url('banner.jpg');
  background-position: center center; /* 가로, 세로 모두 중앙 */
}
```

### 5) `background-size`: 배경 이미지 크기

-   배경 이미지의 크기를 조절합니다.
-   `auto` (기본값), `cover` (요소를 꽉 채우도록 확대/축소), `contain` (이미지가 잘리지 않도록 최대 크기), `px`, `%` 값을 사용합니다.

```css
.full-bg {
  background-image: url('bg.jpg');
  background-size: cover; /* 배경 이미지가 요소를 꽉 채우도록 */
}
```

---

## 3. 테두리 관련 속성

요소의 테두리를 꾸미는 속성들입니다.

### 1) `border`: 테두리

-   테두리의 두께, 스타일, 색상을 한 번에 지정하는 단축 속성입니다.

```css
.box {
  border: 1px solid black; /* 두께 스타일 색상 */
}
```

### 2) `border-width`: 테두리 두께

-   테두리의 두께를 지정합니다. (예: `1px`, `medium`, `thick`)

### 3) `border-style`: 테두리 스타일

-   테두리의 선 스타일을 지정합니다. (예: `solid`, `dashed`, `dotted`, `double`, `none`)

### 4) `border-color`: 테두리 색상

-   테두리의 색상을 지정합니다.

### 5) `border-radius`: 모서리 둥글게

-   요소의 모서리를 둥글게 만듭니다.
-   값을 `px`나 `%`로 지정합니다. 값을 하나만 주면 네 모서리 모두에 적용됩니다.

```css
.rounded-box {
  border-radius: 10px;
}
.circle {
  width: 100px;
  height: 100px;
  background-color: blue;
  border-radius: 50%; /* 원형으로 만듦 (너비/높이가 같을 때) */
}
```

---

## 4. 크기 및 여백 관련 속성

요소의 크기와 다른 요소와의 간격을 제어하는 속성들입니다.

### 1) `width` / `height`: 너비와 높이

-   요소의 콘텐츠 영역의 너비와 높이를 지정합니다.
-   `px`, `%`, `em`, `rem`, `auto` (기본값) 등을 사용합니다.

```css
.card {
  width: 300px;
  height: 200px;
}
```

### 2) `max-width` / `min-width`: 최대/최소 너비

-   요소의 최대/최소 너비를 지정합니다. 반응형 디자인에 매우 중요합니다.

```css
img {
  max-width: 100%; /* 이미지가 부모 요소보다 커지지 않도록 */
  height: auto; /* 비율 유지 */
}
```

### 3) `margin`: 바깥쪽 여백

-   요소의 테두리 바깥쪽 여백을 지정합니다. 다른 요소와의 간격을 만듭니다.
-   `padding`과 동일하게 1~4개의 값을 사용하여 상하좌우 여백을 설정할 수 있습니다.
-   `margin: 0 auto;`는 블록 요소를 가운데 정렬할 때 자주 사용됩니다.

```css
.button {
  margin: 10px 20px;
}
.container {
  margin: 0 auto; /* 블록 요소를 가운데 정렬 */
}
```

### 4) `padding`: 안쪽 여백

-   요소의 콘텐츠와 테두리 사이의 안쪽 여백을 지정합니다.
-   `margin`과 동일하게 1~4개의 값을 사용하여 상하좌우 여백을 설정할 수 있습니다.

```css
.card {
  padding: 15px;
}
```

---

## 정리

이번 시간에는 CSS에서 가장 자주 사용되는 핵심 속성들을 살펴보았습니다. 텍스트, 배경, 테두리, 크기 및 여백 관련 속성들은 웹 페이지의 시각적인 디자인을 구성하는 데 필수적인 요소들입니다. 이 속성들을 조합하고 적절히 활용하면 여러분이 상상하는 거의 모든 디자인을 웹 페이지에 구현할 수 있습니다. 다음 시간에는 요소들이 화면에 어떻게 배치되는지를 결정하는 `display` 속성에 대해 더 자세히 알아보겠습니다.

---
- [목차로 돌아가기](README.md)
- [이전 강의로 이동](05-CSS-Selectors.md)
- [다음 강의로 이동](07-CSS-Display-Property.md)
