# 12. 크로스 플랫폼 개발

## 1) 크로스 플랫폼 개발
- 한 번 작성한 JavaScript 코드를 **브라우저**와 **Node.js** 환경 모두에서 사용할 수 있도록 만드는 방법입니다.
- 중복된 코드를 줄이고, 유지보수를 쉽게 해줍니다.

## 2) 환경 감지하기
- 코드를 실행하는 환경(브라우저 또는 Node.js)을 확인하여 분기할 수 있습니다.

```javascript
if (typeof window !== 'undefined') {
  console.log('브라우저 환경이에요');
} else {
  console.log('Node.js 환경이에요');
}
```

## 3) 공통 모듈 만들기
- 브라우저와 Node.js 모두 지원하는 **유틸리티 함수**를 하나의 파일에 작성할 수 있습니다.

```javascript
// utils.js
export function add(a, b) {
  return a + b;
}

// 브라우저에서 불러오기
// <script type="module" src="main.js"></script>
// import { add } from './utils.js';

// Node.js에서 불러오기
// import { add } from './utils.js';
```

## 4) 번들링(Bundling)
- 여러 개의 파일을 하나로 묶어주는 도구(번들러)를 사용하면 브라우저 호환성과 속도가 좋아집니다.
- 대표 도구: **Webpack**, **Vite**

```bash
npm install --save-dev webpack webpack-cli
```

## 5) 조건부 코드 분기
- 환경별로 다른 코드를 실행하고 싶을 때 `if` 분기문을 사용합니다.

```javascript
function readConfig() {
  if (typeof window !== 'undefined') {
    return localStorage.getItem('config');
  } else {
    const fs = require('fs');
    return fs.readFileSync('config.json', 'utf8');
  }
}
```

> [!TIP]
> 브라우저 전용 API와 Node.js 전용 모듈을 섞어 쓰면 에러가 발생하므로, 항상 환경을 확인하세요.

## 6) 실습 예제
1. `utils.js`에 공통 함수 만들기(`add`, `multiply` 등)
2. 브라우저 `main.js`에서 화면에 결과 출력하기
3. Node.js `app.js`에서 콘솔에 결과 출력하기

---
