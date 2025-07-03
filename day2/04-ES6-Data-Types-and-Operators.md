# 04. 데이터 타입과 연산자

이전 강의에서 우리는 변수라는 '값을 담는 상자'에 대해 배웠습니다. 이번에는 그 상자 안에 담을 수 있는 값의 종류, 즉 **데이터 타입(Data Type)**과, 그 값들을 가지고 계산하거나 비교하는 방법인 **연산자(Operator)**에 대해 깊이 있게 알아보겠습니다.

## 1) 데이터 타입 (Data Types)

데이터 타입은 프로그래밍에서 다룰 수 있는 데이터의 종류를 말합니다. JavaScript는 다양한 데이터 타입을 가지고 있으며, 가장 기본이 되는 원시 타입(Primitive Types)들을 먼저 살펴보겠습니다.

### 1-1) 숫자 (Number)

`Number` 타입은 말 그대로 숫자를 나타냅니다. 정수(e.g., `10`, `0`, `-5`)와 소수(e.g., `3.14`, `-0.5`)를 모두 포함합니다.

```javascript
let age = 17;       // 정수
let height = 180.5;   // 소수
let temperature = -3; // 음수
```

`Number` 타입에는 몇 가지 특수한 값도 있습니다.

*   `Infinity`: 양의 무한대를 나타냅니다.
*   `NaN` (Not a Number): 숫자가 아님을 나타내는 값으로, 잘못된 수학 계산의 결과로 발생합니다.

```javascript
console.log(1 / 0); // Infinity
console.log("문자열" * 2); // NaN
```

### 1-1-1) 유용한 숫자 관련 기능

`Number` 데이터 자체에 내장된 기능들과, 수학 계산을 도와주는 `Math` 객체에 대해 알아봅시다.

**메소드(Method)**는 특정 데이터 타입에 연결된 함수(기능)라고 생각할 수 있습니다. 예를 들어, 숫자 변수 뒤에 점(`.`)을 찍고 사용하는 기능들입니다.

*   **.toFixed(자릿수)**: 소수점 이하의 자릿수를 지정하여 반올림하고, 그 결과를 **문자열**로 반환합니다.
    ```javascript
    const pi = 3.141592;
    console.log(pi.toFixed(2)); // "3.14" (소수점 셋째 자리에서 반올림)
    ```

*   **.toString()**: 숫자를 문자열로 변환합니다.
    ```javascript
    let num = 123;
    let strNum = num.toString();
    console.log(strNum); // "123"
    ```

**`Math` 객체**

`Math`는 수학적인 상수나 함수들을 모아놓은 내장 객체입니다.

*   `Math.round(숫자)`: 숫자를 반올림합니다.
*   `Math.ceil(숫자)`: 숫자를 올림합니다. (ceil은 '천장'이라는 뜻)
*   `Math.floor(숫자)`: 숫자를 내림(버림)합니다. (floor는 '바닥'이라는 뜻)
    ```javascript
    console.log(Math.round(4.7)); // 5
    console.log(Math.round(4.2)); // 4
    console.log(Math.ceil(4.2));  // 5
    console.log(Math.floor(4.7)); // 4
    ```

*   `Math.random()`: 0에서 1 사이(1은 제외)의 임의의 소수를 반환합니다.
    ```javascript
    // 0과 1 사이의 랜덤 숫자
    console.log(Math.random()); 
    // 1부터 10까지의 랜덤 정수를 얻고 싶을 때
    console.log(Math.floor(Math.random() * 10) + 1);
    ```

### 1-2) 문자열 (String)

`String` 타입은 텍스트 데이터를 나타냅니다. 따옴표(`' '`), 큰따옴표(`" "`), 또는 백틱(`` ` ``)으로 텍스트를 감싸서 만듭니다.

```javascript
let name = "홍길동";
let school = '코딩고등학교';

console.log(name);
console.log(school);
console.log("이름의 길이:", name.length); // 3 (문자열의 길이를 확인할 수 있습니다.)
```

#### 1-2-1) 템플릿 리터럴 (Template Literals)

ES6에서 도입된 **템플릿 리터럴**은 문자열을 만들 때 백틱(`` ` ``)을 사용하여 변수를 쉽게 포함시키고 여러 줄의 문자열을 작성하는 새로운 방법입니다. 기존의 따옴표를 사용한 문자열보다 훨씬 유연하고 가독성이 좋습니다.

