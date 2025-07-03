# 09. ES6+ 모듈

## 목차
1. [모듈](#1-모듈)
2. [기본 export와 import](#2-기본-export와-import)
3. [default export](#3-default-export)
4. [모듈의 특징](#4-모듈의-특징)
5. [브라우저와 Node.js에서의 모듈](#5-브라우저와-nodejs에서의-모듈)
   - [5-1) 브라우저 환경](#5-1-브라우저-환경)
   - [5-2) Node.js 환경](#5-2-nodejs-환경)

## 1) 모듈

모듈(Module)은 코드를 작은 파일로 나누어 관리하는 방법입니다. 마치 복잡한 기계를 만들 때 여러 개의 부품을 조립하는 것과 같습니다. 각 부품(모듈)은 독립적인 기능을 수행하며, 필요할 때 다른 부품과 결합하여 더 큰 기능을 만들 수 있습니다.

### 모듈을 사용하는 이유

*   **코드 분리 및 정리**: 하나의 파일에 모든 코드를 작성하면 파일이 너무 커지고 복잡해집니다. 모듈을 사용하면 관련된 코드들을 작은 파일로 나누어 관리할 수 있어 코드를 더 깔끔하게 정리할 수 있습니다.
*   **재사용성**: 한 번 만든 모듈은 다른 프로젝트나 다른 부분에서 필요할 때 쉽게 가져다 사용할 수 있습니다. 예를 들어, 날짜를 계산하는 모듈을 만들어두면, 여러 웹사이트에서 날짜 계산이 필요할 때마다 이 모듈을 재사용할 수 있습니다.
*   **의존성 관리**: 각 모듈이 어떤 다른 모듈에 의존하는지 명확하게 알 수 있어, 코드의 관계를 파악하기 쉽고 유지보수가 용이해집니다.
*   **이름 충돌 방지**: 각 모듈은 자신만의 독립적인 공간을 가지므로, 다른 모듈에서 같은 이름의 변수나 함수를 사용해도 서로 충돌하지 않습니다.

## 2) 기본 export와 import

모듈 시스템의 핵심은 코드를 '내보내고(export)' '가져오는(import)' 것입니다. 이를 통해 여러 파일에 분산된 코드들이 서로 필요한 부분을 주고받으며 하나의 큰 프로그램을 구성할 수 있습니다.

### `export`: 내보내기

`export` 키워드는 현재 파일(모듈)에 있는 변수, 함수, 클래스 등을 다른 파일에서 사용할 수 있도록 '내보낼' 때 사용합니다.

```javascript
// greeting.js 파일

// sayHello 함수를 다른 파일에서 사용할 수 있도록 내보냅니다.
export function sayHello(name) {
  console.log('안녕, ' + name + '!');
}

// 변수도 내보낼 수 있습니다.
export const PI = 3.14159;

// 클래스도 내보낼 수 있습니다.
export class MyClass {
  constructor() {
    console.log('MyClass가 생성되었습니다.');
  }
}
```

### `import`: 가져오기

`import` 키워드는 다른 파일(모듈)에서 `export`로 내보낸 내용을 현재 파일로 '가져올' 때 사용합니다.

`import`할 때는 내보낸 이름과 동일하게 중괄호 `{}` 안에 이름을 명시해야 합니다.

```javascript
// main.js 파일

// greeting.js 파일에서 내보낸 sayHello 함수를 가져옵니다.
import { sayHello, PI, MyClass } from './greeting.js';

sayHello('철수'); // 출력: '안녕, 철수!'
console.log(PI);  // 출력: 3.14159
const myInstance = new MyClass(); // 출력: MyClass가 생성되었습니다.

// 가져올 때 다른 이름으로 사용하고 싶다면 as 키워드를 사용합니다.
import { sayHello as greetUser } from './greeting.js';
greetUser('영희'); // 출력: '안녕, 영희!'
```

> [!NOTE]
> `import` 문에서 파일 경로를 지정할 때는 상대 경로(`./`, `../`) 또는 절대 경로를 사용합니다. 웹 브라우저 환경에서는 `.js`와 같은 파일 확장자를 반드시 포함해야 합니다.

## 3) default export

`default export`는 모듈에서 '단 하나의 대표적인 것'을 내보낼 때 사용합니다. 즉 파일을 대표하는 하나의 기능을 내보낼 때 사용합니다. `default export`는 파일당 한 번만 사용할 수 있으며, `import`할 때 중괄호 `{}` 없이 원하는 이름으로 불러올 수 있습니다.

```javascript
// utils.js 파일에서 기본 내보내기(default)
// 이 모듈의 대표 기능은 greet 함수입니다.
export default function greet() {
  console.log('환영합니다!');
}

// 또는 변수나 클래스도 default로 내보낼 수 있습니다.
// const myValue = 123;
// export default myValue;

// class MyDefaultClass { /* ... */ }
// export default MyDefaultClass;
```

```javascript
// main.js 파일에서 불러오기

// default export는 중괄호 없이 원하는 이름으로 가져올 수 있습니다.
import welcome from './utils.js'; // greet 함수를 welcome이라는 이름으로 가져옵니다.

welcome(); // 출력: '환영합니다!'

// 만약 default export와 일반 export를 함께 사용한다면:
// import welcome, { PI, MyClass } from './utils.js';
```

> [!TIP]
> `default export`는 모듈의 주요 기능이나, 모듈이 단일 책임을 가질 때 유용합니다. 예를 들어, 하나의 유틸리티 함수만 제공하는 모듈이나, 하나의 컴포넌트만 내보내는 모듈 등에 사용됩니다. `default export`는 이름이 아닌 파일의 한 가지 대표 기능을 내보낼 때 사용합니다.

## 4) 모듈의 특징

ES6 모듈은 JavaScript 코드를 더 체계적이고 안전하게 관리할 수 있도록 여러 가지 중요한 특징을 가지고 있습니다.

*   **엄격 모드(Strict Mode) 자동 적용**: 각 모듈 파일은 자동으로 엄격 모드(`'use strict';`)로 실행됩니다. 엄격 모드는 JavaScript 코드의 잠재적인 오류를 줄이고, 안전하지 않거나 비효율적인 문법 사용을 제한하여 더 견고한 코드를 작성할 수 있도록 돕습니다.

    > [!NOTE]
    > 엄격 모드에서는 선언되지 않은 변수를 사용하거나, `delete` 연산자를 변수에 사용하는 것과 같은 특정 동작이 오류를 발생시킵니다. 이는 개발자가 실수를 줄이고 더 나은 코딩 습관을 들이도록 유도합니다.

*   **독립적인 스코프 (Private by Default)**: 모듈 내부에서 선언된 변수, 함수, 클래스 등은 기본적으로 해당 모듈 안에서만 유효합니다. 즉, 모듈 외부에서는 직접 접근할 수 없습니다. 다른 모듈에서 사용하려면 반드시 `export` 키워드를 통해 명시적으로 내보내야 합니다. 이는 전역 스코프 오염을 방지하고, 모듈 간의 이름 충돌을 막아줍니다.

    ```javascript
    // myModule.js
    const privateVar = "나는 비밀 변수!"; // 이 변수는 이 모듈 안에서만 사용 가능합니다.

    export function publicFunction() {
      console.log("나는 공개 함수!");
      console.log(privateVar); // 모듈 내부에서는 privateVar에 접근 가능합니다.
    }
    ```

    ```javascript
    // main.js
    import { publicFunction } from './myModule.js';

    publicFunction(); // 출력: 나는 공개 함수! 
                      // 출력: 나는 비밀 변수!

    // console.log(privateVar); // 오류! privateVar는 myModule.js 내부에서만 유효합니다.
    ```

*   **단 한 번만 실행**: 모듈은 한 번만 로드되고 실행됩니다. 여러 번 `import`하더라도 코드가 중복해서 실행되지 않습니다. 이는 효율성을 높이고, 의도치 않은 부작용을 방지합니다.

> [!TIP]
> 모듈의 이러한 특징들은 대규모 애플리케이션을 개발할 때 코드의 복잡성을 관리하고, 협업을 용이하게 하며, 잠재적인 오류를 줄이는 데 큰 도움이 됩니다. 모듈은 현대 JavaScript 개발의 필수적인 부분입니다.

## 5) 브라우저와 Node.js에서의 모듈

ES6 모듈은 웹 브라우저와 Node.js 환경 모두에서 사용할 수 있지만, 각 환경에서 모듈을 로드하고 사용하는 방식에는 약간의 차이가 있습니다.

### 5-1) 브라우저 환경

웹 브라우저에서 ES6 모듈을 사용하려면, `<script>` 태그에 `type="module"` 속성을 추가해야 합니다. 이 속성을 추가하면 브라우저는 해당 스크립트 파일을 JavaScript 모듈로 인식하고, `import` 및 `export` 문법을 해석할 수 있게 됩니다.

```html
<!-- index.html 파일 -->
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>모듈 예제</title>
</head>
<body>
    <h1>브라우저 모듈 예제</h1>
    <script type="module" src="main.js"></script> <!-- type="module" 속성 중요! -->
</body>
</html>
```

```javascript
// main.js 파일
import { sayHello } from './greeting.js'; // 상대 경로 사용

sayHello('브라우저'); // 출력: 안녕, 브라우저!
```

```javascript
// greeting.js 파일
export function sayHello(name) {
  console.log('안녕, ' + name + '!');
}
```

> [!NOTE]
> 실제 웹 개발에서는 복잡한 모듈 의존성을 관리하고, 구형 브라우저 호환성을 위해 Webpack, Parcel, Vite와 같은 '번들러(Bundler)' 도구를 사용하기도 합니다. 이 도구들은 여러 JavaScript 파일을 하나로 합치고, 모듈 문법을 구형 브라우저에서도 이해할 수 있는 형태로 변환해주는 역할을 합니다.

### 5-2) Node.js 환경

Node.js는 기본적으로 CommonJS 모듈 시스템(`require()`, `module.exports`)을 사용했지만, ES6 모듈(`import`, `export`)도 지원합니다. Node.js에서 ES6 모듈을 사용하려면 몇 가지 설정이 필요합니다.

*   **`.mjs` 확장자 사용**: 파일 확장자를 `.mjs`로 지정하면 Node.js는 해당 파일을 ES 모듈로 인식합니다.

    ```javascript
    // app.mjs
    import { greet } from './utils.mjs';
    greet('Node.js');
    ```

    ```javascript
    // utils.mjs
    export function greet(name) {
      console.log(`Hello, ${name}!`);
    }
    ```

*   **`package.json`에 `"type": "module"` 설정**: 프로젝트의 `package.json` 파일에 `"type": "module"`을 추가하면, 해당 프로젝트의 모든 `.js` 파일이 기본적으로 ES 모듈로 처리됩니다.

    ```json
    // package.json
    {
      "name": "my-node-app",
      "version": "1.0.0",
      "type": "module", // 이 설정을 추가합니다.
      "main": "app.js"
    }
    ```

    ```javascript
    // app.js (이제 ES 모듈로 동작합니다.)
    import { greet } from './utils.js';
    greet('Node.js');
    ```

    ```javascript
    // utils.js (이제 ES 모듈로 동작합니다.)
    export function greet(name) {
      console.log(`Hello, ${name}!`);
    }
    ```

> [!TIP]
> Node.js 환경에서 ES 모듈을 사용할 때, `import` 문에서 파일 경로에 `.js` 또는 `.mjs`와 같은 파일 확장자를 꼭 포함해야 에러를 피할 수 있습니다. 브라우저와 달리 Node.js는 확장자 생략을 지원하지 않습니다.

---

- [목차로 돌아가기](../README.md)
- [이전 강의로 이동](08-ES6-Classes.md)
- [다음 강의로 이동](10-ES6-Async-Patterns.md)
