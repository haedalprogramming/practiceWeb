# Lab3. 간단한 웹 서버 만들기

## 목표

Node.js의 내장 `http` 모듈을 사용하여 아주 간단한 웹 서버를 구축하고, 사용자의 요청(URL)에 따라 다른 응답을 제공하는 방법을 학습합니다. 이 실습을 통해 웹의 기본 동작 방식과 서버가 어떻게 클라이언트(웹 브라우저)의 요청에 응답하는지 이해하게 됩니다. 이는 웹 애플리케이션의 백엔드를 이해하는 데 중요한 첫걸음이 됩니다.

## 예상 소요 시간

45분

---

## 튜토리얼 시작하기

### 1단계: 프로젝트 초기 설정

웹 서버를 만들기 위한 JavaScript 파일을 준비합니다.

1.  `day2` 폴더 안에 `server.js`라는 이름의 새 파일을 만듭니다. 이 파일에 웹 서버의 코드를 작성할 것입니다.

### 2단계: 기본 웹 서버 생성

가장 먼저, 모든 요청에 대해 동일한 메시지를 응답하는 아주 기본적인 웹 서버를 만들어 보겠습니다. 이 단계에서는 Node.js의 `http` 모듈을 사용하여 서버를 생성하고 특정 포트에서 요청을 기다리도록 설정하는 방법을 배웁니다.

`server.js` 파일을 열고 다음 코드를 작성합니다.