*   **변수 삽입 (보간법)**: `${변수명}` 또는 `${표현식}` 형태로 문자열 안에 변수나 JavaScript 코드를 직접 삽입할 수 있습니다.
*   **여러 줄 문자열**: 별도의 줄 바꿈 문자(`
`) 없이도 여러 줄에 걸쳐 문자열을 작성할 수 있습니다.

```javascript
const userName = "김철수";
const userAge = 17;

// 기존 방식 (문자열 연결)
const oldGreeting = "안녕하세요, " + userName + "님! 나이는 " + userAge + "세 입니다.";
console.log(oldGreeting);

// 템플릿 리터럴 사용 (변수 삽입)
const newGreeting = `안녕하세요, ${userName}님! 나이는 ${userAge}세 입니다.`;
console.log(newGreeting);

// 여러 줄 문자열 작성
const multiLineText = `
  안녕하세요, ${userName}님!
  저희 웹사이트에 오신 것을 환영합니다.
  즐거운 시간 되세요!
`;
console.log(multiLineText);

// 표현식 삽입
const price = 10000;
const quantity = 3;
console.log(`총 가격: ${price * quantity}원`); // 출력: 총 가격: 30000원
```

> [!TIP]
> 템플릿 리터럴은 특히 동적인 내용을 포함하는 메시지나 HTML 코드를 JavaScript에서 생성할 때 매우 유용합니다. 코드를 더 깔끔하고 직관적으로 만들어줍니다.

### 1-2-2) 유용한 문자열 메소드

문자열 데이터는 다양한 조작을 할 수 있는 유용한 기능들을 내장하고 있습니다. 이를 **메소드(Method)**라고 부릅니다. 문자열 변수 뒤에 점(`.`)을 찍고 메소드 이름을 적어 사용하며, 마치 문자열에게 특정 행동을 하도록 명령하는 것과 같습니다.

*   **.length**: (메소드가 아닌 **속성(Property)**) 문자열의 길이를 반환합니다.
*   **.toUpperCase()** / **.toLowerCase()**: 문자열을 모두 대문자 또는 소문자로 변경합니다.
    ```javascript
    let greeting = "Hello, World!";
    console.log(greeting.toUpperCase()); // "HELLO, WORLD!"
    console.log(greeting.toLowerCase()); // "hello, world!"
    ```
*   **.trim()**: 문자열의 양 끝에 있는 공백을 제거합니다. 사용자가 입력한 값의 불필요한 공백을 제거할 때 유용합니다.
    ```javascript
    let userInput = "   안녕하세요!   ";
    console.log(userInput.trim()); // "안녕하세요!"
    ```
*   **.indexOf(찾을_문자열)**: 문자열에서 특정 문자열이 처음으로 나타나는 위치(인덱스)를 반환합니다. 문자는 0번째부터 셉니다. 찾는 문자열이 없으면 `-1`을 반환합니다.
    ```javascript
    let text = "JavaScript is fun!";
    console.log(text.indexOf("Java")); // 0
    console.log(text.indexOf("fun"));  // 15
    console.log(text.indexOf("bye"));  // -1 (찾는 문자열이 없음)
    ```
*   **.slice(시작_인덱스, 끝_인덱스)**: 문자열의 일부를 잘라내어 새로운 문자열을 반환합니다. `끝_인덱스`는 포함하지 않습니다.
    ```javascript
    let text = "JavaScript";
    console.log(text.slice(0, 4)); // "Java" (0번째부터 4번째 앞까지)
    console.log(text.slice(4));    // "Script" (4번째부터 끝까지)
    ```
*   **.replace(바꿀_문자열, 새_문자열)**: 문자열에서 특정 문자열을 찾아 다른 문자열로 바꿉니다. 처음 발견된 하나만 바꿉니다.
    ```javascript
    let text = "I like HTML. HTML is easy.";
    console.log(text.replace("HTML", "JavaScript")); // "I like JavaScript. HTML is easy."
    ```
