# 09. Flexbox로 레이아웃 잡기

지금까지 우리는 CSS 박스 모델을 통해 각 요소가 차지하는 공간에 대해 배웠습니다. 하지만 여러 개의 박스를 우리가 원하는 대로 가로로 배치하거나, 간격을 맞추거나, 정렬하는 것은 기존의 방식으로는 꽤 복잡했습니다. 이제 웹 레이아웃의 혁신이라고 불리는 **Flexbox**를 배워보겠습니다. Flexbox를 사용하면 이러한 작업들을 매우 간단하고 유연하게 처리할 수 있습니다.

## 목차

1. [Flexbox란 무엇일까요?](#1-flexbox란-무엇일까요)
2. [Flexbox 시작하기: `display: flex`](#2-flexbox-시작하기-display-flex)
3. [Flex Container에 적용하는 속성](#3-flex-container에-적용하는-속성)
4. [Flex Items에 적용하는 속성](#4-flex-items에-적용하는-속성)

---

## 1. Flexbox란 무엇일까요?

Flexbox(Flexible Box Layout)는 CSS3에 도입된 새로운 레이아웃 모델입니다. 이름에서 알 수 있듯이, **유연하게(Flexible)** 박스들을 배치하고 정렬하는 데 특화되어 있습니다. Flexbox는 주로 **한 방향(1차원)의 레이아웃**을 구성하는 데 사용됩니다. 예를 들어, 아이템들을 가로 또는 세로로 나란히 정렬하는 경우에 매우 강력한 기능을 제공합니다.

> [!IMPORTANT]
> **Flexbox의 핵심: Container와 Items**
> Flexbox 레이아웃은 두 가지 핵심 요소로 구성됩니다.
> 1.  **Flex Container (플렉스 컨테이너)**: 아이템들을 감싸는 부모 요소입니다.
> 2.  **Flex Items (플렉스 아이템)**: 컨테이너 안에 있는 자식 요소들입니다.
> 
> Flexbox의 모든 속성은 컨테이너에 적용하는 속성과 아이템에 적용하는 속성으로 나뉩니다.

---

## 2. Flexbox 시작하기: `display: flex`

Flexbox를 사용하려면, 먼저 부모 요소(컨테이너)에 `display: flex;` 속성을 적용해야 합니다. 이 한 줄의 코드만으로 마법이 시작됩니다.

**HTML:**
```html
<div class="container">
  <div class="item">1</div>
  <div class="item">2</div>
  <div class="item">3</div>
</div>
```

**CSS:**
```css
.container {
  display: flex;
  border: 2px solid black;
}

.item {
  background-color: skyblue;
  padding: 20px;
  margin: 10px;
}
```

위 코드를 적용하면, 원래 세로로 쌓이던 `<div>` 아이템들이 **가로로 나란히 배치**되는 것을 볼 수 있습니다. `display: flex;`를 적용하는 순간, 자식 요소들(Flex Items)은 더 이상 블록 레벨 요소처럼 동작하지 않고 Flexbox의 규칙을 따르게 됩니다.

---

## 3. Flex Container에 적용하는 속성

컨테이너에 적용하는 속성들은 아이템 전체의 배치 방향, 정렬, 간격 등을 제어합니다.

### 1) `flex-direction`: 배치 방향 설정

아이템을 어느 방향으로 쌓을지 결정합니다. (주축(Main Axis)의 방향을 결정)

-   `row` (기본값): 아이템을 가로로, 왼쪽에서 오른쪽으로 배치합니다.
-   `row-reverse`: 아이템을 가로로, 오른쪽에서 왼쪽으로 배치합니다.
-   `column`: 아이템을 세로로, 위에서 아래로 배치합니다.
-   `column-reverse`: 아이템을 세로로, 아래에서 위로 배치합니다.

```css
.container {
  display: flex;
  flex-direction: column; /* 아이템들을 세로로 배치 */
}
```

### 2) `justify-content`: 주축(Main Axis) 방향 정렬

주축 방향으로 아이템들을 어떻게 정렬하고 간격을 나눌지 결정합니다.

-   `flex-start` (기본값): 주축의 시작점에 맞춰 정렬합니다.
-   `flex-end`: 주축의 끝점에 맞춰 정렬합니다.
-   `center`: 주축의 중앙에 맞춰 정렬합니다.
-   `space-between`: 첫 아이템은 시작점에, 마지막 아이템은 끝점에 붙이고 나머지 아이템들 사이의 간격을 균등하게 분배합니다.
-   `space-around`: 모든 아이템들 주위에 균등한 간격을 만듭니다. (양 끝 아이템은 절반의 간격)
-   `space-evenly`: 모든 아이템들 사이 및 양 끝의 간격을 완전히 균등하게 분배합니다.

```css
.container {
  display: flex;
  justify-content: space-between; /* 아이템들 사이에 균등한 간격 */
}
```

### 3) `align-items`: 교차축(Cross Axis) 방향 정렬

주축의 수직 방향인 교차축을 기준으로 아이템들을 어떻게 정렬할지 결정합니다.

-   `stretch` (기본값): 아이템들이 교차축 방향으로 컨테이너를 꽉 채우도록 늘어납니다. (아이템에 `height` 값이 없어야 함)
-   `flex-start`: 교차축의 시작점에 맞춰 정렬합니다.
-   `flex-end`: 교차축의 끝점에 맞춰 정렬합니다.
-   `center`: 교차축의 중앙에 맞춰 정렬합니다.
-   `baseline`: 아이템들의 텍스트 기준선(baseline)에 맞춰 정렬합니다.

```css
.container {
  display: flex;
  height: 200px; /* 컨테이너 높이가 있어야 정렬 확인 가능 */
  align-items: center; /* 아이템들을 세로 중앙으로 정렬 */
}
```

### 4) `flex-wrap`: 아이템 줄 바꿈 설정

컨테이너의 너비보다 아이템들의 총 너비가 클 경우, 줄 바꿈을 할지 여부를 결정합니다.

-   `nowrap` (기본값): 줄 바꿈을 하지 않고 한 줄에 모든 아이템을 욱여넣습니다.
-   `wrap`: 아이템들이 컨테이너를 벗어나면 다음 줄로 넘어갑니다.
-   `wrap-reverse`: 아이템들을 역순으로 다음 줄로 넘깁니다.

### 5) `gap`: 아이템 사이의 간격 설정

`margin`을 사용하는 대신, 아이템들 사이에 직접 간격을 설정하는 가장 현대적이고 간편한 방법입니다.

```css
.container {
  display: flex;
  gap: 10px; /* 모든 아이템 사이에 10px의 간격 */
  row-gap: 20px; /* 행(가로) 간격만 지정 */
  column-gap: 30px; /* 열(세로) 간격만 지정 */
}
```

---

## 4. Flex Items에 적용하는 속성

아이템에 직접 적용하는 속성들은 개별 아이템의 크기나 순서를 제어합니다.

-   `flex-grow`: 컨테이너에 여유 공간이 있을 때, 아이템이 얼마나 더 늘어날지에 대한 비율을 지정합니다. (기본값 0)
-   `flex-shrink`: 컨테이너 공간이 부족할 때, 아이템이 얼마나 더 줄어들지에 대한 비율을 지정합니다. (기본값 1)
-   `order`: 아이템의 시각적인 순서를 변경합니다. 숫자가 작을수록 앞에 배치됩니다. (기본값 0)
-   `align-self`: 특정 아이템 하나에만 `align-items` 속성을 다르게 적용하고 싶을 때 사용합니다.

**예제:**
```css
.item:nth-child(2) { /* 2번째 아이템만 */
  flex-grow: 1; /* 다른 아이템은 그대로 두고, 2번째 아이템만 남은 공간을 모두 차지 */
}

.item:nth-child(3) { /* 3번째 아이템만 */
  order: -1; /* 3번째 아이템을 가장 앞으로 보냄 */
  align-self: flex-end; /* 3번째 아이템만 아래로 정렬 */
}
```

---

## 정리

Flexbox는 현대적인 웹 레이아웃을 만들기 위한 필수 도구입니다. `display: flex`를 시작으로, `flex-direction`, `justify-content`, `align-items` 이 세 가지 핵심 속성만 잘 이해해도 대부분의 1차원 레이아웃을 손쉽게 구성할 수 있습니다. 이제 우리는 더 이상 복잡한 `float`이나 `position` 속성에 의존하지 않고도 유연하고 반응형인 레이아웃을 만들 수 있는 강력한 기술을 갖추게 되었습니다. 다음 실습에서는 직접 Flexbox를 사용하여 자기소개 페이지의 레이아웃을 멋지게 꾸며보겠습니다.

---
- [목차로 돌아가기](README.md)
- [이전 강의로 이동](08-CSS-Box-Model.md)
- [다음 강의로 이동](10-CSS-Position-Property.md)
