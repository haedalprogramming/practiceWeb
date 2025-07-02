# 01. JavaScript 소개

JavaScript(JavaScript)는 웹 개발에서 가장 널리 사용되는 프로그래밍 언어 중 하나입니다. 원래는 웹 페이지에 동적인 기능을 추가하기 위해 만들어졌지만, 시간이 지나면서 웹뿐만 아니라 서버, 데스크탑, 모바일 앱, 게임 개발 등 다양한 영역으로 그 활용 범위가 넓어졌습니다.

이 문서는 JavaScript를 처음 접하는 분들을 위해, JavaScript가 무엇인지, 어떤 환경에서 실행되며, 왜 배워야 하는지를 이해하기 쉽게 설명합니다. 본격적인 문법 학습에 앞서 전체적인 그림을 이해하는 데 중점을 두었습니다.

---

## 1. JavaScript란?

JavaScript는 **사람과 컴퓨터가 대화할 수 있게 해주는 도구**입니다. 우리가 웹사이트에서 버튼을 클릭했을 때 화면이 바뀌거나, 검색어를 입력했을 때 실시간으로 결과가 나타나는 것, 드롭다운 메뉴가 열리는 것, 이미지를 클릭하면 모달 창이 뜨는 것 등은 대부분 JavaScript를 통해 구현됩니다.

JavaScript는 다음과 같은 특징을 가지고 있습니다:

* **동적**: 사용자의 행동에 따라 반응하고 화면을 즉시 바꿀 수 있습니다.
* **인터프리터 언어**: 별도 컴파일 없이 코드를 직접 실행할 수 있어 개발 속도가 빠릅니다.
* **다양한 실행 환경**: 웹 브라우저뿐 아니라 서버(Node.js), 데스크탑 앱, 모바일 앱 등 다양한 환경에서 동작합니다.
* **이벤트 기반 프로그래밍**: 사용자 클릭, 키 입력, 타이머, 네트워크 응답 등 다양한 이벤트를 처리하는 데 적합합니다.
* **표준화된 문법**: ECMAScript 표준을 따르기 때문에 다양한 플랫폼 간 호환성이 뛰어납니다.
* **확장성**: 다양한 라이브러리와 프레임워크(React, Vue, Angular 등)를 활용해 복잡한 애플리케이션도 개발할 수 있습니다.

---

## 2. JavaScript의 실행 환경

JavaScript는 실행 환경에 따라 가능한 기능이 달라집니다. 크게는 **브라우저 환경**과 **Node.js 환경**으로 나눌 수 있으며, 각각의 환경에서 JavaScript는 서로 다른 역할을 수행합니다.

### 2.1 브라우저 환경

웹 브라우저(예: Chrome, Firefox, Safari)는 각자 JavaScript 엔진(예: V8, SpiderMonkey, JavaScriptCore)을 내장하고 있어 웹 페이지 안에서 JavaScript를 실행할 수 있습니다. 이 엔진 덕분에 우리는 웹사이트에서 JavaScript 기반으로 동작하는 다양한 기능들을 사용할 수 있게 됩니다. 이 환경에서는 다음과 같은 작업이 가능합니다:

* HTML 문서의 구조나 내용을 동적으로 수정
* CSS 스타일을 변경하여 화면 구성 조절
* 마우스 클릭, 키보드 입력 등의 사용자 이벤트 처리
* Ajax를 통한 서버와의 비동기 통신
* 애니메이션 효과, 자동 슬라이드 등 인터랙티브한 기능 구현

```html
<script>
    // 브라우저 알림창 띄우기
    alert("Hello, World!");
</script>
```

#### 프론트엔드에서의 활용 예시

아래는 프론트엔드 브라우저 환경에서 JavaScript를 활용하여 기능을 구현하는 예시입니다.

1. **메뉴 열기/닫기 토글**
    ```javascript
    // 버튼 엘리먼트 얻기
    const button = document.querySelector('#menu-button');

    // 메뉴 엘리먼트 얻기
    const menu = document.querySelector('#menu');

    // 버튼 클릭 이벤트 리스너 추가
    button.addEventListener('click', () => {
        menu.classList.toggle('open'); // 메뉴 열기/닫기 토글
    });
    ```

2. **입력값 유효성 검사**
    ```javascript
    const input = document.querySelector('#email');
    input.addEventListener('blur', () => {
    if (!input.value.includes('@')) {
        alert('이메일 주소를 올바르게 입력해주세요.');
    }
    });
    ```

3. **스크롤 애니메이션 효과**
    ```javascript
    window.addEventListener('scroll', () => {
    const header = document.querySelector('header');
    header.classList.toggle('scrolled', window.scrollY > 50);
    });
    ```

4. **모달 창 열기 및 닫기**
    ```javascript
    const modal = document.querySelector('#modal');
    const openBtn = document.querySelector('#open-modal');
    const closeBtn = document.querySelector('#close-modal');

    openBtn.addEventListener('click', () => {
    modal.style.display = 'block';
    });

    closeBtn.addEventListener('click', () => {
    modal.style.display = 'none';
    });
    ```

이처럼 JavaScript는 웹 사용자 경험을 풍부하게 만들 수 있는 강력한 도구입니다.

### 2.2 Node.js 환경