*   **.split(구분자)**: 문자열을 특정 `구분자`를 기준으로 잘라서 여러 개의 문자열이 담긴 **배열(Array)**로 만들어 반환합니다. 배열은 '값의 목록'이라고 생각하면 됩니다.
    ```javascript
    let fruits = "사과,바나나,딸기,오렌지";
    let fruitArray = fruits.split(",");
    console.log(fruitArray); // ["사과", "바나나", "딸기", "오렌지"]
    ```
> [!NOTE]
> 여기서 `.length`는 괄호`()`가 없는 **속성(Property)**이고, `.toUpperCase()`처럼 괄호가 있는 것들은 **메소드(Method)**입니다. 속성은 데이터의 특징(값)을 나타내고, 메소드는 데이터가 할 수 있는 동작(기능)을 나타낸다고 생각하면 쉽습니다.

### 1-3) 불리언 (Boolean)

`Boolean` 타입은 `true`(참) 또는 `false`(거짓) 두 가지 값만 가집니다. 주로 조건문에서 어떤 조건이 맞는지 틀리는지를 판단할 때 사용됩니다.

```javascript
let isStudent = true;
let isAdult = false;

console.log(isStudent); // true
console.log(isAdult); // false
```

### 1-4) undefined 와 null

`undefined`와 `null`은 '값이 없음'을 나타내지만 약간의 차이가 있습니다.

*   **undefined**: 변수를 선언했지만 아직 아무런 값도 할당하지 않았을 때 JavaScript 엔진이 자동으로 부여하는 값입니다. "아직 값이 정해지지 않았다"는 의미입니다.
*   **null**: 개발자가 의도적으로 '값이 비어있음'을 나타내기 위해 사용하는 값입니다. "현재는 비어있지만, 나중에 값이 채워질 수도 있다" 또는 "값이 없다"는 것을 명시적으로 표현할 때 사용합니다.

```javascript
let futureJob;
console.log(futureJob); // undefined (아직 값이 할당되지 않음)

let score = null;
console.log(score); // null (의도적으로 '값이 없음'을 설정함)
```

> [!NOTE]
> `undefined`는 시스템이 정해주는 '값이 정해지지 않은 상태'이고, `null`은 개발자가 명시적으로 지정하는 '빈 값'이라는 미묘한 차이를 기억해두세요.

### 1-5) `typeof` 연산자로 타입 확인하기

변수에 어떤 종류의 데이터가 들어있는지 확인하고 싶을 때 `typeof` 연산자를 사용할 수 있습니다. 코드의 현재 상태를 파악하고 오류를 찾는 데 매우 유용합니다.

```javascript
console.log(typeof 17);        // "number"
console.log(typeof "홍길동");    // "string"
console.log(typeof true);      // "boolean"

let something;
console.log(typeof something); // "undefined"
console.log(typeof null);      // "object"
```
> [!IMPORTANT]
> `typeof null`의 결과가 `"object"`인 것은 JavaScript의 초기 버전부터 존재했던 유명한 버그입니다. 하지만 수많은 웹사이트들이 이 동작에 의존하고 있어 하위 호환성을 위해 수정되지 않고 있습니다. `null`은 객체가 아닌 원시 타입이라는 점을 꼭 기억하세요!

## 2) 연산자 (Operators)

연산자는 특정 연산을 수행하는 기호입니다. 변수와 값들을 조합하여 새로운 값을 만들어낼 수 있습니다.

### 2-1) 산술 연산자

숫자 계산에 사용되는 연산자입니다.

*   `+` (더하기)
*   `-` (빼기)
*   `*` (곱하기)
*   `/` (나누기)
*   `%` (나머지)

```javascript
let x = 10;
let y = 4;

console.log(x + y); // 14
console.log(x - y); // 6
console.log(x * y); // 40
console.log(x / y); // 2.5
console.log(x % y); // 2 (10을 4로 나눈 나머지)
```

`+` 연산자는 문자열을 연결하는 데에도 사용됩니다.

```javascript
let firstName = "길동";
let lastName = "홍";
console.log(lastName + firstName); // "홍길동"
```
> [!TIP]
> **연산자 우선순위**
>
> 수학에서처럼 JavaScript에서도 연산자에는 우선순위가 있습니다. 곱하기(`*`), 나누기(`/`)가 더하기(`+`), 빼기(`-`)보다 먼저 계산됩니다.
> `console.log(2 + 3 * 4);` // 14 (3 * 4가 먼저 계산됩니다.)
>
> 괄호 `( )`를 사용하면 계산 순서를 바꿀 수 있습니다.
> `console.log((2 + 3) * 4);` // 20 (괄호 안의 2 + 3이 먼저 계산됩니다.)

