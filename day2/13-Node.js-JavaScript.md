# 12. Node.js JavaScript

## 1) Node.js 환경

**Node.js**는 웹 브라우저 없이 컴퓨터(서버) 위에서 JavaScript를 실행할 수 있게 해주는 '런타임(Runtime)' 환경입니다. 쉽게 말해, JavaScript가 웹 브라우저 밖에서도 동작할 수 있도록 만들어주는 프로그램입니다. 여러분이 웹사이트를 방문할 때, 그 웹사이트의 데이터와 기능을 제공하는 '서버'라는 컴퓨터가 있는데, Node.js는 바로 이 서버를 만들 때 주로 사용됩니다.

### Node.js가 필요한 이유

*   **풀스택 개발**: JavaScript 하나로 웹 페이지(프론트엔드)와 서버(백엔드)를 모두 개발할 수 있게 해줍니다. 이는 개발 효율성을 높이고, 프론트엔드 개발자가 백엔드 개발에도 쉽게 참여할 수 있도록 돕습니다.
*   **다양한 활용**: 웹 서버 개발 외에도 파일 처리, 데이터베이스 연결, 실시간 채팅 애플리케이션, 명령줄 도구(CLI) 등 다양한 종류의 프로그램을 만들 수 있습니다.
*   **높은 성능**: Node.js는 비동기 방식으로 동작하여 많은 요청을 효율적으로 처리할 수 있어, 실시간 데이터 처리나 대규모 트래픽이 발생하는 서비스에 적합합니다.

### 브라우저 JavaScript와 Node.js JavaScript의 차이점

| 특징           | 브라우저 JavaScript                               | Node.js JavaScript                                |
| :------------- | :------------------------------------------------ | :------------------------------------------------ |
| **실행 환경**  | 웹 브라우저 (Chrome, Firefox, Safari 등)          | Node.js 런타임 (컴퓨터 자체)                      |
| **주요 역할**  | 웹 페이지의 동적인 기능, 사용자 인터랙션          | 웹 서버, API 서버, 파일 시스템 제어, 데이터베이스 연동 |
| **접근 가능**  | DOM (HTML 요소), `window` 객체, 브라우저 API      | 파일 시스템, 운영체제 기능, 외부 라이브러리       |
| **모듈 시스템**| ES Modules (`import`/`export`)                  | CommonJS (`require`/`module.exports`) 기본, ES Modules 지원 |

> [!NOTE]
> Node.js는 JavaScript의 활용 범위를 웹 브라우저를 넘어 서버와 데스크톱 환경까지 확장시켜 주었습니다. 이는 JavaScript 개발자들에게 더 넓은 기회를 제공하고, 웹 기술을 기반으로 한 다양한 애플리케이션 개발을 가능하게 했습니다.

---

## 2) 파일 시스템 다루기 (fs 모듈)

Node.js는 컴퓨터의 파일 시스템에 직접 접근하여 파일을 읽고, 쓰고, 삭제하는 등의 작업을 할 수 있습니다. 이러한 기능을 제공하는 것이 바로 내장 모듈인 `fs` (File System) 모듈입니다. `fs` 모듈은 Node.js 환경에서만 사용할 수 있습니다.

### `fs` 모듈 사용하기

`fs` 모듈의 함수들은 대부분 비동기 방식으로 동작합니다. 비동기 방식은 파일 작업이 완료될 때까지 기다리지 않고 다음 코드를 실행하여 프로그램이 멈추지 않도록 합니다. 작업이 완료되면 콜백 함수를 통해 결과를 알려줍니다.

```javascript
// fs 모듈 불러오기: Node.js에서는 require() 함수를 사용하여 모듈을 가져옵니다.
const fs = require('fs');

// 1. 파일 읽기: fs.readFile(경로, 인코딩, 콜백함수)
fs.readFile('example.txt', 'utf8', (err, data) => {
  if (err) { // 오류가 발생하면 err 객체에 정보가 담깁니다.
    console.error('파일 읽기 오류:', err.message);
    return; // 오류 발생 시 함수 종료
  }
  console.log('파일 내용:', data);
});

// 2. 파일 쓰기: fs.writeFile(경로, 내용, 콜백함수)
const contentToWrite = '안녕하세요! Node.js로 작성된 파일입니다.';
fs.writeFile('output.txt', contentToWrite, err => {
  if (err) {
    console.error('파일 쓰기 오류:', err.message);
    return;
  }
  console.log('output.txt에 내용이 성공적으로 저장되었습니다.');
});

// 3. 파일 삭제: fs.unlink(경로, 콜백함수)
// fs.unlink('old_file.txt', err => {
//   if (err) {
//     console.error('파일 삭제 오류:', err.message);
//     return;
//   }
//   console.log('old_file.txt가 성공적으로 삭제되었습니다.');
// });

console.log('파일 작업 요청을 보냈습니다. 비동기적으로 처리됩니다.');
```