Node.js는 JavaScript를 서버 측 언어처럼 사용할 수 있게 해주는 런타임(실행환경)입니다. 즉 **웹 브라우저가 아닌 컴퓨터 자체에서** JavaScript를 실행할 수 있도록 해주며, 특히 프론트엔드 및 백엔드 서버 개발에 많이 사용됩니다.

Node.js에서는 다음과 같은 작업이 가능합니다:

* 웹 서버 또는 API 서버 구축
* 파일 시스템 읽기/쓰기
* 데이터베이스 연동
* 네트워크 통신 처리
* 백그라운드 작업 처리 등

```javascript
// Node.js 환경에서 실행되는 예시
const fs = require('fs');
fs.writeFileSync('example.txt', 'Hello, Node.js!');
```

> [!NOTE]
> Node.js 환경은 웹 애플리케이션의 뒷단을 구성하는 데 유용하며, 프론트엔드와 백엔드를 모두 JavaScript로 개발할 수 있게 해줍니다. 이를 풀스택 JavaScript라고 부르며, 많은 현대 웹 개발 프로젝트에서 활용되고 있습니다. Node.js 에서 프론트엔드를 개발하는 경우, Vite나 Webpack 같은 빌드 도구(번들러, 트랜스파일러 등)가 코드를 웹 브라우저에서 실행될 수 있는 형태로 변환해주게 됩니다. 이 부분은 프론트엔드 개발 파트에서 본격적으로 다루겠습니다.

---

## 2.3 다른 언어와의 차이점

JavaScript는 C, Java, Python 등 다른 언어와 비교했을 때 다음과 같은 차이점이 있습니다:

* **브라우저에서 직접 실행 가능**: JavaScript는 별도의 개발 환경 설치 없이 웹 브라우저에서 곧바로 실행할 수 있다는 장점이 있습니다. 반면 Java나 Python은 실행을 위해 별도 설치가 필요합니다.
* **유연한 문법**: 변수 선언 방식(var, let, const)이나 함수 정의 방법 등에서 유연성이 높고, 문법 오류가 있어도 실행되는 경우가 많아 초보자에게 친숙하지만, 큰 프로젝트에서는 주의가 필요합니다.
* **객체 기반 프로그래밍**: 클래스가 존재하긴 하지만 전통적인 클래스 기반 언어(C++, Java)와는 다르게, 프로토타입 기반 객체지향 방식을 사용합니다.
* **싱글 스레드, 비동기 처리 중심**: 기본적으로 싱글 스레드지만, 비동기 처리(콜백, Promise, async/await)에 특화되어 있어 네트워크 작업이나 사용자 입력 처리를 효율적으로 할 수 있습니다.

이러한 특징 덕분에 JavaScript는 웹 환경에 최적화되어 있으며, 배우기 쉬우면서도 깊이 있게 확장해나갈 수 있는 언어입니다.

## 3. JavaScript를 배우는 이유

JavaScript는 코딩을 처음 배우는 사람에게도 적합한 언어입니다. 그 이유는 다음과 같습니다:

* 웹 브라우저만 있으면 바로 실습 가능 (추가 설치 필요 없음)
* 결과를 화면에서 바로 확인 가능하여 피드백이 빠름
* 문법이 비교적 간단하고 유연하여 접근성이 높음
* 전 세계적으로 사용자가 많아 튜토리얼과 예제가 풍부함
* 다양한 오픈소스 라이브러리와 프레임워크를 활용 가능
* 프론트엔드, 백엔드, 앱 개발까지 하나의 언어로 확장 가능

또한 JavaScript는 HTML, CSS와 함께 **웹의 3대 핵심 기술**로 불리며, 웹 브라우저 상에서 동작하는 유일한 프로그래밍 언어이기도 합니다. 웹 개발을 시작하려는 누구에게나 필수적인 기술입니다.

> [!NOTE]
> **TypeScript란?**
> 
> TypeScript는 JavaScript의 상위 집합으로, 정적 타입 검사를 추가하여 더 안전한 코드를 작성할 수 있도록 도와줍니다. 실제 현업에서는 JavaScript의 상위 확장 언어인 TypeScript가 널리 사용되며, 타입 안정성과 유지보수 측면에서 장점을 제공합니다. 하지만 본 강의에서는 JavaScript만을 중심으로 다루겠습니다.

## 4. 마무리

JavaScript는 단순한 웹 페이지 꾸미기를 넘어서, 현대 소프트웨어 개발의 중심 언어 중 하나로 자리잡고 있습니다. 배우기 쉬우면서도 확장성이 높고, 다양한 분야에서 활용 가능하다는 점에서 매우 매력적인 언어입니다.

이 문서를 통해 JavaScript의 기본 개념과 실행 환경, 활용 사례를 이해하고 흥미를 가지셨다면, 이제 본격적인 문법 학습으로 나아갈 차례입니다. 앞으로의 학습에서는 변수, 제어문, 함수, 객체, 배열, DOM 조작 등 JavaScript의 핵심 개념을 차근차근 배워나가게 될 것입니다.

여러분의 첫 번째 코딩 언어로서 JavaScript가 좋은 출발점이 되기를 바랍니다!

---

- [목차로 돌아가기](../README.md)
- [다음 강의로 이동](02-ES6-Basic.md)