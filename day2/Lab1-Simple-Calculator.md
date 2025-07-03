# Lab 1: 간단한 계산기 만들기

이번 실습에서는 Node.js 환경에서 동작하는 간단한 명령줄 인터페이스(CLI) 계산기를 만들어보면서, JavaScript의 기초와 Node.js의 기본적인 입출력 처리 방법을 배웁니다.

## 목표

- Node.js 환경에서 JavaScript 코드를 실행합니다.
- `readline` 모듈을 사용하여 사용자 입력을 받습니다.
- 입력받은 수식을 계산하고 결과를 출력합니다.

## 선수 지식

- [Node.js에서의 JavaScript](../day2/15-Node.js-JavaScript.md)

## 실습 과정

### 1. 프로젝트 설정

먼저, 계산기 프로젝트를 위한 폴더를 만들고 `calculator.js` 파일을 생성합니다.

### 2. JavaScript로 기능 구현하기

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
> - `readline.createInterface()`