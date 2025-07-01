# 07. ES6+ 모듈

## 1) 모듈
- 모듈은 코드를 작은 파일로 나누어 관리하는 방법입니다.

## 2) 기본 export와 import
- **export**: 파일에서 함수나 변수를 다른 파일로 내보낼 때 사용합니다.
- **import**: 다른 파일에서 내보낸 내용을 불러올 때 사용합니다.

```javascript
// greeting.js 파일에서 내보내기
export function sayHello(name) {
  console.log('안녕, ' + name + '!');
}

// main.js 파일에서 불러오기
import { sayHello } from './greeting.js';

sayHello('철수'); // '안녕, 철수!'
```

## 3) default export
- **default export**는 파일당 한 번만 사용할 수 있고, import할 때 중괄호 `{}` 없이 불러옵니다.

```javascript
// utils.js 파일에서 기본 내보내기(default)
export default function greet() {
  console.log('환영합니다!');
}

// main.js 파일에서 불러오기
import greet from './utils.js';

greet(); // '환영합니다!'
```

> [!TIP]
> default export는 이름이 아닌 파일의 한 가지 대표 기능을 내보낼 때 사용합니다.

## 4) 모듈의 특징
- 각 모듈 파일은 **엄격 모드(strict mode)**로 실행되어, 더 안전한 코드를 작성할 수 있어요.
- 모듈 내부의 변수나 함수는 기본적으로 외부에 되지 않아, 이름 충돌을 방지합니다.

## 5) 브라우저와 Node.js에서의 모듈
- 브라우저: `<script type="module">` 속성을 사용하거나 번들러(webpack 등)를 통해 모듈을 불러옵니다.
- Node.js: ES 모듈 사용 시 `.mjs` 확장자 또는 `package.json`에 `"type": "module"` 설정이 필요합니다.

```html
<!-- 브라우저 예시 -->
<script type="module" src="main.js"></script>
```

```json
// Node.js 설정 예시 (package.json)
{
  "type": "module"
}
```

> [!TIP]
> Node.js 환경에서 ES 모듈을 사용할 때 파일 경로에 `.js` 확장자를 꼭 포함해야 에러를 피할 수 있습니다.

---