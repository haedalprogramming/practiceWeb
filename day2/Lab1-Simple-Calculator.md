# Lab 1: 간단한 계산기 만들기

## 목차
1. [목표](#1-목표)
2. [선수 지식](#2-선수-지식)
3. [실습 과정](#3-실습-과정)
   - [3-1) 프로젝트 설정](#3-1-프로젝트-설정)
   - [3-2) JavaScript로 기능 구현하기](#3-2-javascript로-기능-구현하기)
   - [3-3) 결과 확인](#3-3-결과-확인)

---

이번 실습에서는 Node.js 환경에서 동작하는 간단한 명령줄 인터페이스(CLI) 계산기를 만들어보면서, JavaScript의 기초와 Node.js의 기본적인 입출력 처리 방법을 배웁니다.

## 1) 목표

- Node.js 환경에서 JavaScript 코드를 실행합니다.
- `readline` 모듈을 사용하여 사용자 입력을 받습니다.
- 입력받은 수식을 계산하고 결과를 출력합니다.

## 2) 선수 지식

- [Node.js에서의 JavaScript](../day2/15-Node.js-JavaScript.md)

## 3) 실습 과정

### 3-1) 프로젝트 설정

먼저, 계산기 프로젝트를 위한 폴더를 만들고 `calculator.js` 파일을 생성합니다.

### 3-2) JavaScript로 기능 구현하기

`calculator.js` 파일에 다음 코드를 작성하여 계산기 기능을 구현합니다.

```javascript
const readline = require('readline');

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout
});

console.log("간단한 계산기입니다. 계산할 수식을 입력하세요. (예: 10 + 5)");
console.log("종료하려면 'exit'을 입력하세요.");

rl.on('line', (line) => {
  if (line.toLowerCase() === 'exit') {
    rl.close();
    return;
  }

  try {
    // eval 함수는 보안에 취약할 수 있으므로, new Function을 사용하여 좀 더 안전하게 실행합니다.
    const result = new Function('return ' + line)();
    if (isNaN(result)) {
        console.log("잘못된 수식입니다.");
    } else {
        console.log(`결과: ${result}`);
    }
  } catch (error) {
    console.error("계산 중 오류가 발생했습니다:", error.message);
    console.log("올바른 형식의 수식을 입력해주세요. (예: 10 * 5)");
  }

  console.log("\n다음 수식을 입력하세요:");
});

rl.on('close', () => {
  console.log("계산기를 종료합니다.");
  process.exit(0);
});
```

> [!NOTE]
> - `require('readline')`은 Node.js의 내장 모듈인 `readline`을 불러옵니다. 이 모듈은 터미널과 같은 읽기 가능한 스트림에서 한 줄씩 데이터를 읽기 위한 인터페이스를 제공합니다.
> - `readline.createInterface()`는 `readline` 인터페이스 객체를 생성합니다. `input`과 `output`으로 각각 표준 입력(`process.stdin`)과 표준 출력(`process.stdout`)을 지정하여, 터미널에서 입력을 받고 터미널로 결과를 출력할 수 있게 합니다.
> - `rl.on('line', callback)`은 사용자가 터미널에 한 줄을 입력하고 Enter 키를 누를 때마다 `callback` 함수를 실행합니다. 입력된 내용은 `line` 매개변수로 전달됩니다.
> - `new Function('return ' + line)()`은 문자열 형태의 수식을 계산하는 방법 중 하나입니다. `eval()`과 유사하지만, 전역 스코프에 접근하지 않아 상대적으로 더 안전합니다.
> - `isNaN(result)`는 계산 결과가 숫자인지 확인합니다. 숫자가 아니면 `true`를 반환합니다.
> - `rl.close()`는 `readline` 인터페이스를 닫고, 'close' 이벤트를 발생시킵니다.
> - `rl.on('close', callback)`은 인터페이스가 닫힐 때 `callback` 함수를 실행합니다.

### 3-3) 결과 확인

터미널에서 다음 명령어를 실행하여 계산기 프로그램을 실행합니다.

```bash
node calculator.js
```

프로그램이 실행되면, "10 + 5"와 같은 수식을 입력하고 Enter를 눌러보세요. 계산 결과가 출력되는 것을 확인할 수 있습니다. 계산을 마친 후 'exit'을 입력하면 프로그램이 종료됩니다.

---

- [목차로 돌아가기](../README.md)
- [이전 강의로 이동](16-Exception-Handling.md)
- [다음 실습: 투두리스트 만들기](Lab2-Todo-List.md)