```javascript
// server.js 파일

// Node.js의 내장 http 모듈을 불러옵니다.
// `http` 모듈은 웹 서버를 만들고 HTTP 요청을 처리하는 데 필요한 모든 핵심 기능을 제공합니다.
const http = require('http');

// 서버가 클라이언트의 요청을 기다릴 포트 번호를 정의합니다.
// 웹 서버는 특정 포트(예: 80, 443, 3000, 8080 등)를 통해 통신합니다.
// 3000번 포트는 개발 환경에서 웹 서버를 실행할 때 흔히 사용되는 포트 중 하나입니다.
const PORT = 3000;

// 웹 서버를 생성합니다.
// `http.createServer()` 메서드는 웹 서버 객체를 반환합니다.
// 이 메서드에는 클라이언트로부터 요청이 올 때마다 실행될 '요청 처리 함수'를 인자로 전달해야 합니다.
// 이 요청 처리 함수는 두 개의 중요한 매개변수를 가집니다:
//   `req` (Request): 클라이언트(웹 브라우저)가 서버로 보낸 요청에 대한 모든 정보를 담고 있는 객체입니다.
//                    예를 들어, 요청 URL, HTTP 메서드(GET, POST 등), 헤더 정보 등을 포함합니다.
//   `res` (Response): 서버가 클라이언트에게 보낼 응답에 대한 정보를 설정하고 실제 데이터를 보내는 데 사용되는 객체입니다.
const server = http.createServer((req, res) => {
  // 이 블록 안의 코드는 클라이언트로부터 HTTP 요청이 들어올 때마다 실행됩니다.

  // 응답 헤더를 설정합니다.
  // `res.writeHead()` 메서드는 HTTP 응답의 상태 코드와 헤더를 설정합니다.
  // 첫 번째 인자 `200`은 HTTP 상태 코드입니다. `200 OK`는 요청이 성공적으로 처리되었음을 의미합니다.
  // 두 번째 인자는 객체 형태로 헤더 정보를 담습니다.
  //   `'Content-Type': 'text/plain; charset=utf-8'`: 클라이언트에게 보내는 응답의 내용이 일반 텍스트(`text/plain`)이며,
  //                                                  문자 인코딩은 `utf-8`임을 알려줍니다. 한글이 깨지지 않도록 중요합니다.
  res.writeHead(200, { 'Content-Type': 'text/plain; charset=utf-8' });

  // 응답 본문을 전송하고 응답을 완료합니다.
  // `res.end()` 메서드는 클라이언트에게 보낼 실제 데이터를 작성하고, 응답 프로세스를 종료합니다.
  // 이 메서드가 호출되면 서버는 클라이언트에게 응답을 보내고 연결을 닫습니다.
  res.end('Hello, Web Server!');
});

// 서버가 특정 포트에서 클라이언트의 요청을 기다리도록 설정합니다.
// `server.listen()` 메서드는 서버를 시작하고 지정된 포트에서 들어오는 연결을 수신 대기합니다.
// 두 번째 인자는 서버가 성공적으로 시작되었을 때 실행될 콜백 함수입니다.
server.listen(PORT, () => {
  // 서버가 성공적으로 시작되면 콘솔에 메시지를 출력하여 사용자에게 알립니다.
  console.log(`서버 실행 중: http://localhost:${PORT}`);
  console.log('웹 브라우저를 열고 위 주소로 접속해보세요!');
});
```

**코드 상세 설명:**

*   **`const http = require('http');`**: Node.js에서 웹 서버를 구축하는 데 필요한 핵심 모듈인 `http`를 불러옵니다. `require()`는 Node.js 환경에서 다른 모듈을 가져올 때 사용되는 함수입니다.
*   **`const PORT = 3000;`**: 서버가 클라이언트의 요청을 들을(listen) 포트 번호를 `PORT`라는 상수에 정의했습니다. 웹 브라우저에서 이 포트 번호를 통해 서버에 접속하게 됩니다.
*   **`const server = http.createServer((req, res) => { ... });`**: 이 코드는 실제 웹 서버 객체를 생성합니다. `http.createServer()` 함수는 인자로 하나의 함수를 받는데, 이 함수는 클라이언트로부터 HTTP 요청이 들어올 때마다 Node.js에 의해 자동으로 호출됩니다. 이 함수는 두 개의 매개변수를 가집니다:
    *   **`req` (Request 객체)**: 클라이언트가 서버로 보낸 요청에 대한 모든 정보를 담고 있습니다. 예를 들어, 클라이언트가 어떤 주소(`req.url`)로 요청했는지, 어떤 방식으로(`req.method` - GET, POST 등) 요청했는지 등의 정보를 이 객체를 통해 얻을 수 있습니다.
    *   **`res` (Response 객체)**: 서버가 클라이언트에게 보낼 응답을 구성하는 데 사용되는 객체입니다. 이 객체를 통해 응답의 상태 코드, 헤더, 그리고 실제 데이터를 설정하고 보낼 수 있습니다.
*   **`res.writeHead(200, { 'Content-Type': 'text/plain; charset=utf-8' });`**: 이 줄은 HTTP 응답의 시작 부분인 **헤더(Header)**를 설정합니다. `200`은 HTTP 상태 코드 중 하나로, 요청이 성공적으로 처리되었음을 의미합니다. 두 번째 인자인 객체는 응답 헤더의 내용을 정의합니다. `'Content-Type': 'text/plain; charset=utf-8'`는 클라이언트에게 보내는 데이터가 일반 텍스트(`text/plain`)이며, 문자 인코딩은 `utf-8`임을 알려줍니다. 이렇게 설정해야 한글 메시지가 웹 브라우저에서 깨지지 않고 올바르게 표시됩니다.
*   **`res.end('Hello, Web Server!');`**: 이 줄은 클라이언트에게 보낼 실제 응답 데이터인 **본문(Body)**을 작성하고, 응답 프로세스를 종료합니다. `res.end()`가 호출되면 서버는 클라이언트에게 응답을 보내고, 해당 요청에 대한 연결을 닫습니다.
*   **`server.listen(PORT, () => { ... });`**: 이 코드는 위에서 생성한 `server` 객체가 `PORT` 변수에 지정된 포트 번호(여기서는 3000번)에서 클라이언트의 요청을 기다리도록 합니다. 서버가 성공적으로 시작되면, 두 번째 인자로 전달된 콜백 함수가 실행되어 콘솔에 서버가 실행 중임을 알리는 메시지를 출력합니다.

**테스트:**

터미널(명령 프롬프트 또는 PowerShell)을 열고 `day2` 폴더로 이동한 다음, 다음 명령어를 실행하여 웹 서버를 시작합니다.

```bash
node server.js
```

터미널에 `서버 실행 중: http://localhost:3000` 메시지가 출력되면, 웹 브라우저를 열고 주소창에 `http://localhost:3000`을 입력한 후 접속합니다. 웹 브라우저 화면에 `Hello, Web Server!` 메시지가 보이면 서버가 성공적으로 작동하고 있는 것입니다.

