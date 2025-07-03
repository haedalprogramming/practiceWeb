# 02. JSX 이해하기

이전 시간에 React와 개발 환경 설정에 대해 알아보았습니다. 이번 시간에는 React의 핵심 문법인 **JSX**에 대해 깊이 있게 배우겠습니다.

## 1. JSX란 무엇인가?

**JSX**는 **JavaScript XML**의 약자로, JavaScript 코드 안에서 UI를 쉽게 작성할 수 있도록 도와주는 특별한 문법(Syntax Extension)입니다. HTML과 매우 비슷하게 생겼지만, 실제로는 JavaScript의 확장 기능입니다.

```jsx
const element = <h1>안녕하세요, React!</h1>;
```

위 코드는 HTML처럼 보이지만, 변수에 HTML 구조를 직접 할당하고 있습니다. 이것이 바로 JSX입니다. 브라우저는 JSX를 직접 이해하지 못합니다. 우리가 Vite를 통해 프로젝트를 설정할 때 함께 설치된 **Babel**이라는 도구가 JSX 코드를 브라우저가 이해할 수 있는 일반 JavaScript 코드로 변환해줍니다.

> [!NOTE]
> **JSX는 JavaScript다!**
> 
> JSX는 HTML이 아닙니다. HTML의 문법을 빌려왔을 뿐, 그 본질은 JavaScript입니다. 따라서 JSX 안에서는 JavaScript의 모든 기능을 활용할 수 있습니다. 이 점이 JSX를 강력하게 만드는 핵심 요소입니다.

## 2. JSX 사용의 장점

그렇다면 왜 그냥 HTML이나 JavaScript 함수를 쓰지 않고 JSX를 사용할까요?

*   **직관적인 코드 작성**: HTML과 유사한 문법 덕분에 UI가 어떻게 보일지 코드를 통해 쉽게 예측할 수 있습니다. 로직(JavaScript)과 뷰(UI)를 한 파일에 함께 작성하여 코드의 연관성을 파악하기 쉽습니다.
*   **JavaScript의 모든 기능 활용**: JSX 코드 중간에 JavaScript 변수나 함수를 자유롭게 사용할 수 있어 동적인 UI를 쉽게 만들 수 있습니다.
*   **높은 개발 생산성**: 복잡한 UI 구조를 JavaScript만으로 만드는 것보다 훨씬 간결하고 빠르게 코드를 작성할 수 있습니다.

## 3. JSX 기본 문법

JSX를 사용할 때 반드시 지켜야 할 몇 가지 중요한 규칙이 있습니다.

### 1) JavaScript 표현식 사용하기: `{}`

JSX 내에서 JavaScript 변수, 함수 호출, 연산 등 JavaScript 코드를 사용하고 싶을 때는 중괄호(`{}`)로 감싸주면 됩니다.

```jsx
// 1. 변수 사용하기
const name = "홍길동";
const element = <h1>안녕하세요, {name}님!</h1>; // 결과: <h1>안녕하세요, 홍길동님!</h1>

// 2. 간단한 연산
const price = 1000;
const element2 = <p>가격: {price * 1.1}원 (부가세 포함)</p>; // 결과: <p>가격: 1100원 (부가세 포함)</p>

// 3. 함수 호출 결과 사용하기
function formatDate(date) {
  return date.toLocaleDateString();
}
const todayElement = <p>오늘은 {formatDate(new Date())}입니다.</p>; // 결과: <p>오늘은 2023. 10. 27.입니다.</p>
```

> [!CAUTION]
> 중괄호 안에는 **표현식(Expression)**만 사용할 수 있습니다. `if`문, `for`문과 같은 **문장(Statement)**은 직접 사용할 수 없습니다. 조건부 렌더링이나 반복 렌더링은 이후 강의에서 다른 방법으로 배우게 됩니다.

### 2) 속성(Attribute) 정의하기

HTML 태그에 속성을 부여하듯이 JSX에서도 속성을 정의할 수 있습니다. 속성 값으로 문자열을 사용할 때는 따옴표를, JavaScript 표현식을 사용할 때는 중괄호를 사용합니다.

```jsx
// 문자열을 값으로 사용하는 경우
const imgElement = <img src="/images/profile.jpg" alt="프로필 사진" />;

// JavaScript 변수를 값으로 사용하는 경우
const profileImageUrl = "/images/profile.jpg";
const altText = "프로필 사진";
const imgElement2 = <img src={profileImageUrl} alt={altText} />;
```

> [!IMPORTANT]
> **`class` 대신 `className` 사용**
> 
> HTML에서 CSS 클래스를 지정할 때 `class` 속성을 사용하지만, JSX에서는 `className`을 사용해야 합니다. `class`는 JavaScript에서 클래스를 정의할 때 사용하는 예약어(Reserved Word)이기 때문입니다.
> 
> ```jsx
> // 잘못된 사용 예
> const errorDiv = <div class="error">오류 메시지</div>;
> 
> // 올바른 사용 예
> const successDiv = <div className="success">성공 메시지</div>;
> ```
> 이 외에도 `tabindex`는 `tabIndex`처럼 camelCase(카멜케이스) 표기법을 따르는 몇 가지 속성들이 있습니다.

### 3) 반드시 하나의 부모 요소로 감싸기

컴포넌트가 반환하는 JSX는 반드시 **하나의 부모 요소**로 감싸져 있어야 합니다. 여러 개의 요소를 나란히 반환할 수 없습니다.

```jsx
// 잘못된 사용 예: 두 개의 형제 요소가 최상위에 있음
return (
  <h1>제목</h1>
  <p>내용</p>
);

// 올바른 사용 예 1: div로 감싸기
return (
  <div>
    <h1>제목</h1>
    <p>내용</p>
  </div>
);

// 올바른 사용 예 2: Fragment 사용하기
// 불필요한 div 생성을 피하고 싶을 때 사용합니다.
return (
  <>
    <h1>제목</h1>
    <p>내용</p>
  </>
);
```

`<>`와 `</>`로 감싸는 것을 **Fragment(조각)**라고 부릅니다. 화면에 보이지 않는 가상의 컨테이너 역할을 하여, 여러 요소를 묶어주는 용도로 사용됩니다.

### 4) 태그는 반드시 닫기

HTML에서는 `<br>`이나 `<img>`처럼 닫는 태그가 없는 태그들이 허용되지만, JSX에서는 모든 태그를 반드시 닫아야 합니다. 내용이 없는 태그는 스스로 닫는 태그(`/>`) 형식으로 작성해야 합니다.

```jsx
// 잘못된 사용 예
// <br>
// <input type="text">

// 올바른 사용 예
<br />
<input type="text" />
```

## 4. JSX 주석

JSX 코드 내에서 주석을 작성할 때는 JavaScript 주석(`//`, `/* */`)을 중괄호 `{}`로 감싸서 사용해야 합니다.

```jsx
const element = (
  <div>
    {/* 이것은 JSX 주석입니다. */}
    <h1>안녕하세요!</h1>
    {// 한 줄 주석도 가능합니다.}
    <p>이것은 JSX 예제입니다.</p>
  </div>
);
```

이제 JSX의 기본 문법에 대해 알아보았습니다. 다음 시간에는 이 JSX를 사용하여 UI의 독립적인 부품을 만드는 **컴포넌트**에 대해 배우겠습니다.

---

- [목차로 돌아가기](README.md)
- [이전 강의로 이동](13-Introducing-React-and-Setup.md)
- [다음 강의로 이동](15-Components.md)