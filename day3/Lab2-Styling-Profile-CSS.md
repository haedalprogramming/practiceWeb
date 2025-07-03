# Lab2. 자기소개 페이지 꾸미기 (CSS)

이전 실습에서 우리는 HTML을 사용하여 자기소개 페이지의 뼈대를 만들었습니다. 이번 실습에서는 CSS를 사용하여 이 페이지를 시각적으로 아름답게 꾸며보겠습니다. CSS 선택자, 박스 모델, 그리고 Flexbox를 활용하여 레이아웃을 구성하는 방법을 직접 적용해 볼 것입니다.

이 실습을 통해 여러분은 다음을 할 수 있게 됩니다:

*   외부 CSS 파일을 HTML 문서에 연결합니다.
*   다양한 CSS 선택자를 사용하여 특정 HTML 요소를 선택합니다.
*   글자 스타일, 배경색, 여백, 테두리 등 기본적인 CSS 속성을 적용합니다.
*   `box-sizing: border-box`를 사용하여 요소의 크기 계산을 직관적으로 만듭니다.
*   Flexbox를 사용하여 웹 페이지의 주요 섹션들을 배치하고 정렬합니다.

---

## Step-by-Step 가이드

### Step 1: 실습 환경 준비

이전 실습에서 만들었던 `my-profile.html` 파일을 준비합니다. 이 파일이 `day3` 폴더 안에 있는지 확인해주세요.

### Step 2: 새로운 CSS 파일 생성

`day3` 폴더 안에 `style.css` 라는 이름의 새 파일을 생성합니다. 이 파일에 자기소개 페이지를 꾸밀 모든 CSS 코드를 작성할 것입니다.

```
day3/
├── my-profile.html
├── style.css
└── ...
```

### Step 3: HTML 파일에 CSS 연결하기

`my-profile.html` 파일의 `<head>` 태그 안에 `<link>` 태그를 사용하여 `style.css` 파일을 연결합니다. 이렇게 해야 HTML 문서가 CSS 파일의 스타일을 읽어들일 수 있습니다.

**`my-profile.html` 수정:**

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>나의 자기소개</title>
  <link rel="stylesheet" href="style.css"> <!-- 이 줄을 추가합니다. -->
</head>
<body>
  <!-- ... (기존 내용) ... -->
</body>
</html>
```

### Step 4: 모든 요소에 `box-sizing: border-box` 적용

`style.css` 파일에 가장 먼저 모든 요소에 `box-sizing: border-box`를 적용하는 코드를 추가합니다. 이는 앞으로 요소의 크기를 계산할 때 혼란을 줄여줍니다.

**`style.css`:**

```css
/* style.css */

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: 'Arial', sans-serif;
  line-height: 1.6;
  color: #333;
  background-color: #f4f4f4;
}
```

*   `margin: 0; padding: 0;`: 브라우저마다 기본적으로 가지고 있는 여백을 초기화하여, 우리가 원하는 대로 정확하게 레이아웃을 잡을 수 있도록 합니다.
*   `body`: 웹 페이지 전체의 기본 글꼴, 줄 간격, 글자색, 배경색을 설정합니다.

### Step 5: `<header>` 스타일 꾸미기

페이지의 머리말인 `<header>`에 배경색, 글자색, 패딩 등을 적용하여 눈에 띄게 만들어 보세요.

**`style.css`:**

```css
/* ... (이전 내용) ... */

header {
  background-color: #333;
  color: #fff;
  padding: 20px 0;
  text-align: center;
}

header h1 {
  margin-bottom: 10px;
}
```

### Step 6: `<main>`과 `<section>`에 기본 스타일 적용

`<main>` 영역에 최대 너비를 지정하고 가운데 정렬합니다. 각 `<section>`에는 배경색과 패딩, 마진을 주어 구획을 명확히 합니다.

**`style.css`:**

```css
/* ... (이전 내용) ... */

main {
  max-width: 960px;
  margin: 20px auto; /* 위아래 20px, 좌우 auto로 가운데 정렬 */
  padding: 0 20px;
}

section {
  background-color: #fff;
  padding: 20px;
  margin-bottom: 20px;
  border-radius: 8px; /* 모서리를 둥글게 */
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); /* 그림자 효과 */
}

section h2 {
  color: #0056b3;
  margin-bottom: 15px;
}
```

### Step 7: 이미지와 링크 스타일 꾸미기

이미지의 크기를 조절하고, 링크에 마우스를 올렸을 때 색상이 변하도록 가상 클래스를 사용해 보세요.

**`style.css`:**

```css
/* ... (이전 내용) ... */

img {
  max-width: 100%; /* 이미지가 부모 요소 너비를 넘지 않도록 */
  height: auto;
  display: block; /* 이미지를 블록 요소로 만들어 마진 적용 용이 */
  margin: 0 auto 15px auto; /* 가운데 정렬 및 아래 여백 */
  border-radius: 4px;
}

a {
  color: #007bff;
  text-decoration: none; /* 기본 밑줄 제거 */
}

a:hover {
  text-decoration: underline; /* 마우스 올리면 밑줄 다시 표시 */
  color: #0056b3;
}
```

### Step 8: `<nav>`와 `<ul>`, `<li>` 스타일 꾸미기

만약 `my-profile.html`에 `<nav>` 태그를 추가했다면, 내비게이션 메뉴를 가로로 배치하고 꾸며보세요. `<ul>`과 `<li>`의 기본 스타일(점, 여백)을 제거하고 Flexbox를 사용합니다.

**`my-profile.html` (예시: `<header>` 안에 `<nav>` 추가)**

```html
<header>
  <h1>김코딩의 자기소개</h1>
  <p>안녕하세요! 웹 개발을 배우고 있는 김코딩입니다.</p>
  <nav>
    <ul>
      <li><a href="#">홈</a></li>
      <li><a href="#">소개</a></li>
      <li><a href="#">연락처</a></li>
    </ul>
  </nav>