서버를 멈추려면 터미널에서 `Ctrl + C` (Windows/Linux) 또는 `Cmd + C` (macOS)를 누릅니다.

### 3단계: 요청 URL에 따른 다른 응답 처리 (라우팅)

이제 서버가 모든 요청에 대해 동일한 응답을 보내는 대신, 사용자가 어떤 주소(URL)로 접속했는지에 따라 다른 내용을 응답하도록 기능을 확장해 보겠습니다. 웹 애플리케이션에서는 사용자가 `/about`, `/products`, `/api/users` 등 다양한 경로로 접속할 수 있으며, 각 경로에 맞는 콘텐츠를 제공해야 합니다. 이렇게 요청 URL에 따라 다른 처리를 하는 것을 **라우팅(Routing)**이라고 합니다.

`server.js` 파일의 `http.createServer` 함수 내부를 다음과 같이 수정합니다. 기존의 `res.writeHead`와 `res.end` 부분을 `if...else if...else` 문으로 대체합니다.

```javascript
// server.js 파일 (이전 코드에 이어서)

const server = http.createServer((req, res) => {
  // `req.url`은 클라이언트가 요청한 URL 경로를 나타냅니다.
  // 예를 들어, 웹 브라우저에서 `http://localhost:3000/about`으로 접속하면 `req.url`은 `'/about'`이 됩니다.
  console.log(`요청 URL: ${req.url}`);

  // 요청 URL의 값에 따라 다른 응답 로직을 실행합니다.
  // `if...else if...else` 문을 사용하여 다양한 경로를 처리합니다.
  if (req.url === '/') {
    // 사용자가 웹 서버의 기본 주소(루트 경로)로 접속했을 때 (`http://localhost:3000/`)
    res.writeHead(200, { 'Content-Type': 'text/plain; charset=utf-8' });
    res.end('환영합니다!');
  } else if (req.url === '/about') {
    // 사용자가 `/about` 경로로 접속했을 때 (`http://localhost:3000/about`)
    res.writeHead(200, { 'Content-Type': 'text/plain; charset=utf-8' });
    res.end('이것은 소개 페이지입니다.');
  } else if (req.url === '/api/users') {
    // 사용자가 `/api/users` 경로로 접속했을 때 (`http://localhost:3000/api/users`)
    // 이 경로에서는 가상의 사용자 목록을 JSON 형태로 클라이언트에게 응답할 것입니다.
    const users = [
      { id: 1, name: 'Alice' },
      { id: 2, name: 'Bob' },
      { id: 3, name: 'Charlie' }
    ];
    // JavaScript 객체인 `users` 배열을 JSON 형식의 문자열로 변환합니다.
    // `JSON.stringify()` 메서드는 JavaScript 값을 JSON 문자열로 변환합니다.
    // `null, 2`는 JSON 문자열을 사람이 읽기 좋게 들여쓰기하여 변환하라는 옵션입니다.
    const jsonResponse = JSON.stringify(users, null, 2);

    // 응답 헤더의 `Content-Type`을 `application/json`으로 설정하여
    // 클라이언트(웹 브라우저)에게 보내는 데이터가 JSON 형식임을 명확히 알려줍니다.
    res.writeHead(200, { 'Content-Type': 'application/json; charset=utf-8' });
    res.end(jsonResponse); // 변환된 JSON 문자열을 응답 본문으로 보냅니다.
  } else {
    // 위에 정의된 어떤 경로에도 해당하지 않는 경우 (예: `http://localhost:3000/nonexistent`)
    // HTTP 상태 코드 404 (Not Found)를 설정하고, 페이지를 찾을 수 없다는 메시지를 보냅니다.
    res.writeHead(404, { 'Content-Type': 'text/plain; charset=utf-8' });
    res.end('404 Not Found: 페이지를 찾을 수 없습니다.');
  }
});

