# 11. 고급 기능

## 목차
1. [Iterator와 Generator](#1-iterator와-generator)
   - [1-1) Iterator (이터레이터)](#1-1-iterator-이터레이터)
   - [1-2) Generator (제너레이터)](#1-2-generator-제너레이터)
2. [Map과 Set](#2-map과-set)
   - [2-1) Map (맵)](#2-1-map-맵)
   - [2-2) Set (셋)](#2-2-set-셋)
3. [Proxy와 Reflect](#3-proxy와-reflect)
   - [3-1) Proxy (프록시)](#3-1-proxy-프록시)
   - [3-2) Reflect (리플렉트)](#3-2-reflect-리플렉트)

---

## 1) Iterator와 Generator

### 1-1) Iterator (이터레이터)

**Iterator**는 '순서가 있는 값들을 하나씩 꺼내는 방법을 정의한 규칙'입니다. 마치 책의 페이지를 한 장씩 넘기거나, 앨범의 사진을 한 장씩 보는 것처럼, 데이터 컬렉션(배열 등)의 요소들을 순서대로 접근할 수 있게 해주는 표준화된 방법입니다.

JavaScript의 배열(`Array`), 문자열(`String`), Map, Set 등은 모두 기본적으로 Iterator를 가지고 있어서 `for...of` 반복문으로 쉽게 순회할 수 있습니다.

```javascript
const numbers = [1, 2, 3];

// for...of 반복문은 Iterator를 사용하여 배열의 요소를 하나씩 꺼냅니다.
for (const num of numbers) {
  console.log(num); // 1, 2, 3 순서대로 출력
}

const myString = "Hello";
for (const char of myString) {
  console.log(char); // H, e, l, l, o 순서대로 출력
}
```

> [!NOTE]
> Iterator는 데이터를 '소비'하는 방식입니다. 한 번 꺼낸 값은 다시 꺼낼 수 없습니다. 지난 값을 다시 꺼내려면 새로 이터레이션을 진행해야 합니다.

### 1-2) Generator (제너레이터)

**Generator**는 Iterator를 쉽게 만들 수 있도록 도와주는 특별한 함수입니다. 일반 함수와 달리, Generator 함수는 실행을 중간에 멈췄다가 다시 이어서 실행할 수 있습니다.

Generator 함수는 `function*` 키워드로 정의하고, `yield` 키워드를 사용하여 값을 '생성'하고 함수의 실행을 일시 중지합니다. `next()` 메서드를 호출할 때마다 `yield` 다음의 값이 반환되고, 함수는 다음 `yield`를 만날 때까지 멈춰있습니다.

```javascript
// idGenerator는 Generator 함수입니다. function* 로 정의합니다.
function* idGenerator() {
  let id = 1;
  while (true) {
    yield id++;           // yield는 값을 반환하고 함수의 실행을 일시 중지합니다.
  }
}

const gen = idGenerator(); // Generator 함수를 호출하면 Generator 객체가 반환됩니다.

console.log(gen.next());     // { value: 1, done: false } - 첫 번째 yield 값
console.log(gen.next());     // { value: 2, done: false } - 두 번째 yield 값
console.log(gen.next().value); // 3 - value 속성만 꺼내서 출력
console.log(gen.next().value); // 4
```

> [!TIP]
> Generator는 무한한 시퀀스를 생성하거나, 복잡한 비동기 작업을 순차적으로 처리할 때 유용하게 사용될 수 있습니다. 지금은 '필요할 때마다 값을 하나씩 만들어주는 특별한 함수' 정도로 이해하면 충분합니다.

---

## 2) Map과 Set

JavaScript에는 데이터를 저장하고 관리하는 다양한 방법이 있습니다. 배열과 객체 외에도 ES6에서 새롭게 도입된 `Map`과 `Set`은 특정 상황에서 매우 유용하게 사용될 수 있는 자료 구조입니다.

### 2-1) Map (맵)

**Map**은 키(key)와 값(value)을 한 쌍으로 저장한다는 점에서 객체(Object)와 비슷합니다. 하지만 Map은 객체보다 더 유연하고 강력한 기능을 제공합니다.

#### Map의 특징

*   **모든 타입의 키 허용**: 객체는 키로 문자열이나 심볼만 허용하지만, Map은 숫자, 불리언, 객체, 함수 등 **모든 데이터 타입을 키로 사용할 수 있습니다.**
*   **삽입 순서 유지**: Map은 요소들이 추가된 순서를 기억합니다. `for...of` 반복문 등으로 순회할 때 이 순서가 유지됩니다.
*   **성능**: 대량의 데이터를 추가하거나 삭제할 때 객체보다 더 나은 성능을 보일 수 있습니다.
*   **`size` 속성**: Map에 저장된 요소의 개수를 `size` 속성으로 쉽게 알 수 있습니다.

```javascript
// Map 생성
const myMap = new Map();

// 값 추가: set(key, value)
myMap.set('apple', 3); // 문자열 키
myMap.set(1, 'one');    // 숫자 키
myMap.set(true, '참');   // 불리언 키

const objKey = { id: 1 };
myMap.set(objKey, '객체를 키로 사용'); // 객체를 키로 사용

// 값 가져오기: get(key)
console.log(myMap.get('apple')); // 출력: 3
console.log(myMap.get(1));      // 출력: 'one'
console.log(myMap.get(objKey)); // 출력: '객체를 키로 사용'

// Map의 크기 확인
console.log(myMap.size); // 출력: 4

// 값 존재 여부 확인: has(key)
console.log(myMap.has('apple')); // 출력: true
console.log(myMap.has('grape')); // 출력: false

// 값 삭제: delete(key)
myMap.delete('apple');
console.log(myMap.has('apple')); // 출력: false

// 모든 값 삭제: clear()
// myMap.clear();
// console.log(myMap.size); // 출력: 0

// Map 반복하기
for (const [key, value] of myMap) {
  console.log(`${key}: ${value}`);
}
// 출력:
// 1: one
// true: 참
// { id: 1 }: 객체를 키로 사용
```

### 2-2) Set (셋)

**Set**은 중복 없는 유일한 값들만 저장하는 특별한 컬렉션입니다. 수학에서 배운 집합 개념을 자료구조로 구현한 것이라고 생각하면 됩니다.

#### Set의 특징

*   **중복 허용 안 함**: Set에 같은 값을 여러 번 추가해도 한 번만 저장됩니다.
*   **순서 없음**: Set은 요소의 삽입 순서를 보장하지 않습니다. (하지만 대부분의 JavaScript 엔진은 삽입 순서를 유지합니다.)
*   **`size` 속성**: Set에 저장된 요소의 개수를 `size` 속성으로 쉽게 알 수 있습니다.

```javascript
// Set 생성
const mySet = new Set();

// 값 추가: add(value)
mySet.add(1); 
mySet.add(2); 
mySet.add(2); // 2는 이미 있으므로 추가되지 않습니다.
mySet.add(3); 
mySet.add('hello');
mySet.add({ a: 1 }); // 객체는 참조가 다르므로 중복으로 간주되지 않습니다.

console.log(mySet); // 출력: Set {1, 2, 3, 'hello', { a: 1 }}

// Set의 크기 확인
console.log(mySet.size); // 출력: 5

// 값 존재 여부 확인: has(value)
console.log(mySet.has(1));     // 출력: true
console.log(mySet.has(5));     // 출력: false

// 값 삭제: delete(value)
mySet.delete(2);
console.log(mySet); // 출력: Set {1, 3, 'hello', { a: 1 }}

// 모든 값 삭제: clear()
// mySet.clear();
// console.log(mySet.size); // 출력: 0

// Set 반복하기
for (const item of mySet) {
  console.log(item);
}
// 출력:
// 1
// 3
// hello
// { a: 1 }
```

> [!TIP]
> Map과 Set은 특정 상황에서 배열이나 일반 객체보다 더 효율적이고 직관적인 코드를 작성할 수 있게 해줍니다. 예를 들어, 고유한 값 목록을 관리하거나, 키-값 쌍의 데이터를 유연하게 다룰 때 유용합니다.

## 3) Proxy와 Reflect

`Proxy`와 `Reflect`는 객체의 동작을 가로채거나 조작할 수 있는 강력한 도구입니다. 이들은 객체 지향 프로그래밍에서 '메타 프로그래밍'이라고 불리는 영역에 속하며, 객체의 기본 동작을 커스터마이징할 때 사용됩니다.

### 3-1) Proxy (프록시)

**Proxy**는 특정 객체(`target`)의 작업을 가로채서(`trap`) 개발자가 정의한 방식으로 처리할 수 있게 해주는 객체입니다. 마치 어떤 요청이 실제 객체에 도달하기 전에 중간에서 가로채서 다른 작업을 수행하는 '대리인' 또는 '중개인'과 같습니다.

`new Proxy(target, handler)` 형태로 생성하며, `target`은 프록시할 객체, `handler`는 가로챌 작업을 정의하는 객체입니다.

```javascript
// 원본 객체
const person = { 
  name: '영희',
  age: 15
};

// Proxy 핸들러: 객체 속성에 접근(get)하는 작업을 가로챕니다.
const handler = {
  get(target, prop, receiver) {
    console.log(`'${String(prop)}' 속성에 접근함`); // 어떤 속성에 접근했는지 로그를 남깁니다.
    // Reflect.get을 사용하여 원본 객체의 속성 값을 가져옵니다.
    return Reflect.get(target, prop, receiver);
  },
  set(target, prop, value, receiver) {
    console.log(`'${String(prop)}' 속성을 '${value}'로 설정함`); // 속성 설정 시 로그를 남깁니다.
    // Reflect.set을 사용하여 원본 객체의 속성 값을 설정합니다.
    return Reflect.set(target, prop, value, receiver);
  }
};

// person 객체에 대한 Proxy를 생성합니다.
const proxyPerson = new Proxy(person, handler);

console.log(proxyPerson.name); // 'name' 속성에 접근함 출력 후 '영희'
proxyPerson.age = 16;          // 'age' 속성을 '16'로 설정함 출력 후 age가 16으로 변경
console.log(proxyPerson.age);  // 'age' 속성에 접근함 출력 후 16
```

> [!NOTE]
> Proxy는 객체의 속성 접근, 할당, 함수 호출 등 다양한 작업을 가로챌 수 있습니다. 이를 통해 유효성 검사, 로깅, 접근 제어, 데이터 바인딩 등 복잡한 기능을 구현할 수 있습니다.

### 3-2) Reflect (리플렉트)

**Reflect**는 객체에 대한 기본적인 작업을 수행하는 내장 객체입니다. `Reflect`의 메서드들은 `Proxy` 핸들러에서 사용될 때 특히 유용합니다. `Reflect`는 객체에 대한 작업을 수행하는 표준화된 방법을 제공하며, 기존의 `Object` 메서드보다 더 예측 가능하고 안전하게 동작합니다.

`Reflect`의 메서드들은 대부분 `Proxy` 핸들러의 `trap` 메서드와 이름이 동일합니다. 예를 들어, `Proxy`의 `get` 트랩 안에서 `Reflect.get`을 사용하여 원본 객체의 속성 값을 가져오는 것이 일반적입니다.

```javascript
const myObject = { a: 1, b: 2 };

// Reflect.get: 객체의 속성 값을 가져옵니다.
console.log(Reflect.get(myObject, 'a')); // 출력: 1

// Reflect.set: 객체의 속성 값을 설정합니다.
Reflect.set(myObject, 'c', 3);
console.log(myObject.c); // 출력: 3

// Reflect.has: 객체가 특정 속성을 가지고 있는지 확인합니다.
console.log(Reflect.has(myObject, 'b')); // 출력: true

// Reflect.deleteProperty: 객체의 속성을 삭제합니다.
Reflect.deleteProperty(myObject, 'a');
console.log(myObject); // 출력: { b: 2, c: 3 }
```

> [!TIP]
> `Proxy`와 `Reflect`는 일반적인 웹 개발에서는 자주 사용되지 않을 수 있는 고급 기능입니다. 하지만 프레임워크나 라이브러리를 만들거나, 객체의 동작을 세밀하게 제어해야 할 때 매우 강력한 도구가 됩니다. 지금은 이런 기능이 있다는 것을 알아두는 정도로 충분합니다.

---

- [목차로 돌아가기](../README.md)
- [이전 강의로 이동](10-ES6-Async-Patterns.md)
- [다음 강의로 이동](12-Browser-JavaScript.md) 