> [!TIP]
> `fs` 모듈에는 비동기 메서드 외에도 `readFileSync`, `writeFileSync`와 같이 이름에 `Sync`가 붙은 동기 메서드들도 있습니다. 동기 메서드는 작업이 완료될 때까지 다음 코드를 실행하지 않고 기다립니다. 이는 코드를 작성하기는 더 쉽지만, 파일 작업이 오래 걸릴 경우 프로그램 전체가 멈출 수 있으므로, 특별한 이유가 없다면 비동기 메서드를 사용하는 것이 좋습니다. 특히 웹 서버와 같이 여러 요청을 동시에 처리해야 하는 환경에서는 비동기 방식이 필수적입니다.

## 3) 간단한 HTTP 서버 만들기

Node.js의 가장 강력한 기능 중 하나는 웹 서버를 직접 만들 수 있다는 것입니다. 웹 서버는 웹 브라우저의 요청을 받아들이고, 요청에 맞는 웹 페이지나 데이터를 응답으로 보내주는 역할을 합니다. Node.js의 내장 `http` 모듈을 사용하면 아주 간단하게 웹 서버를 구축할 수 있습니다.

```javascript
// http 모듈 불러오기: 웹 서버를 만드는 데 필요한 기능을 제공합니다.
const http = require('http');

// 웹 서버 생성
// createServer() 메서드는 클라이언트(웹 브라우저)의 요청이 올 때마다 실행될 함수를 인자로 받습니다.
// 이 함수는 두 개의 매개변수를 가집니다: req (요청 객체), res (응답 객체)
const server = http.createServer((req, res) => {
  // req (Request): 클라이언트가 서버로 보낸 요청에 대한 정보를 담고 있습니다. (예: 요청 URL, HTTP 메서드)
  // res (Response): 서버가 클라이언트에게 보낼 응답에 대한 정보를 설정합니다.

  // 응답 헤더 설정: 클라이언트에게 보낼 응답의 정보를 설정합니다.
  // 200: HTTP 상태 코드 (성공을 의미)
  // {'Content-Type': 'text/plain; charset=utf-8'}: 응답 내용이 일반 텍스트이고, 한글을 포함할 수 있도록 UTF-8로 인코딩되었음을 알립니다.
  res.writeHead(200, {'Content-Type': 'text/plain; charset=utf-8'});

  // 응답 본문 전송 및 연결 종료
  // res.end() 메서드는 클라이언트에게 보낼 실제 내용을 작성하고, 응답을 완료합니다.
  res.end('안녕하세요! Node.js 서버입니다. 웹 브라우저에서 이 메시지를 볼 수 있습니다.');
});

// 서버가 특정 포트에서 요청을 기다리도록 설정합니다.
// listen(포트번호, 콜백함수): 서버가 시작되면 콜백 함수가 실행됩니다.
const PORT = 3000;
server.listen(PORT, () => {
  console.log(`서버 실행 중: http://localhost:${PORT}`);
  console.log('웹 브라우저를 열고 위 주소로 접속해보세요!');
});

// 이 코드를 app.js로 저장하고 터미널에서 node app.js를 실행한 후,
// 웹 브라우저에서 http://localhost:3000 에 접속하면 '안녕하세요! Node.js 서버입니다.' 메시지를 볼 수 있습니다.

## 4) 모듈 시스템 (CommonJS vs ES Modules)

Node.js는 JavaScript 코드를 모듈화하여 관리하는 두 가지 주요 시스템을 지원합니다. 하나는 Node.js가 오랫동안 사용해온 **CommonJS** 방식이고, 다른 하나는 웹 브라우저와 Node.js 모두에서 사용되는 최신 표준인 **ES Modules** 방식입니다.

### 4-1) CommonJS (기본 모듈 시스템)

CommonJS는 Node.js의 기본 모듈 시스템입니다. `require()` 함수를 사용하여 다른 모듈을 가져오고, `module.exports` 객체를 사용하여 현재 모듈의 내용을 내보냅니다.