// ... (server.listen 코드는 이전과 동일하게 유지됩니다.)
```

**코드 상세 설명:**

*   **`console.log(`요청 URL: ${req.url}`);`**: 클라이언트로부터 요청이 들어올 때마다 어떤 URL로 요청이 왔는지 콘솔에 출력하여 서버의 동작을 쉽게 확인할 수 있도록 합니다. `req.url`은 현재 요청의 경로를 나타내는 문자열 속성입니다.
*   **`if (req.url === '/') { ... }`**: `req.url`이 `/` (루트 경로)와 정확히 일치하는지 확인합니다. 일치하면 `환영합니다!`라는 일반 텍스트 응답을 보냅니다. `res.writeHead`로 `Content-Type`을 `text/plain`으로 설정합니다.
*   **`else if (req.url === '/about') { ... }`**: `req.url`이 `/about`과 일치하는지 확인합니다. 일치하면 `이것은 소개 페이지입니다.`라는 일반 텍스트 응답을 보냅니다.
*   **`else if (req.url === '/api/users') { ... }`**: `req.url`이 `/api/users`와 일치하는지 확인합니다. 이 경로는 API 엔드포인트(데이터를 제공하는 주소) 역할을 합니다.
    *   **`const users = [ ... ];`**: 가상의 사용자 데이터를 JavaScript 배열 안에 객체 형태로 정의했습니다. 실제 애플리케이션에서는 이 데이터가 데이터베이스에서 오게 됩니다.
    *   **`const jsonResponse = JSON.stringify(users, null, 2);`**: `JSON.stringify()` 메서드를 사용하여 JavaScript 객체인 `users` 배열을 JSON 형식의 문자열로 변환합니다. 웹에서 데이터를 주고받을 때 JSON 형식은 매우 흔하게 사용됩니다. `null, 2` 인자는 JSON 문자열을 사람이 읽기 좋게 들여쓰기하여 변환하도록 지시합니다.
    *   **`res.writeHead(200, { 'Content-Type': 'application/json; charset=utf-8' });`**: JSON 데이터를 응답할 때는 `Content-Type`을 `application/json`으로 설정해야 합니다. 이렇게 해야 웹 브라우저나 다른 클라이언트 프로그램이 이 응답이 JSON 데이터임을 인식하고 올바르게 처리할 수 있습니다.
*   **`else { ... }`**: 위에 나열된 어떤 `if` 또는 `else if` 조건에도 해당하지 않는 모든 요청에 대해 실행되는 블록입니다. 이는 주로 존재하지 않는 페이지에 대한 요청일 때 사용됩니다.
    *   **`res.writeHead(404, { 'Content-Type': 'text/plain; charset=utf-8' });`**: HTTP 상태 코드 `404`는 `Not Found`를 의미합니다. 클라이언트가 요청한 리소스(페이지)를 서버에서 찾을 수 없음을 알려주는 표준 응답입니다.
    *   **`res.end('404 Not Found: 페이지를 찾을 수 없습니다.');`**: 사용자에게 페이지를 찾을 수 없다는 메시지를 보냅니다.

**테스트:**

서버를 다시 시작해야 합니다. 터미널에서 `Ctrl + C` (Windows/Linux) 또는 `Cmd + C` (macOS)를 눌러 현재 실행 중인 서버를 멈춘 후, 다음 명령어를 다시 실행합니다.

```bash
node server.js
```

서버가 다시 시작되면, 웹 브라우저를 열고 다음 주소들로 접속하여 각각 다른 응답이 오는지 확인합니다.

*   `http://localhost:3000/` : `환영합니다!` 메시지가 표시되어야 합니다.
*   `http://localhost:3000/about` : `이것은 소개 페이지입니다.` 메시지가 표시되어야 합니다.
*   `http://localhost:3000/api/users` : JSON 형식의 사용자 데이터가 표시되어야 합니다. (브라우저에 따라 JSON이 예쁘게 보일 수도, 그냥 텍스트로 보일 수도 있습니다.)
*   `http://localhost:3000/anything-else` (존재하지 않는 경로) : `404 Not Found: 페이지를 찾을 수 없습니다.` 메시지가 표시되어야 합니다.

