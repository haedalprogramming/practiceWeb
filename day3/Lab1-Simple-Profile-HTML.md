# Lab1. 나의 하루 일과 타임테이블  페이지 만들기 (HTML)

지금까지 우리는 HTML의 기본 구조, 자주 사용하는 태그들, 그리고 의미를 담는 시맨틱 태그에 대해 배웠습니다. 이제 배운 내용을 바탕으로 여러분만의 간단한 하루일과 타임테이블 웹 페이지를 직접 만들어 보겠습니다.

이 실습을 통해 여러분은 다음을 할 수 있게 됩니다:

*   HTML 문서의 기본 구조를 작성합니다.
*   다양한 HTML 태그를 사용하여 텍스트, 이미지, 링크 등을 추가합니다.
*   시맨틱 태그를 활용하여 웹 페이지의 구조를 의미 있게 만듭니다.

---

## Step-by-Step 가이드

### Step 1: 새로운 HTML 파일 생성

먼저, 하루 일과 페이지를 만들 새로운 HTML 파일을 생성합니다. `day3` 폴더 안에 `my-day-routine.html` 이라는 이름으로 파일을 만들어 주세요.

```
day3/
├── ...
└── my-day-routine.html
```

### Step 2: HTML 기본 구조 작성

`my-day-routinee.html` 파일에 모든 HTML 문서가 가져야 할 기본적인 구조를 작성합니다. `<!DOCTYPE html>`, `<html>`, `<head>`, `<body>` 태그를 포함합니다.

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>나의 하루 일과</title>
</head>
<body>
  <!-- 여기에 내용을 작성할 것입니다. -->
</body>
</html>
```

*   `<!DOCTYPE html>`: 이 문서가 HTML5 표준을 따른다는 것을 브라우저에게 알려줍니다.
*   `<html lang="ko">`: 문서의 시작을 알리고, 문서의 주 언어가 한국어(`ko`)임을 명시합니다.
*   `<head>`: 웹 페이지 자체에 대한 정보(메타데이터)를 담는 부분입니다. 브라우저 화면에는 직접 보이지 않습니다.
    *   `<meta charset="UTF-8">`: 한글이 깨지지 않도록 문자 인코딩을 UTF-8로 설정합니다.
    *   `<meta name="viewport" content="width=device-width, initial-scale=1.0">`: 다양한 기기(모바일, 태블릿 등)에서 웹 페이지가 잘 보이도록 설정합니다.
    *   `<title>나의 자기소개</title>`: 웹 브라우저 탭에 표시될 페이지의 제목입니다.
*   `<body>`: 웹 브라우저 화면에 실제로 보여질 모든 내용을 담는 부분입니다.

### Step 3: 페이지의 머리말 (`<header>`) 추가

`<body>` 태그 안에 페이지의 머리말 역할을 할 `<header>` 태그를 추가하고, 여러분의 이름과 간단한 소개를 `<h1>` 태그를 사용하여 작성합니다.

```html
<body>
  <header>
    <h1>OOO의 하루 일과</h1>
  </header>

  <!-- 여기에 다른 내용을 추가할 것입니다. -->
</body>
```

### Step 4: 주요 내용 (`<main>`)과 섹션 (`<section>`) 추가

이제 하루일과 페이지의 핵심 내용을 담을 `<main>` 태그를 추가합니다. 그 안에 여러분의 일과를 표현하는 `<section>`을 만들어 보세요. `<h2>`로 섹션 제목을, `<ul>`과 `<li>`로 목록을 작성합니다.

```<body>
  <header>
    <h1>OOO의 하루 일과</h1>
  </header>

  <main>
    <section>
      <h2>일정 요약</h2>
      <ul>
        <li>07:00 - 기상 및 준비</li>
        <li>08:00 - 등교</li>
        <li>09:00 ~ 16:00 - 수업</li>
        <li>18:00 - 저녁 식사</li>
        <li>21:00 - 공부 및 자유시간</li>
        <li>23:30 - 취침</li>
      </ul>
    </section>

    <section>
      <h2>일과표</h2>
      <table border="1">
        <thead>
          <tr>
            <th>시간</th>
            <th>활동</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>07:00</td>
            <td>기상 및 아침 준비</td>
          </tr>
          <tr>
            <td>08:00</td>
            <td>학교 등교</td>
          </tr>
          <tr>
            <td>09:00 ~ 16:00</td>
            <td>학교 수업</td>
          </tr>
          <tr>
            <td>18:00</td>
            <td>저녁 식사</td>
          </tr>
          <tr>
            <td>21:00</td>
            <td>자기계발 및 휴식</td>
          </tr>
          <tr>
            <td>23:30</td>
            <td>취침</td>
          </tr>
        </tbody>
      </table>
    </section>

    <section>
      <h2>하이라이트 사진</h2>
      <img src="https://via.placeholder.com/300x200" alt="공부하는 모습">
      <p>열심히 공부하는 시간을 보내는 모습이에요!</p>
    </section>
  </main>