```javascript
// math.js (CommonJS 모듈)

// add 함수를 외부로 내보냅니다.
function add(a, b) {
  return a + b;
}

// subtract 함수도 외부로 내보냅니다.
function subtract(a, b) {
  return a - b;
}

// module.exports 객체에 내보낼 함수들을 할당합니다.
module.exports = {
  add: add,
  subtract: subtract
};

// 또는 축약형으로:
// module.exports = {
//   add,
//   subtract
// };
```

```javascript
// app.js (CommonJS 모듈)

// math.js 모듈을 가져옵니다.
const math = require('./math.js'); // 파일 확장자 .js를 포함하는 것이 좋습니다.

console.log(math.add(5, 3));      // 출력: 8
console.log(math.subtract(10, 4)); // 출력: 6

// 특정 함수만 가져올 수도 있습니다.
const { add } = require('./math.js');
console.log(add(2, 3)); // 출력: 5
```

> [!NOTE]
> CommonJS는 Node.js 환경에서 오랫동안 사용되어 왔으며, 많은 기존 Node.js 프로젝트와 라이브러리들이 이 방식을 사용합니다.

### 4-2) ES Modules (최신 표준)

ES Modules는 JavaScript의 공식적인 모듈 시스템으로, `import`와 `export` 키워드를 사용합니다. 웹 브라우저와 Node.js 모두에서 사용될 수 있도록 설계되었습니다. Node.js에서 ES Modules를 사용하려면 파일 확장자를 `.mjs`로 하거나, `package.json` 파일에 `"type": "module"`을 설정해야 합니다.

```javascript
// math.mjs (ES Modules)

// add 함수를 내보냅니다.
export function add(a, b) {
  return a + b;
}

// subtract 함수도 내보냅니다.
export function subtract(a, b) {
  return a - b;
}

// default export도 가능합니다.
// export default function multiply(a, b) { return a * b; }
```

```javascript
// app.mjs (ES Modules)

// math.mjs 모듈에서 add 함수를 가져옵니다.
import { add, subtract } from './math.mjs'; // 파일 확장자 .mjs를 반드시 포함해야 합니다.

console.log(add(5, 3));      // 출력: 8
console.log(subtract(10, 4)); // 출력: 6

// default export를 가져올 때는 중괄호 없이 원하는 이름으로 가져옵니다.
// import multiply from './math.mjs';
// console.log(multiply(2, 3));
```

> [!TIP]
> 새로운 Node.js 프로젝트를 시작한다면 ES Modules를 사용하는 것이 좋습니다. 이는 웹 브라우저와 Node.js 환경 간의 코드 공유를 용이하게 하고, JavaScript의 최신 표준을 따르기 때문입니다. 하지만 기존 CommonJS 프로젝트와 함께 작업할 때는 두 모듈 시스템의 차이점을 이해하고 적절히 사용하는 것이 중요합니다.

---

## 5) npm 패키지 관리

**npm(Node Package Manager)**은 Node.js 생태계에서 가장 크고 인기 있는 패키지 관리자입니다. npm은 전 세계 개발자들이 만든 수많은 오픈소스 라이브러리(패키지)들을 쉽게 설치하고 관리할 수 있도록 도와주는 도구입니다. 마치 스마트폰의 앱 스토어처럼, 필요한 기능을 가진 패키지를 검색하고 다운로드하여 여러분의 프로젝트에 추가할 수 있습니다.

### `package.json` 파일

`package.json` 파일은 Node.js 프로젝트의 '설명서'와 같습니다. 이 파일에는 프로젝트의 이름, 버전, 설명, 작성자, 그리고 가장 중요한 **의존성(dependencies)** 정보가 담겨 있습니다. 의존성은 여러분의 프로젝트가 작동하기 위해 필요한 다른 패키지들을 의미합니다.

`npm init` 명령어를 사용하여 `package.json` 파일을 생성할 수 있습니다.

```bash
npm init -y # -y 옵션은 모든 질문에 기본값으로 응답하여 빠르게 package.json을 생성합니다.
```

### npm 명령어

*   **`npm install <package-name>`**: 특정 패키지를 프로젝트에 설치합니다. 설치된 패키지는 `node_modules` 폴더에 저장되고, `package.json` 파일의 `dependencies` 또는 `devDependencies`에 기록됩니다.

    ```bash
npm install express # Express 웹 프레임워크 설치
npm install lodash  # Lodash 유틸리티 라이브러리 설치
```

*   **`npm install`**: `package.json` 파일에 기록된 모든 의존성 패키지들을 한 번에 설치합니다. 새로운 프로젝트를 시작하거나, 다른 사람이 만든 프로젝트를 내려받았을 때 이 명령어를 실행하여 필요한 모든 패키지를 설치합니다.

    ```bash
npm install
```