각 주소로 접속할 때마다 터미널에 `요청 URL: /`, `요청 URL: /about` 등 요청 URL이 출력되는지도 확인해보세요. 이를 통해 서버가 어떤 요청을 받았는지 알 수 있습니다.

### 4단계: HTML 파일 응답하기 (선택 사항, 심화)

지금까지는 서버가 일반 텍스트나 JSON 데이터를 응답했지만, 실제 웹 페이지처럼 HTML 파일을 응답할 수도 있습니다. 이 단계는 조금 더 심화된 내용이지만, 웹 서버가 어떻게 웹 페이지를 제공하는지 이해하는 데 매우 중요합니다.

1.  `day2` 폴더 안에 `index.html`이라는 이름의 새 파일을 만들고 다음 HTML 내용을 복사하여 붙여넣습니다.

    ```html
    <!-- day2/index.html 파일 내용 -->
    <!DOCTYPE html>
    <html lang="ko">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>나의 첫 웹 페이지</title>
        <style>
            body { font-family: Arial, sans-serif; text-align: center; margin-top: 50px; }
            h1 { color: #333; }
            p { color: #666; }
        </style>
    </head>
    <body>
        <h1>환영합니다!</h1>
        <p>이것은 Node.js 서버가 제공하는 HTML 페이지입니다.</p>
        <button onclick="alert('버튼이 클릭되었습니다!')">클릭해보세요</button>
    </body>
    </html>
    ```

    이 HTML 파일은 간단한 제목, 문단, 그리고 JavaScript `alert()` 함수가 연결된 버튼을 포함하고 있습니다.

