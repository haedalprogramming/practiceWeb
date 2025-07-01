# 11. Node.js JavaScript

## 1) Node.js 환경
- **Node.js**는 브라우저 없이 컴퓨터(서버) 위에서 JavaScript를 실행할 수 있게 해주는 프로그램입니다.
- 서버용 프로그램, 파일 처리, 데이터베이스 연결 등 다양한 작업에 사용됩니다.

## 2) 파일 시스템 다루기 (fs 모듈)
- `fs` 모듈을 사용하면 컴퓨터 속 파일을 읽고, 쓰고, 삭제할 수 있습니다.

```javascript
// fs 모듈 불러오기
const fs = require('fs');

// 파일 읽기 (비동기)
fs.readFile('example.txt', 'utf8', (err, data) => {
  if (err) {
    console.error('읽기 오류:', err);
  } else {
    console.log('파일 내용:', data);
  }
});

// 파일 쓰기 (비동기)
fs.writeFile('output.txt', 'Hello, Node.js!', err => {
  if (err) console.error('쓰기 오류:', err);
  else console.log('output.txt에 저장함');
});
```  

> [!TIP]
> `.readFileSync`, `.writeFileSync` 메서드를 사용하면 동기 방식으로 파일을 처리할 수 있지만, 서버 성능이 느려질 수 있어요.

## 3) 간단한 HTTP 서버 만들기
- `http` 모듈로 웹 서버를 직접 만들 수 있습니다.

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  res.writeHead(200, {'Content-Type': 'text/plain; charset=utf-8'});
  res.end('안녕하세요! Node.js 서버입니다.');
});

server.listen(3000, () => {
  console.log('서버 실행 중: http://localhost:3000');
});
```

## 4) 모듈 시스템 (CommonJS vs ES Modules)
- **CommonJS**: `require`와 `module.exports`를 사용합니다.
- **ES Modules**: `import`와 `export`를 사용합니다. 최신 Node.js에서 지원합니다.

```javascript
// CommonJS 예시 (기본)
const math = require('./math.js');
console.log(math.add(2, 3));

// ES Modules 예시
import { add } from './math.js';
console.log(add(2, 3));
```

## 5) npm 패키지 관리
- **npm**은 Node.js용 도구들을 모아놓은 저장소입니다.
- `npm init`으로 프로젝트 초기화, `npm install`로 다양한 패키지를 설치할 수 있습니다.

```bash
npm init -y          # package.json 파일 자동 생성
npm install express  # Express 웹 프레임워크 설치
```

## 6) 환경 변수 (process.env)
- 프로그램 실행 환경 정보를 저장하고 읽을 수 있습니다.

```javascript
// 매개변수 없이 실행: node app.js
// 환경 변수 설정: PORT=4000 node app.js
const port = process.env.PORT || 3000;
console.log('서버 포트:', port);
```

---