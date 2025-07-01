# 09. ES6+ 고급 기능

## 1) Iterator와 Generator
- **Iterator**: 순서가 있는 값들을 하나씩 꺼내는 방법을 정의합니다.
- **Generator**: `function*`과 `yield`를 사용하여 Iterator를 쉽게 만들 수 있어요.

```javascript
// Generator 함수로 ID 생성기 만들기
function* idGenerator() {
  let id = 1;
  while (true) {
    yield id++;           // 호출할 때마다 다음 값을 꺼내줌
  }
}

const gen = idGenerator();
console.log(gen.next().value); // 1
console.log(gen.next().value); // 2
```

## 2) Map과 Set
- **Map**: 키(key)와 값(value)을 한 쌍으로 저장합니다.
- **Set**: 중복 없는 유일한 값들만 저장합니다.

```javascript
// Map 예시
const map = new Map();
map.set('apple', 3);
map.set('banana', 2);
console.log(map.get('apple')); // 3

// Set 예시
const set = new Set([1, 2, 2, 3]);
console.log(set); // Set {1, 2, 3}
```

> [!TIP]
> 객체(Object)는 문자열 키만 지원하는 반면, Map은 모든 타입의 키를 사용할 수 있어요.

## 3) Optional Chaining(?.)과 Nullish Coalescing(??)
- **Optional Chaining (`?.`)**: 중간에 값이 없으면 `undefined`를 반환해 오류를 막아줍니다.
- **Nullish Coalescing (`??`)**: `null` 또는 `undefined`인 경우에만 기본값을 사용합니다.

```javascript
const user = { profile: { name: '철수' } };
console.log(user.profile?.name);        // '철수'
console.log(user.contact?.email);      // undefined (오류 없이 종료)

const input = null;
const text = input ?? '입력 없음';
console.log(text); // '입력 없음'
```

## 4) Proxy와 Reflect
- **Proxy**: 객체의 동작(읽기, 쓰기 등)을 가로채어 원하는 대로 바꿀 수 있어요.
- **Reflect**: 객체 작동을 기본 방식대로 수행할 수 있는 도구입니다.

```javascript
// Proxy 예시: 읽기(log)를 가로채기
const person = { name: '영희' };
const proxyPerson = new Proxy(person, {
  get(target, prop) {
    console.log(prop + ' 속성에 접근함');
    return Reflect.get(target, prop);
  }
});
console.log(proxyPerson.name); // 'name 속성에 접근함' 출력 후 '영희'
```

---

이제 모던 자바스크립트의 주요 문법과 고급 기능을 모두 살펴보았습니다! 다음으로는 브라우저와 Node.js 환경에서 자바스크립트를 어떻게 활용하는지 배워봅니다. 