2.  `server.js` 파일을 수정하여 루트 경로(`req.url === '/'`) 요청 시 `index.html` 파일을 읽어 응답하도록 변경합니다. 이를 위해 `fs` 모듈을 다시 불러와야 합니다.

    ```javascript
    // server.js 파일 (이전 코드에 이어서)

    const http = require('http');
    // HTML 파일을 읽기 위해 Node.js의 파일 시스템 모듈을 다시 불러옵니다.
    const fs = require('fs');

    const PORT = 3000;

    const server = http.createServer((req, res) => {
      console.log(`요청 URL: ${req.url}`);

      if (req.url === '/') {
        // 루트 경로 요청 시 `index.html` 파일을 읽어 응답합니다.
        // `fs.readFile()`은 파일을 비동기적으로 읽습니다. 즉, 파일 읽기가 완료될 때까지 기다리지 않고
        // 다음 코드를 실행하며, 파일 읽기가 끝나면 콜백 함수를 호출합니다.
        fs.readFile('./index.html', (err, data) => {
          if (err) {
            // 파일 읽기 중 오류가 발생하면 500 Internal Server Error 상태 코드를 보냅니다.
            res.writeHead(500, { 'Content-Type': 'text/plain; charset=utf-8' });
            res.end('서버 오류: HTML 파일을 읽을 수 없습니다.');
            return; // 오류 처리 후 함수를 종료합니다.
          }
          // 파일 읽기가 성공하면, 응답 헤더의 `Content-Type`을 `text/html`로 설정합니다.
          // 이렇게 해야 웹 브라우저가 받은 데이터를 HTML 문서로 해석하고 화면에 렌더링합니다.
          res.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' });
          res.end(data); // 읽어온 HTML 파일의 내용을 응답 본문으로 보냅니다.
        });
      } else if (req.url === '/about') {
        // 기존 `/about` 경로 처리 로직은 동일하게 유지됩니다.
        res.writeHead(200, { 'Content-Type': 'text/plain; charset=utf-8' });
        res.end('이것은 소개 페이지입니다.');
      } else if (req.url === '/api/users') {
        // 기존 `/api/users` 경로 처리 로직은 동일하게 유지됩니다.
        const users = [
          { id: 1, name: 'Alice' },
          { id: 2, name: 'Bob' },
          { id: 3, name: 'Charlie' }
        ];
        const jsonResponse = JSON.stringify(users, null, 2);
        res.writeHead(200, { 'Content-Type': 'application/json; charset=utf-8' });
        res.end(jsonResponse);
      } else {
        // 기존 404 Not Found 처리 로직은 동일하게 유지됩니다.
        res.writeHead(404, { 'Content-Type': 'text/plain; charset=utf-8' });
        res.end('404 Not Found: 페이지를 찾을 수 없습니다.');
      }
    });

    // server.listen 코드는 이전과 동일하게 유지됩니다.
    server.listen(PORT, () => {
      console.log(`서버 실행 중: http://localhost:${PORT}`);
      console.log('웹 브라우저를 열고 위 주소로 접속해보세요!');
    });
    ```

**코드 상세 설명:**

*   **`const fs = require('fs');`**: `server.js` 파일 상단에 `fs` 모듈을 다시 불러오는 코드를 추가했습니다. 이 모듈은 파일을 읽는 데 필요합니다.
*   **`fs.readFile('./index.html', (err, data) => { ... });`**: `fs` 모듈의 `readFile` 함수를 사용하여 `index.html` 파일을 읽습니다. `readFile`은 `readFileSync`와 달리 **비동기적**으로 작동합니다. 즉, 파일을 읽는 동안에도 다른 작업을 계속 수행할 수 있습니다. 파일 읽기가 완료되면, 첫 번째 인자로 오류(`err`)를, 두 번째 인자로 파일 내용(`data`)을 받는 콜백 함수가 실행됩니다.
    *   **`if (err) { ... }`**: 파일 읽기 중 오류가 발생하면 `err` 객체에 오류 정보가 담깁니다. 이 경우, 서버는 `500 Internal Server Error` 상태 코드를 클라이언트에게 보내고 오류 메시지를 응답합니다.
    *   **`res.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' });`**: HTML 파일을 응답할 때는 `Content-Type`을 `text/html`로 설정해야 합니다. 이렇게 해야 웹 브라우저가 받은 데이터를 단순한 텍스트가 아닌 HTML 문서로 인식하고, 웹 페이지를 올바르게 렌더링(화면에 그리는)할 수 있습니다.
    *   **`res.end(data);`**: `index.html` 파일에서 읽어온 HTML 내용을 클라이언트에게 응답 본문으로 보냅니다.

**테스트:**

서버를 다시 시작(`Ctrl + C`로 멈춘 후 `node server.js` 재실행)하고 `http://localhost:3000/`으로 접속합니다. 이제 `Hello, Web Server!` 텍스트 대신 `index.html` 파일의 내용이 웹 페이지 형태로 표시되어야 합니다. 페이지 안의 버튼을 클릭했을 때 `alert` 창이 뜨는 것도 확인해보세요.

## 마무리

이 실습을 통해 여러분은 Node.js의 `http` 모듈을 사용하여 웹 서버를 만들고, 요청 URL에 따라 다른 응답을 제공하는 기본적인 라우팅 개념을 익혔습니다. 또한, 정적인 HTML 파일을 클라이언트에게 제공하는 방법도 경험했습니다. 이는 웹 애플리케이션의 백엔드를 이해하는 데 중요한 첫걸음이 됩니다. 더 나아가, Node.js 서버가 단순히 텍스트를 보내는 것을 넘어, 실제 웹 페이지를 구성하는 HTML, CSS, JavaScript 파일을 클라이언트에게 전달하여 동적인 웹 애플리케이션을 만들 수 있다는 가능성을 엿볼 수 있습니다.

---

- [목차로 돌아가기](../README.md)
- [이전 강의로 이동](Lab2-Grade-Processor.md)
- [다음 강의로 이동](../../day3/README.md)