*   **`npm uninstall <package-name>`**: 설치된 패키지를 프로젝트에서 제거합니다. `node_modules` 폴더에서 해당 패키지를 삭제하고, `package.json`에서도 의존성 정보를 제거합니다.

    ```bash
npm uninstall express
```

*   **`npm run <script-name>`**: `package.json` 파일의 `scripts` 섹션에 정의된 명령어를 실행합니다. 개발 서버를 시작하거나, 테스트를 실행하거나, 코드를 빌드하는 등의 작업을 자동화할 때 사용됩니다.

    ```json
    // package.json 예시
    {
      "name": "my-app",
      "version": "1.0.0",
      "scripts": {
        "start": "node app.js",
        "test": "mocha",
        "build": "webpack"
      }
    }
    ```

    ```bash
npm run start # node app.js 명령어를 실행합니다.
npm run test  # mocha 명령어를 실행합니다.
```

> [!TIP]
> npm은 Node.js 개발의 핵심 도구입니다. npm을 통해 수많은 오픈소스 패키지를 활용하여 개발 시간을 단축하고, 더 강력한 애플리케이션을 만들 수 있습니다. `package.json` 파일과 `node_modules` 폴더는 Git과 같은 버전 관리 시스템에 포함시키지 않는 것이 일반적입니다. 대신 `package.json`만 공유하고, 다른 개발자가 `npm install`을 실행하여 필요한 패키지를 설치하도록 합니다.

---

## 6) 환경 변수 (process.env)

**환경 변수(Environment Variables)**는 프로그램이 실행되는 환경에 따라 달라지는 값들을 저장하는 변수입니다. 예를 들어, 데이터베이스 접속 정보, API 키, 서버 포트 번호 등은 개발 환경과 실제 서비스 환경에서 다를 수 있습니다. 이러한 값들을 코드 안에 직접 작성(하드코딩)하는 대신 환경 변수로 관리하면, 코드 수정 없이 환경에 따라 다른 값을 사용할 수 있어 매우 유용합니다.

Node.js에서는 `process.env` 객체를 통해 환경 변수에 접근할 수 있습니다. `process`는 현재 Node.js 프로세스에 대한 정보를 담고 있는 전역 객체이며, `env`는 환경 변수들을 속성으로 가지고 있는 객체입니다.

```javascript
// app.js

// 환경 변수 PORT가 설정되어 있으면 그 값을 사용하고, 없으면 기본값 3000을 사용합니다.
const port = process.env.PORT || 3000;

// 환경 변수 NODE_ENV가 'production'이면 '운영 모드', 아니면 '개발 모드'를 출력합니다.
const environment = process.env.NODE_ENV === 'production' ? '운영 모드' : '개발 모드';

console.log('서버 포트:', port);
console.log('현재 환경:', environment);

// API 키와 같은 민감한 정보도 환경 변수로 관리할 수 있습니다.
const apiKey = process.env.API_KEY;
if (apiKey) {
  console.log('API 키가 설정되었습니다.');
} else {
  console.log('API 키가 설정되지 않았습니다.');
}
```

### 환경 변수 설정 방법

환경 변수를 설정하는 방법은 운영체제에 따라 다릅니다.

*   **Linux/macOS (터미널)**:
    ```bash
    PORT=4000 NODE_ENV=production API_KEY=your_secret_key node app.js
    ```

*   **Windows (명령 프롬프트)**:
    ```cmd
    set PORT=4000&&set NODE_ENV=production&&set API_KEY=your_secret_key&&node app.js
    ```

*   **Windows (PowerShell)**:
    ```powershell
    $env:PORT=4000; $env:NODE_ENV="production"; $env:API_KEY="your_secret_key"; node app.js
    ```

> [!TIP]
> 환경 변수는 보안상 중요한 정보(데이터베이스 비밀번호, API 키 등)를 코드에 직접 노출하지 않고 관리하는 데 필수적입니다. 또한, 개발, 테스트, 운영 등 다양한 환경에서 동일한 코드를 사용하면서도 환경에 맞는 설정을 적용할 수 있게 해줍니다. 실제 프로젝트에서는 `dotenv`와 같은 라이브러리를 사용하여 `.env` 파일에 환경 변수를 저장하고 관리하는 것이 일반적입니다.

---

- [목차로 돌아가기](../README.md)
- [이전 강의로 이동](11-Browser-JavaScript.md)
- [다음 강의로 이동](../../day3/README.md)
