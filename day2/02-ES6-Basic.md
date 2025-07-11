# 02. 기본 문법

## 목차

1. [콘솔 출력](#1-콘솔-출력)
2. [문(statement)](#2-문statement)
3. [세미콜론](#3-세미콜론)
4. [주석](#4-주석)
5. [변수 (Variables)](#5-변수-variables)
6. [자료형 (Data Types)](#6-자료형-data-types)

---

## 1) 콘솔 출력

콘솔 출력은 프로그래밍에서 매우 중요한 기능 중 하나입니다. 단순히 화면에 문자열을 출력하는 것 뿐만 아니라, 이를 통해 여러분이 작성한 코드가 제대로 작동하는지 확인하거나, 특정 시점에 변수에 어떤 값이 들어있는지 알아보고 싶을 때 사용합니다.

JavaScript에서 콘솔 출력은 `console.log()` 함수를 사용합니다. 여기서 `console`은 컴퓨터의 '터미널' 또는 '개발자 도구'라는 특별한 공간을 의미하고, `log`는 그 공간에 무언가를 '기록한다'는 뜻입니다. 즉, `console.log()`는 여러분이 원하는 정보를 컴퓨터 화면의 특정 공간에 보여달라고 명령하는 것입니다.

```javascript
console.log('Hello, World!'); // 'Hello, World!'라는 문자열을 콘솔에 출력합니다.
console.log(123);            // 숫자 123을 콘솔에 출력합니다.
console.log(1 + 2);          // 덧셈 결과인 3을 콘솔에 출력합니다.
```

> [!NOTE]
> 웹 브라우저에서 `console.log()`의 결과를 확인하려면, 웹 페이지에서 마우스 오른쪽 버튼을 클릭한 후 '검사' 또는 '개발자 도구'를 선택하고, 'Console' 탭을 클릭하면 됩니다. 이곳에서 여러분의 JavaScript 코드가 출력하는 메시지들을 볼 수 있습니다.

---

## 2) 문(statement)

문(statement)은 JavaScript 코드의 최소 단위로서, 컴퓨터에게 '무엇을 하라'고 지시하는 하나의 명령 또는 작업을 의미합니다. 모든 JavaScript 프로그램은 이러한 문(statement)들이 모여서 만들어집니다.

예를 들어, 아래와 같은 코드는 모두 문(statement)입니다.

```javascript
console.log('Hello, World!'); // 'Hello, World!'를 출력하라는 명령
let x = 10;                  // x라는 변수에 10을 저장하라는 명령
if (x > 5) {                 // 만약 x가 5보다 크다면... 이라는 조건 명령
    console.log('x는 5보다 큽니다.');
}
```

서로 다른 문은 세미콜론(;)으로 구분하는 것이 일반적입니다. 이는 컴퓨터에게 '여기까지가 하나의 명령이고, 이제 다음 명령이 시작된다'고 알려주는 역할을 합니다.

```javascript
console.log('a'); console.log('b'); // 두 개의 명령을 한 줄에 작성
```

> [!NOTE]
> 문(statement)은 컴퓨터가 수행할 수 있는 최소한의 완전한 작업 단위를 의미합니다. 여러분이 작성하는 모든 코드는 결국 이러한 문들의 조합으로 이루어집니다.

---

## 3) 세미콜론

JavaScript에서 세미콜론(;)은 문(statement)의 끝을 나타내는 역할을 합니다. 문장의 끝에 마침표를 찍는 것에 비유할 수 있습니다. 컴퓨터는 세미콜론을 보고 하나의 명령이 끝났음을 인식하고 다음 명령을 준비합니다.

원칙적으로 세미콜론(;)으로 문을 구분하지만, 아래와 같이 세미콜론 없이 줄바꿈만으로 문을 구분할 수 있습니다. 이는 JavaScript 엔진이 자동으로 세미콜론을 넣어주는 '자동 세미콜론 삽입(Automatic Semicolon Insertion, ASI)'이라는 기능 때문입니다.

```javascript
console.log('a') // JavaScript 엔진이 여기에 자동으로 세미콜론을 삽입합니다.
console.log('b')
```

> [!WARNING]
> 자동 세미콜론 삽입(ASI) 기능은 편리하지만, 때로는 예상치 못한 오류를 발생시킬 수 있습니다. 따라서 코드의 명확성과 안정성을 위해 **각 문장의 끝에는 세미콜론을 명시적으로 붙여주는 것을 권장**합니다. 특히 여러 개발자가 함께 작업하는 프로젝트에서는 일관된 코드 스타일을 유지하는 것이 중요합니다.

---

## 4) 주석

주석은 코드를 설명하거나, 특정 코드 블록을 일시적으로 비활성화할 때 사용합니다. 주석으로 처리된 부분은 JavaScript 엔진이 무시하므로, 프로그램 실행에 아무런 영향을 주지 않습니다. 주석은 여러분이 작성한 코드를 다른 사람이 이해하기 쉽게 만들고, 미래의 여러분이 코드를 다시 봤을 때도 쉽게 기억할 수 있도록 돕는 중요한 도구입니다.

한 줄 주석은 아래와 같이 `//` 뒤에 작성할 수 있습니다.

```javascript
// Hello, World! 출력
console.log('Hello, World!');
```

주석은 한 라인을 다 차지하지 않아도 되고, 문 뒤에 올 수 있습니다.

```javascript
console.log('Hello, World!'); // Hello, World! 출력
```

여러 줄 주석은 아래와 같이 `/* */`로 작성할 수 있습니다. `/*`와 `*/` 사이에 있는 모든 문자열은 주석으로 처리됩니다.

```javascript
/*
  Hello, World! 출력하는 함수.
  이 함수는 인자로 받은 메시지를 콘솔에 출력합니다.
  작성자: [여러분의 이름]
  작성일: 2025년 7월 3일
*/
function printMessage(message) {
  console.log(message);
}

printMessage('Hello, World!');
```

여러 줄 주석을 활용해 코드를 비활성화시킬 수 있습니다.

```javascript
let a = 1;
/*
let a = 2; // 이 코드는 주석 처리되어 실행되지 않습니다.
*/
console.log(a); // 결과는 1이 출력됩니다.
```

> [!TIP]
> Visual Studio Code 등 많은 코드 편집기에서 주석 단축키 `Ctrl + /` (Windows/Linux) 또는 `Cmd + /` (macOS)를 지원합니다. 원하는 영역을 드래그한 후 단축키를 누르면 주석 처리되고, 다시 누르면 해제됩니다.

> [!WARNING]
> 중첩 주석은 지원되지 않습니다. `/* */` 안에 `/* */` 를 작성하면 오류가 발생합니다.

> [!NOTE]
> **좋은 주석 작성법**
> 
> *   **왜(Why) 그렇게 했는지 설명**: 코드가 '무엇을(What)' 하는지는 코드 자체로 알 수 있지만, '왜(Why)' 그렇게 작성했는지는 주석으로 설명하는 것이 좋습니다. 예를 들어, 특정 로직을 선택한 이유나, 발생할 수 있는 문제점을 미리 경고하는 등의 내용을 담을 수 있습니다.
> *   **간결하고 명확하게**: 너무 길거나 복잡한 주석은 오히려 가독성을 해칠 수 있습니다. 핵심 내용을 간결하고 명확하게 작성합니다.
> *   **최신 상태 유지**: 코드가 변경되면 주석도 함께 업데이트해야 합니다. 오래된 주석은 오히려 혼란을 줄 수 있습니다.
> *   **불필요한 주석 피하기**: 너무 당연한 내용을 주석으로 달 필요는 없습니다. 코드를 읽으면 바로 이해할 수 있는 내용은 주석을 달지 않는 것이 좋습니다.

---

## 5) 변수 (Variables)

프로그래밍에서 **변수**는 데이터를 저장하는 '이름이 붙은 상자'와 같습니다. 여러분이 어떤 값을 기억하고 싶을 때, 그 값을 변수라는 상자에 넣어두고 필요할 때마다 상자의 이름을 불러서 안에 있는 값을 꺼내 사용할 수 있습니다.

### 변수를 사용하는 이유

*   **데이터 저장**: 프로그램이 실행되는 동안 필요한 데이터를 임시로 저장할 수 있습니다.
*   **재사용성**: 한 번 저장한 데이터를 여러 번 반복해서 사용할 수 있습니다.
*   **유연성**: 프로그램이 실행되는 동안 변수 안의 값을 변경할 수 있습니다.

### 변수 선언하기: `let`과 `const`

JavaScript에서는 변수를 선언할 때 주로 `let`과 `const`라는 키워드를 사용합니다.

#### `let`

`let`은 변수에 저장된 값을 나중에 변경할 수 있을 때 사용합니다.

```javascript
let message = "안녕하세요!"; // message라는 변수에 "안녕하세요!"라는 값을 저장합니다.
console.log(message);      // 출력: 안녕하세요!

message = "반갑습니다!";    // message 변수의 값을 "반갑습니다!"로 변경합니다.
console.log(message);      // 출력: 반갑습니다!
```

> [!NOTE]
> `let`으로 선언된 변수는 나중에 값을 바꿀 수 있기 때문에, '변할 수 있는 값'을 저장할 때 유용합니다.

#### `const`

`const`는 변수에 저장된 값을 한 번 정하면 나중에 변경할 수 없을 때 사용합니다. '상수(constant)'라고도 부릅니다.

```javascript
const PI = 3.14159; // PI라는 변수에 3.14159라는 값을 저장합니다.
console.log(PI);    // 출력: 3.14159

// PI = 3.14; // 이 코드는 오류를 발생시킵니다. const로 선언된 변수는 값을 변경할 수 없습니다.
```

> [!NOTE]
> `const`로 선언된 변수는 한 번 할당된 후에는 값을 변경할 수 없으므로, '변하지 않는 값'을 저장할 때 사용합니다. 예를 들어, 수학 상수(PI), 고정된 설정 값 등에 사용됩니다.

### 변수 이름 짓기 규칙

변수 이름을 지을 때는 몇 가지 규칙을 따라야 합니다.

*   영어 알파벳, 숫자, 언더스코어(`_`), 달러 기호(`$`)를 사용할 수 있습니다.
*   첫 글자는 숫자가 될 수 없습니다.
*   JavaScript에서 특별한 의미를 가지는 예약어(예: `let`, `const`, `function` 등)는 변수 이름으로 사용할 수 없습니다.
*   변수 이름은 의미를 명확하게 나타내도록 짓는 것이 좋습니다. (예: `userName` 대신 `u`는 좋지 않습니다.)

```javascript
let userName = "김철수"; // 좋은 예시: 변수의 목적이 명확합니다.
let _age = 20;
let $price = 1000;
// let 1stName = "홍길동"; // 오류: 숫자로 시작할 수 없습니다.
// let let = "변수";     // 오류: 예약어는 사용할 수 없습니다.
```

> [!TIP]
> 변수 이름을 지을 때는 보통 `camelCase` 방식을 많이 사용합니다. 첫 단어는 소문자로 시작하고, 그 다음 단어부터는 첫 글자를 대문자로 쓰는 방식입니다. (예: `myFirstName`, `totalAmount`)

---

## 6) 자료형 (Data Types)

변수에 저장할 수 있는 데이터의 종류를 **자료형**이라고 합니다. JavaScript에는 여러 가지 자료형이 있지만, 여기서는 가장 기본적이고 자주 사용되는 몇 가지를 소개하겠습니다.

### 6-1) 숫자 (Number)

숫자 자료형은 정수(소수점이 없는 수)와 실수(소수점이 있는 수)를 모두 포함합니다.

```javascript
let integer = 10;    // 정수
let float = 3.14;    // 실수
let result = integer + float; // 덧셈도 가능합니다.
console.log(result); // 출력: 13.14
```

### 6-2) 문자열 (String)

문자열 자료형은 글자들의 나열을 의미합니다. 작은따옴표(`'`)나 큰따옴표(`"`)로 감싸서 표현합니다.

```javascript
let greeting = "안녕하세요";
let name = '김영희';
let sentence = greeting + ", " + name + "님!"; // 문자열을 합칠 수 있습니다.
console.log(sentence); // 출력: 안녕하세요, 김영희님!
```

### 6-3) 불리언 (Boolean)

불리언 자료형은 오직 두 가지 값, `true`(참) 또는 `false`(거짓)만을 가집니다. 주로 어떤 조건이 맞는지 틀리는지를 판단할 때 사용됩니다.

```javascript
let isStudent = true;  // 학생이면 참
let hasLicense = false; // 면허가 없으면 거짓

console.log(isStudent);   // 출력: true
console.log(hasLicense);  // 출력: false
```

> [!NOTE]
> 불리언 값은 나중에 배우게 될 '조건문'에서 매우 중요하게 사용됩니다. 예를 들어, '만약 학생이라면...'과 같은 조건을 만들 때 `isStudent` 변수의 값이 `true`인지 `false`인지에 따라 다른 동작을 하도록 만들 수 있습니다.

---

- [목차로 돌아가기](../README.md)
- [이전 강의로 이동](01-Introducing-JavaScript.md)
- [다음 강의로 이동](03-ES6-Variables-and-Scoping.md)