### 2-2) 할당 연산자

할당 연산자는 변수에 값을 할당(저장)하는 데 사용됩니다. 가장 기본적인 `=` 외에도 기존 값에 연산을 축약해서 적용하는 복합 할당 연산자들이 있습니다.

*   `=`: 오른쪽의 값을 왼쪽 변수에 할당합니다.
*   `+=`: 왼쪽 변수의 값에 오른쪽 값을 더한 후, 결과를 다시 왼쪽 변수에 할당합니다. (`x += y`는 `x = x + y`와 같습니다.)
*   `-=`: 왼쪽 변수의 값에서 오른쪽 값을 뺀 후, 결과를 다시 왼쪽 변수에 할당합니다.
*   `++`: 변수의 값을 1 증가시킵니다.
*   `--`: 변수의 값을 1 감소시킵니다.

```javascript
let count = 0;

count += 5; // count는 5가 됨
console.log(count); // 5

count -= 2; // count는 3이 됨
console.log(count); // 3

count++; // count는 4가 됨
console.log(count); // 4

count--; // count는 3이 됨
console.log(count); // 3
```

### 2-3) 비교 연산자

두 값을 비교하여 `true` 또는 `false`의 불리언 값을 반환합니다.

*   `===` (엄격한 동등): 타입과 값이 모두 같으면 `true`
*   `!==` (엄격한 불일치): 타입 또는 값이 다르면 `true`
*   `>` (보다 큼)
*   `<` (보다 작음)
*   `>=` (크거나 같음)
*   `<=` (작거나 같음)

```javascript
let a = 10;
let b = "10";
let c = 20;

console.log(a === 10);   // true
console.log(a === b);    // false (a는 숫자, b는 문자열이므로 타입이 다름)
console.log(a !== b);    // true
console.log(a < c);      // true
console.log(c >= 20);    // true
```

> [!WARNING]
> JavaScript에는 `==` (느슨한 동등) 연산자도 있지만, 이 연산자는 타입을 자동으로 변환하여 비교하기 때문에 예측하지 못한 결과를 낳을 수 있습니다. 예를 들어, `10 == "10"`은 `true`가 됩니다. 이런 혼란을 피하기 위해 항상 **타입과 값까지 정확하게 비교하는 `===` (엄격한 동등) 연산자**를 사용하는 습관을 들이는 것이 좋습니다.

### 2-4) 논리 연산자

여러 개의 조건을 조합하여 하나의 `true` 또는 `false` 값을 만들 때 사용됩니다.

*   `&&` (AND): 두 조건이 **모두** `true`일 때만 `true`
*   `||` (OR): 두 조건 중 **하나라도** `true`이면 `true`
*   `!` (NOT): `true`는 `false`로, `false`는 `true`로 바꿈

```javascript
let age = 15;
let hasTicket = true;

// 나이가 13세 이상이고, 티켓도 있어야 입장 가능
let canEnter = age >= 13 && hasTicket;
console.log("입장 가능:", canEnter); // true

// 주말이거나, 할인 쿠폰이 있으면 할인 적용
let isWeekend = false;
let hasCoupon = true;
let getsDiscount = isWeekend || hasCoupon;
console.log("할인 적용:", getsDiscount); // true

// 입장 가능한 상태가 아니라면?
console.log("입장 불가:", !canEnter); // false
```

### 2-5) 주의: JavaScript의 자동 타입 변환 (Type Coercion)

JavaScript는 다른 데이터 타입끼리 연산을 시도할 때, 에러를 발생시키는 대신 "도와주려는" 목적으로 자동으로 타입을 변환하는 특징이 있습니다. 이를 **타입 강제 변환(Type Coercion)** 이라고 부릅니다. 이 유연함은 때때로 개발자의 의도와 다른 결과를 낳아 찾기 어려운 버그의 원인이 되기도 합니다.

