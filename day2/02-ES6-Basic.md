# 02. ES6+ 기본 문법

## 콘솔 출력

콘솔 출력은, 원하는 문자열을 화면에 출력하는 것을 의미합니다.

JavaScript에서 콘솔 출력은 `console.log()` 함수를 사용합니다.

```javascript
console.log('Hello, World!');
```

## 문(statement)

문(statement)은 JavaScript 코드의 최소 단위로서, 어떤 작업을 수행하는 문법 구조와 명령어를 말합니다.

예를 들어, 아래와 같은 코드는 문(statement)입니다.

```javascript
console.log('Hello, World!');
```

서로 다른 문은 세미콜론(;)으로 구분합니다.

```javascript
console.log('a'); console.log('b');
```

## 세미콜론

원칙적으로 세미콜론(;)으로 문을 구분하지만, 아래와 같이 세미콜론 없이 줄바꿈만으로 문을 구분할 수 있습니다.

```javascript
console.log('a')
console.log('b')
```


## 주석

주석을 통해 코드에 대한 설명이나 메모를 남길 수 있습니다. 주석은 코드 실행에 영향을 주지 않습니다.

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
  인자로 받은 메시지를 콘솔에 출력합니다.
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
let a = 2;
*/
console.log(a);
```

> [!TIP]
> Visual Studio Code 등 많은 코드 편집기에서 주석 단축키 `Ctrl + /` 를 지원합니다. 원하는 영역을 드래그한 후 단축키를 누르면 주석 처리되고, 다시 누르면 해제됩니다.

> [!WARNING]
> 중첩 주석은 지원되지 않습니다. `/* */` 안에 `/* */` 를 작성하면 오류가 발생합니다.