</header>
```

**`style.css`:**

```css
/* ... (이전 내용) ... */

nav ul {
  list-style: none; /* 목록의 점 제거 */
  display: flex; /* Flexbox 적용 */
  justify-content: center; /* 아이템들을 가운데 정렬 */
  padding: 0;
}

nav ul li {
  margin: 0 15px;
}

nav ul li a {
  color: #fff;
  font-weight: bold;
}

nav ul li a:hover {
  color: #ddd;
}
```

### Step 9: `<footer>` 스타일 꾸미기

페이지의 꼬리말인 `<footer>`에 배경색, 글자색, 패딩 등을 적용합니다.

**`style.css`:**

```css
/* ... (이전 내용) ... */

footer {
  background-color: #333;
  color: #fff;
  text-align: center;
  padding: 20px 0;
  margin-top: 20px;
}
```

### Step 10: 웹 브라우저에서 결과 확인

`my-profile.html` 파일을 웹 브라우저로 열어보세요. 이제 이전보다 훨씬 깔끔하고 보기 좋은 자기소개 페이지가 완성되었을 것입니다. 각 요소에 적용된 스타일과 레이아웃을 확인해 보세요.

---

## 🚀 도전 과제 (Optional)

실습을 완료했다면, 다음 기능들을 추가하여 자기소개 페이지를 더 멋지게 꾸며보세요.

1.  **클래스 선택자 활용**: 특정 `<p>` 태그나 `<li>` 태그에 `class="important"`와 같은 클래스를 추가하고, 이 클래스에만 적용되는 특별한 스타일(예: 글자색, 배경색)을 `style.css`에 추가해 보세요.
2.  **ID 선택자 활용**: 특정 섹션에 `id="contact-info"`와 같은 ID를 부여하고, 이 ID에만 적용되는 고유한 스타일을 추가해 보세요.
3.  **폰트 변경**: Google Fonts 등에서 마음에 드는 폰트를 찾아 `body`나 특정 요소에 적용해 보세요.
4.  **반응형 디자인**: 화면 너비가 줄어들 때 (예: 모바일 화면) 레이아웃이 자동으로 변경되도록 `@media` 쿼리를 사용해 보세요. (이것은 조금 더 어려운 도전 과제입니다!)

## ✅ 최종 코드

실습을 진행하다 막히는 부분이 있다면 아래의 완성된 코드를 참고하세요.

<details>
<summary><b>my-profile.html</b></summary>

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>나의 자기소개</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <header>
    <h1>김코딩의 자기소개</h1>
    <p>안녕하세요! 웹 개발을 배우고 있는 김코딩입니다.</p>
    <nav>
      <ul>
        <li><a href="#">홈</a></li>
        <li><a href="#">소개</a></li>
        <li><a href="#">연락처</a></li>
      </ul>
    </nav>
  </header>

  <main>
    <section>
      <h2>나의 관심사</h2>
      <ul>
        <li>웹 개발 (HTML, CSS, JavaScript)</li>
        <li>독서 (특히 SF 소설)</li>
        <li>영화 감상</li>
      </ul>
    </section>

    <section>
      <h2>나의 사진</h2>
      <img src="https://via.placeholder.com/150" alt="나의 프로필 사진">
      <p>이것은 저의 프로필 사진입니다.</p>
    </section>

    <section>
      <h2>연락처</h2>
      <p>더 많은 정보를 원하시면 <a href="https://github.com/your-github-id" target="_blank">나의 GitHub</a>를 방문해주세요.</p>
    </section>
  </main>

  <footer>
    <p>&copy; 2025 김코딩. 모든 권리 보유.</p>
  </footer>
</body>
</html>
```

</details>

<details>
<summary><b>style.css</b></summary>

```css
/* style.css */

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: 'Arial', sans-serif;
  line-height: 1.6;
  color: #333;
  background-color: #f4f4f4;
}

header {
  background-color: #333;
  color: #fff;
  padding: 20px 0;
  text-align: center;
}

header h1 {
  margin-bottom: 10px;
}

nav ul {
  list-style: none;
  display: flex;
  justify-content: center;
  padding: 0;
}

nav ul li {
  margin: 0 15px;
}

nav ul li a {
  color: #fff;
  font-weight: bold;
}

nav ul li a:hover {
  color: #ddd;
}

main {
  max-width: 960px;
  margin: 20px auto;
  padding: 0 20px;
}

section {
  background-color: #fff;
  padding: 20px;
  margin-bottom: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

section h2 {
  color: #0056b3;
  margin-bottom: 15px;
}

img {
  max-width: 100%;
  height: auto;
  display: block;
  margin: 0 auto 15px auto;
  border-radius: 4px;
}

a {
  color: #007bff;
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
  color: #0056b3;
}

footer {
  background-color: #333;
  color: #fff;
  text-align: center;
  padding: 20px 0;
  margin-top: 20px;
}
```

</details>

---
- [목차로 돌아가기](README.md)
- [이전 강의로 돌아가기](10-CSS-Pseudo-Classes.md)
- [다음 강의로 이동](13-Introducing-React-and-Setup.md)