```javascript
// 숫자와 문자열의 덧셈
console.log(100 + "1"); // "1001" (숫자 100이 문자열 "100"으로 변환되어 합쳐짐)

// 숫자와 문자열의 뺄셈
console.log(100 - "1"); // 99 (문자열 "1"이 숫자 1로 변환되어 계산됨)

// 숫자와 불리언의 연산
console.log(10 + true);  // 11 (true가 숫자 1로 변환됨)
console.log(10 + false); // 10 (false가 숫자 0으로 변환됨)

// 숫자와 배열의 연산
console.log(5 + [1, 2]); // "51,2" (배열이 문자열 "1,2"로 변환된 후 합쳐짐)
```

> [!WARNING]
> **예측 불가능한 결과와 버그**
>
> 위 예시처럼 JavaScript는 에러를 발생시키지 않고 어떻게든 결과를 만들어냅니다. 이러한 동작은 코드가 조용히, 그리고 예상치 못하게 잘못된 값을 가지게 만들어 버그를 유발할 수 있습니다.
>
> 따라서 항상 `===`를 사용하여 엄격하게 타입을 비교하고, `typeof`를 통해 변수의 타입을 명확히 인지하며 코드를 작성하는 것이 중요합니다. 의도치 않은 타입 변환을 항상 경계하는 습관을 들이세요.

### 2-6) Optional Chaining(?.)과 Nullish Coalescing(??)

JavaScript에서 객체나 변수에 값이 없을 때(`null` 또는 `undefined`) 발생하는 오류를 안전하게 처리하기 위해 ES2020에 도입된 유용한 문법입니다.

#### Optional Chaining (`?.`)

**Optional Chaining (`?.`)**은 객체의 속성에 접근할 때, 해당 속성이 `null` 또는 `undefined`인 경우 오류를 발생시키지 않고 `undefined`를 반환하도록 해줍니다. 마치 '만약 이 속성이 있다면 접근하고, 없다면 그냥 `undefined`라고 알려줘'라고 말하는 것과 같습니다.

이전에는 객체의 속성에 접근하기 전에 해당 속성이 존재하는지 일일이 `if` 문으로 확인해야 했습니다. 하지만 `?.`를 사용하면 코드를 훨씬 간결하게 작성할 수 있습니다.

```javascript
const user = {
  name: '김철수',
  address: {
    city: '서울',
    zipCode: '12345'
  },
  contact: null // contact 속성이 null입니다.
};

console.log(user.name);             // 출력: 김철수
console.log(user.address.city);     // 출력: 서울

// Optional Chaining 사용 전: user.contact가 null이므로 오류 발생
// console.log(user.contact.email); // TypeError: Cannot read properties of null (reading 'email')

// Optional Chaining 사용 후: user.contact가 null이므로 오류 없이 undefined 반환
console.log(user.contact?.email);   // 출력: undefined

// user.profile이 없으므로 undefined 반환
console.log(user.profile?.name);    // 출력: undefined

// 배열에서도 사용 가능
const arr = [1, 2];
console.log(arr?.[0]); // 출력: 1
console.log(arr?.[2]); // 출력: undefined

// 함수 호출에서도 사용 가능
const someFunc = undefined;
// someFunc(); // TypeError: someFunc is not a function
someFunc?.(); // 오류 없이 undefined 반환
```

> [!NOTE]
> Optional Chaining은 특히 중첩된 객체 속성에 접근할 때 코드의 안정성을 높여줍니다. 데이터가 존재하지 않을 가능성이 있는 경우에 유용하게 사용됩니다.

#### Nullish Coalescing (`??`)

**Nullish Coalescing (`??`)** 연산자는 왼쪽 피연산자가 `null` 또는 `undefined`일 때만 오른쪽 피연산자의 값을 반환하고, 그렇지 않으면 왼쪽 피연산자의 값을 반환합니다. 이는 `||`(OR) 연산자와 비슷해 보이지만 중요한 차이가 있습니다.

*   `||` (OR) 연산자: 왼쪽 피연산자가 `false`, `0`, `''`(빈 문자열), `null`, `undefined`일 때 오른쪽 값을 반환합니다. (falsy 값)
*   `??` (Nullish Coalescing) 연산자: 왼쪽 피연산자가 오직 `null` 또는 `undefined`일 때만 오른쪽 값을 반환합니다.