```

### Step 5: 이미지 (`<img>`)와 링크 (`<a>`) 추가

하루일과 페이지에 여러분의 사진이나 좋아하는 이미지, 그리고 외부 링크를 추가해 보세요. 이미지는 `<img>` 태그를 사용하고, 링크는 `<a>` 태그를 사용합니다.

> [!TIP]
> 이미지는 `public` 폴더나 `src/assets` 폴더에 넣어두고 상대 경로로 연결할 수 있습니다. 지금은 임시로 아무 이미지 URL이나 사용해도 좋습니다.


*   `<img>` 태그의 `src` 속성에는 이미지 파일의 경로를, `alt` 속성에는 이미지를 설명하는 텍스트를 넣어줍니다. `alt` 텍스트는 이미지가 로드되지 않거나 시각 장애인을 위한 스크린 리더가 이미지를 설명할 때 사용됩니다.
*   `<a>` 태그의 `href` 속성에는 이동할 웹 주소를, `target="_blank"`는 링크를 새 탭에서 열도록 합니다.

### Step 6: 페이지의 꼬리말 (`<footer>`) 추가

페이지의 가장 아래에는 저작권 정보나 연락처 등 부가적인 정보를 담을 `<footer>` 태그를 추가합니다.

```<body>
  <header>
    <h1>OOO의 하루 일과</h1>
  </header>

  <main>
    <section>
      <h2>일정 요약</h2>
      <ul>
        <li>07:00 - 기상 및 준비</li>
        <li>08:00 - 등교</li>
        <li>09:00 ~ 16:00 - 수업</li>
        <li>18:00 - 저녁 식사</li>
        <li>21:00 - 공부 및 자유시간</li>
        <li>23:30 - 취침</li>
      </ul>
    </section>

    <section>
      <h2>일과표</h2>
      <table border="1">
        <thead>
          <tr>
            <th>시간</th>
            <th>활동</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>07:00</td>
            <td>기상 및 아침 준비</td>
          </tr>
          <tr>
            <td>08:00</td>
            <td>학교 등교</td>
          </tr>
          <tr>
            <td>09:00 ~ 16:00</td>
            <td>학교 수업</td>
          </tr>
          <tr>
            <td>18:00</td>
            <td>저녁 식사</td>
          </tr>
          <tr>
            <td>21:00</td>
            <td>자기계발 및 휴식</td>
          </tr>
          <tr>
            <td>23:30</td>
            <td>취침</td>
          </tr>
        </tbody>
      </table>
    </section>

    <section>
      <h2>하이라이트 사진</h2>
      <img src="https://via.placeholder.com/300x200" alt="공부하는 모습">
      <p>열심히 공부하는 시간을 보내는 모습이에요!</p>
    </section>
  </main>

  <footer>
    <p>&copy; 2025 OOO. 하루를 열심히 사는 중입니다!</p>
  </footer>
</body>

  
```

### Step 7: 웹 브라우저에서 결과 확인

`my-day-routine` 파일을 웹 브라우저로 직접 열어보세요. (파일을 더블 클릭하거나, 브라우저에 파일을 드래그 앤 드롭하면 됩니다.) 여러분이 작성한 내용이 화면에 잘 표시되는지 확인합니다.

---

## 🚀 도전 과제 (Optional)

실습을 완료했다면, 다음 기능들을 추가하여 하루루틴 페이지를 더 풍성하게 만들어 보세요.

1.  **다른 섹션 추가**: `<h2>`와 `<p>` 태그를 사용하여 여러분의 학력, 경력, 좋아하는 명언 등 새로운 섹션을 추가해 보세요.
2.  **목록 추가**: `<ul>` 외에 순서가 있는 목록인 `<ol>` 태그를 사용하여 좋아하는 영화 목록이나 개발 학습 계획 등을 추가해 보세요.
3.  **강조 텍스트**: `<strong>`이나 `<em>` 태그를 사용하여 중요한 단어나 문장을 강조해 보세요.
4.  **시맨틱 태그 활용**: `my-day-rountine.html` 파일에 `<nav>`, `<article>`, `<aside>`와 같은 다른 시맨틱 태그를 적절히 활용하여 페이지의 구조를 더 의미 있게 만들어 보세요.


---
- [목차로 돌아가기](README.md)
- [이전 강의로 이동](03-Semantic-HTML.md)
- [다음 강의로 이동](04-Getting-Started-with-CSS.md)
