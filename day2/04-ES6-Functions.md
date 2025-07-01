# 04. ES6+ 함수

## 1) 함수
- 함수는 특정 작업을 수행하기 위한 코드들의 모임입니다.

```javascript
// sayHello라는 이름의 함수에 동작을 담아둠
function sayHello() {
  console.log('안녕하세요!');
}

sayHello(); // 함수 이름을 불러 동작을 실행 --> '안녕하세요!'
```

> [!TIP]
> 함수를 만들 때는 `function` 키워드, 함수 이름, 소괄호 `()`, 그리고 중괄호 `{}`를 사용합니다.

## 2) 매개변수(parameter)와 인자(argument)
- **매개변수**: 함수 이름 뒤 괄호 안의 빈 상자 역할을 하는 변수
- **인자**: 함수를 호출할 때 실제로 넣어주는 값

```javascript
function greet(name) {          // name은 매개변수
  console.log('안녕, ' + name + '!');
}

greet('철수');                  // '철수'는 인자
greet('영희');                  // '영희'는 인자
```

## 3) ES6+ 화살표 함수(Arrow Function)
- 함수를 더 짧게 쓸 수 있습니다.
- `this`가 헷갈릴 때 외부의 `this`를 그대로 사용합니다.

```javascript
// 일반 함수
function add(a, b) {
  return a + b;
}

// 화살표 함수 기본 형태
const addArrow = (a, b) => {
  return a + b;
};

// 중괄호와 return을 한 줄로 줄인 형태
const multiply = (a, b) => a * b;
```

> [!TIP]
> 한 줄짜리 로직은 `{}`와 `return`을 생략할 수 있습니다.

## 4) 기본 매개변수(Default Parameters)
- 인자를 넘기지 않을 때 사용할 기본값을 정할 수 있습니다.

```javascript
function makeTea(type = '홍차') {
  console.log(type + '가 준비되었습니다.');
}

makeTea();        // '홍차가 준비되었습니다.'
makeTea('녹차');  // '녹차가 준비되었습니다.'
```

## 5) 나머지 매개변수(Rest Parameters)
- 인자의 개수를 모를 때, 모두 모아서 배열로 받을 수 있습니다.

```javascript
function sumAll(...numbers) {      // numbers는 배열이 됨
  let total = 0;
  for (let num of numbers) {
    total += num;
  }
  return total;
}

sumAll(1, 2, 3);       // 6
sumAll(5, 10, 15, 20); // 50
```

> [!TIP]
> ES5의 `arguments` 객체 대신 `...rest`를 쓰면 배열 메서드를 바로 사용할 수 있어요.

## 6) 고차 함수(Higher-Order Function)
- 함수를 인자로 받거나, 함수를 결과로 **내보내는** 함수를 말합니다.
- 반복되는 로직을 줄이고, 필요한 부분만 바꾸어 재사용할 수 있습니다.

```javascript
// 함수를 반환하는 예시
function createMultiplier(factor) {
  return (number) => number * factor;
}

const double = createMultiplier(2);  // factor=2인 함수를 만듦
console.log(double(5));             // 10

// 함수를 인자로 받는 예시
function repeat(task, times) {
  for (let i = 0; i < times; i++) {
    task();
  }
}

repeat(() => console.log('🐱'), 3);
// 🐱
// 🐱
// 🐱
```

> [!TIP]
> 고차 함수를 사용하면 비슷한 코드를 줄이고, 필요한 부분만 다르게 바꿔서

---