```javascript
const userInput = null;
const defaultName = '게스트';

// || 연산자 사용 예시
const nameWithOR = userInput || defaultName;
console.log(nameWithOR); // 출력: 게스트 (userInput이 null이므로 defaultName 사용)

const zeroValue = 0;
const resultWithOR = zeroValue || '기본값';
console.log(resultWithOR); // 출력: 기본값 (zeroValue가 0이므로 기본값 사용 - 의도치 않을 수 있음)

// ?? 연산자 사용 예시
const nameWithNullish = userInput ?? defaultName;
console.log(nameWithNullish); // 출력: 게스트 (userInput이 null이므로 defaultName 사용)

const resultWithNullish = zeroValue ?? '기본값';
console.log(resultWithNullish); // 출력: 0 (zeroValue가 0이지만 null이나 undefined가 아니므로 0 사용 - 의도한 대로 동작)

const emptyString = '';
const textWithNullish = emptyString ?? '입력 없음';
console.log(textWithNullish); // 출력: '' (emptyString이 null이나 undefined가 아니므로 빈 문자열 사용)
```

> [!TIP]
> `??` 연산자는 `0`이나 `''`(빈 문자열)과 같은 유효한 값들을 기본값으로 대체하지 않고 그대로 사용하고 싶을 때 매우 유용합니다. 이는 사용자 입력이나 설정 값 등에서 `null` 또는 `undefined`와 `0`이나 `''`를 명확히 구분해야 할 때 특히 중요합니다.

### 2-7) 비트 연산자 (Bitwise Operators)

비트 연산자는 컴퓨터가 데이터를 처리하는 가장 기본적인 단위인 **비트(bit)** 수준에서 연산을 수행합니다. 일반적으로는 자주 사용되지 않지만, 그래픽 처리나 데이터 압축, 암호화 등 저수준(low-level) 프로그래밍이나 특정 알고리즘에서는 매우 유용할 수 있습니다.

> [!NOTE]
> **비트(Bit)란?**
> 컴퓨터는 모든 정보를 0과 1의 조합으로 저장하고 이해합니다. 이 0 또는 1 하나를 '비트'라고 부릅니다. 예를 들어, 숫자 5는 이진수(2진법)로 `101`로 표현됩니다. 비트 연산자는 바로 이 이진수 표현을 가지고 연산을 합니다.

주요 비트 연산자는 다음과 같습니다.

*   `&` (AND): 두 비트가 모두 1일 때만 1을 반환합니다.
*   `|` (OR): 두 비트 중 하나라도 1이면 1을 반환합니다.
*   `^` (XOR): 두 비트가 다를 때 1을 반환합니다.
*   `~` (NOT): 비트를 반전시킵니다. 0은 1로, 1은 0으로 바꿉니다.
*   `<<` (Left Shift): 비트를 왼쪽으로 지정된 수만큼 이동시킵니다.
*   `>>` (Right Shift): 비트를 오른쪽으로 지정된 수만큼 이동시킵니다.

```javascript
let a = 5;  // 이진수: 0101
let b = 3;  // 이진수: 0011

// AND 연산
//   0101 (5)
// & 0011 (3)
// --------
//   0001 (1)
console.log(a & b); // 1

// OR 연산
//   0101 (5)
// | 0011 (3)
// --------
//   0111 (7)
console.log(a | b); // 7

// XOR 연산
//   0101 (5)
// ^ 0011 (3)
// --------
//   0110 (6)
console.log(a ^ b); // 6

// Left Shift: 5를 왼쪽으로 1비트 이동. 0101 -> 1010 (10). x << y는 x * (2**y)와 비슷합니다.
console.log(a << 1); // 10

// Right Shift: 5를 오른쪽으로 1비트 이동. 0101 -> 0010 (2). x >> y는 x / (2**y)의 정수 부분과 비슷합니다.
console.log(a >> 1); // 2
```

> [!NOTE]
> 비트 연산자는 고급 주제이며, 일반적인 웹 프론트엔드 개발에서는 자주 사용되는 편은 아닙니다. 지금 당장 모든 것을 완벽하게 이해할 필요는 없습니다. '숫자를 0과 1의 조합으로 바꿔서 계산하는 특별한 연산자도 있구나' 정도로만 알아두고 넘어가도 충분합니다.


---

- [목차로 돌아가기](../README.md)
- [이전 강의로 이동](03-ES6-Variables-and-Scoping.md)
- [다음 강의로 이동](05-ES6-Conditional-Statements-and-Loops.md) 