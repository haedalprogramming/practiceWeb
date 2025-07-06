# 08. CSS 박스 모델 (Box Model)

웹 페이지에 있는 모든 HTML 요소는 사각형의 박스(box) 모양으로 생각할 수 있습니다. CSS 박스 모델은 이 박스의 크기, 여백, 테두리 등을 어떻게 계산하고 배치할지를 정의하는 매우 중요한 개념입니다. 박스 모델을 이해하면 요소의 크기를 정확하게 제어하고 레이아웃을 원하는 대로 구성할 수 있습니다.

## 목차

1. [박스 모델의 구성 요소](#1-박스-모델의-구성-요소)
2. [박스 모델 관련 속성](#2-박스-모델-관련-속성)
3. [박스의 크기를 계산하는 방법: `box-sizing`](#3-박스의-크기를-계산하는-방법-box-sizing)

---

## 1. 박스 모델의 구성 요소

CSS 박스는 네 가지 영역으로 구성됩니다. 안쪽에서부터 바깥쪽으로 다음과 같은 순서로 쌓여있습니다.

![CSS Box Model](./images/boxmodel.gif)

1.  **Content (콘텐츠)**: 텍스트나 이미지 등 요소의 실제 내용이 표시되는 영역입니다. `width`와 `height` 속성으로 크기를 조절할 수 있습니다.
2.  **Padding (패딩)**: 콘텐츠 영역과 테두리(Border) 사이의 안쪽 여백입니다. 요소에 내부적인 공간을 만들어줍니다.
3.  **Border (테두리)**: 패딩을 감싸는 선입니다. 두께, 스타일, 색상을 지정할 수 있습니다.
4.  **Margin (마진)**: 박스의 가장 바깥쪽 영역으로, 테두리 바깥의 여백입니다. 다른 요소와의 간격을 만들 때 사용됩니다.

---

## 2. 박스 모델 관련 속성

### 1) `width`와 `height`

-   콘텐츠 영역의 너비와 높이를 지정합니다.
-   `px`, `%`, `em`, `rem` 등 다양한 단위를 사용할 수 있습니다.

```css
.box {
  width: 200px;
  height: 150px;
}
```

### 2) `padding`

-   콘텐츠와 테두리 사이의 안쪽 여백을 지정합니다.
-   값을 1개부터 4개까지 지정하여 네 방향의 여백을 다르게 설정할 수 있습니다.

```css
.box {
  /* 모든 방향(상하좌우)에 20px 패딩 */
  padding: 20px;

  /* 상하 10px, 좌우 20px 패딩 */
  padding: 10px 20px;

  /* 상 10px, 좌우 20px, 하 30px 패딩 */
  padding: 10px 20px 30px;

  /* 상 10px, 우 20px, 하 30px, 좌 40px 패딩 (시계 방향) */
  padding: 10px 20px 30px 40px;

  /* 특정 방향만 지정 */
  padding-top: 10px;
  padding-right: 20px;
}
```

### 3) `border`

-   테두리의 두께, 스타일, 색상을 한 번에 지정합니다.

```css
.box {
  /* 1px 두께의 실선(solid) 검은색(#000) 테두리 */
  border: 1px solid #000;

  /* 각 속성을 따로 지정할 수도 있음 */
  border-width: 2px;
  border-style: dashed; /* 점선 */
  border-color: blue;
}
```

### 4) `margin`

-   요소의 바깥쪽 여백을 지정합니다.
-   `padding`과 마찬가지로 값을 1개부터 4개까지 지정하여 네 방향의 여백을 설정할 수 있습니다.

```css
.box {
  /* 모든 방향(상하좌우)에 15px 마진 */
  margin: 15px;

  /* 상하 10px, 좌우 20px 마진 */
  margin: 10px 20px;

  /* 특정 방향만 지정 */
  margin-left: 30px;
  margin-bottom: 40px;
}
```

---

## 3. 박스의 크기를 계산하는 방법: `box-sizing`

여기서 한 가지 중요한 문제가 발생합니다. 만약 어떤 요소의 `width`를 `200px`로 설정하고, `padding`을 `20px`, `border`를 `10px`로 주면 이 요소가 화면에서 차지하는 실제 너비는 얼마일까요?

기본적으로 브라우저는 다음과 같이 계산합니다.

> 실제 너비 = `width` + `padding-left` + `padding-right` + `border-left` + `border-right`
> = 200px + 20px + 20px + 10px + 10px = **260px**

이렇게 되면 우리가 `width: 200px`로 지정했음에도 불구하고 실제 크기는 달라져서 레이아웃을 예측하기가 매우 까다로워집니다.

> [!IMPORTANT]
> **`box-sizing: border-box;` 로 해결하기**
> 이 문제를 해결하기 위해 `box-sizing` 속성을 사용합니다. 이 속성의 값을 `border-box`로 설정하면, `width`와 `height`가 콘텐츠 영역만이 아닌 **테두리(border)까지 포함한 크기**를 의미하게 됩니다. 즉, `padding`이나 `border`를 추가해도 박스의 전체 크기는 변하지 않고, 내부의 콘텐츠 영역 크기가 자동으로 줄어듭니다.

**`box-sizing` 속성의 두 가지 값:**

-   `content-box` (기본값): `width`와 `height`가 콘텐츠 영역의 크기만을 의미합니다.
-   `border-box`: `width`와 `height`가 패딩과 테두리를 포함한 전체 박스의 크기를 의미합니다.

**예제 비교:**

```css
.box {
  width: 200px;
  height: 100px;
  padding: 20px;
  border: 10px solid red;
}

.content-box { /* 기본값 */
  box-sizing: content-box;
  /* 최종 너비: 200 + 20*2 + 10*2 = 260px */
}

.border-box {
  box-sizing: border-box;
  /* 최종 너비: 200px (우리가 지정한 값) */
}
```

> [!TIP]
> **모든 요소에 `border-box` 적용하기**
> 레이아웃 작업을 훨씬 직관적으로 만들기 위해, 보통 CSS 파일의 맨 위에 다음과 같이 모든 요소(`*`)에 `box-sizing: border-box;`를 적용하는 코드를 추가합니다. 이렇게 하면 모든 요소의 크기 계산이 동일한 방식으로 이루어져 예측이 쉬워집니다.
> 
> ```css
> * {
>   box-sizing: border-box;
> }
> ```

---

## 정리

CSS 박스 모델은 모든 HTML 요소가 콘텐츠, 패딩, 테두리, 마진으로 구성된 사각형이라는 개념입니다. 각 속성을 이용해 요소의 크기와 여백을 조절할 수 있으며, 특히 `box-sizing: border-box;`를 사용하면 레이아웃을 훨씬 더 직관적이고 예측 가능하게 만들 수 있습니다. 이 박스 모델에 대한 이해는 앞으로 배울 Flexbox 등 복잡한 레이아웃을 다루는 데 필수적인 기초가 됩니다.

---
- [목차로 돌아가기](README.md)
- [이전 강의로 이동](06-CSS-Core-Properties.md)
- [다음 강의로 이동](09-Layout-with-Flexbox.md)
