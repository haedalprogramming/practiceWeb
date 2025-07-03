# 07. 배열과 객체 이해하기

## 목차
1. [배열(Array)](#1-배열array)
2. [배열 조작하기 (추가, 제거, 변경)](#2-배열-조작하기-추가-제거-변경)
3. [배열 반복과 주요 메서드](#3-배열-반복과-주요-메서드)
   - [3-1) `map()` 메서드](#3-1-map-메서드)
   - [3-2) `filter()` 메서드](#3-2-filter-메서드)
   - [3-3) `reduce()` 메서드](#3-3-reduce-메서드)
   - [3-4) `forEach()` 메서드](#3-4-foreach-메서드)
   - [3-5) `indexOf()`와 `includes()` 메서드](#3-5-indexof와-includes-메서드)
   - [3-6) `concat()` 메서드](#3-6-concat-메서드)
   - [3-7) `join()` 메서드](#3-7-join-메서드)
   - [3-8) `slice()`와 `splice()` 메서드](#3-8-slice와-splice-메서드)
4. [객체(Object)이란 무엇인가?](#4-객체object이란-무엇인가)
5. [객체 수정과 추가/삭제](#5-객체-수정과-추가삭제)
6. [객체 관련 유용한 메서드](#6-객체-관련-유용한-메서드)
   - [6-1) `Object.keys()`](#6-1-objectkeys)
   - [6-2) `Object.values()`](#6-2-objectvalues)
   - [6-3) `Object.entries()`](#6-3-objectentries)
   - [6-4) `hasOwnProperty()`](#6-4-hasownproperty)
7. [구조 분해 할당 (Destructuring Assignment)](#7-구조-분해-할당-destructuring-assignment)
   - [7-1) 배열 구조 분해](#7-1-배열-구조-분해)
   - [7-2) 객체 구조 분해](#7-2-객체-구조-분해)
8. [스프레드 연산자 (`...`) (Spread Operator)](#8-스프레드-연산자--spread-operator)
   - [8-1) 배열에서 스프레드 연산자 사용](#8-1-배열에서-스프레드-연산자-사용)
   - [8-2) 객체에서 스프레드 연산자 사용](#8-2-객체에서-스프레드-연산자-사용)
   - [8-3) 함수에서 스프레드 연산자 사용](#8-3-함수에서-스프레드-연산자-사용)

## 1) 배열(Array)

배열은 여러 개의 값을 순서대로 보관하는 자료 구조입니다. 마치 번호표가 붙어있는 사물함들이 줄지어 있는 것과 같습니다. 각 사물함에는 하나의 값이 들어있고, 우리는 번호표(인덱스)를 통해 원하는 사물함의 값을 꺼내거나 바꿀 수 있습니다.

### 배열을 사용하는 이유

*   **데이터 묶음**: 관련된 여러 데이터를 하나의 변수로 묶어서 관리할 수 있습니다. 예를 들어, 학생들의 점수 목록, 쇼핑 카트의 상품 목록 등을 배열로 만들 수 있습니다.
*   **순서 보장**: 데이터가 저장된 순서가 중요할 때 배열을 사용합니다. 예를 들어, 영화 상영 시간표처럼 순서가 중요한 정보에 적합합니다.
*   **반복 처리 용이**: 배열에 저장된 값들을 하나씩 꺼내서 반복적으로 처리하기 편리합니다.

```javascript
// 배열 만들기: 대괄호([]) 안에 값을 쉼표로 구분하여 작성합니다.
const fruits = ['사과', '바나나', '오렌지']; // 0번 사물함: 사과, 1번 사물함: 바나나, 2번 사물함: 오렌지

// 첫 번째 요소 꺼내기 (인덱스 0)
console.log(fruits[0]); // 출력: '사과' (0번 사물함의 값)

// 배열 길이 확인하기: 배열에 몇 개의 값이 들어있는지 알려줍니다.
console.log(fruits.length); // 출력: 3 (사물함이 3개)

// 다양한 타입의 값을 배열에 저장할 수 있습니다.
const mixedArray = ['텍스트', 123, true, null];
console.log(mixedArray[1]); // 출력: 123
```

> [!TIP]
> 배열의 인덱스는 항상 0부터 시작합니다. 즉, 첫 번째 값은 인덱스 0, 두 번째 값은 인덱스 1... 이런 식입니다. 마지막 요소는 `length - 1` 인덱스로 꺼낼 수 있습니다. 예를 들어, `fruits` 배열의 마지막 요소는 `fruits[fruits.length - 1]`로 접근할 수 있습니다.

---

## 2) 배열 조작하기 (추가, 제거, 변경)

배열은 한 번 만들면 끝이 아니라, 필요에 따라 안에 있는 값을 추가하거나, 제거하거나, 변경할 수 있습니다. 마치 사물함에 물건을 넣고 빼고 바꾸는 것과 같습니다.

*   **추가 (add)**: `push()` 메서드를 사용해 배열의 **맨 끝**에 새로운 값을 추가합니다.
*   **제거 (remove)**: `pop()` 메서드를 사용해 배열의 **맨 끝**에 있는 값을 꺼내면서 제거합니다. 제거된 값은 반환됩니다.
*   **변경 (update)**: 특정 인덱스에 새로운 값을 할당하여 기존 값을 덮어씁니다.

```javascript
const nums = [1, 2, 3];
console.log("초기 배열:", nums); // 출력: 초기 배열: [1, 2, 3]

// 값 추가: push()
nums.push(4); // 배열의 맨 뒤에 4를 추가합니다.
console.log("4 추가 후:", nums); // 출력: 4 추가 후: [1, 2, 3, 4]

nums.push(5, 6); // 여러 개의 값을 한 번에 추가할 수도 있습니다.
console.log("5, 6 추가 후:", nums); // 출력: 5, 6 추가 후: [1, 2, 3, 4, 5, 6]

// 값 제거: pop()
const lastRemoved = nums.pop(); // 배열의 맨 뒤에 있는 6을 제거하고 lastRemoved에 저장합니다.
console.log("제거된 값:", lastRemoved); // 출력: 제거된 값: 6
console.log("6 제거 후:", nums); // 출력: 6 제거 후: [1, 2, 3, 4, 5]

// 값 변경: 인덱스 사용
nums[1] = 20; // 인덱스 1번(두 번째) 값을 2에서 20으로 변경합니다.
console.log("인덱스 1 변경 후:", nums); // 출력: 인덱스 1 변경 후: [1, 20, 3, 4, 5]

// 존재하지 않는 인덱스에 값을 할당하면 배열의 길이가 늘어납니다.
nums[10] = 100; // 인덱스 10번에 100을 할당합니다. 중간은 비어있게 됩니다.
console.log("인덱스 10 추가 후:", nums); // 출력: 인덱스 10 추가 후: [1, 20, 3, 4, 5, <5 empty items>, 100]
console.log("새로운 배열 길이:", nums.length); // 출력: 새로운 배열 길이: 11
```

> [!NOTE]
> `push()`와 `pop()`은 배열의 맨 끝에서만 작동합니다. 배열의 중간이나 앞에서 값을 추가하거나 제거하는 메서드도 있지만, 지금은 가장 기본적인 `push()`와 `pop()`을 이해하는 것이 중요합니다.

---

## 3) 배열 반복과 주요 메서드

배열에 저장된 여러 값들을 하나씩 꺼내서 어떤 작업을 하고 싶을 때 `map()`과 `filter()`와 같은 유용한 메서드들을 사용합니다.

### 3-1) `map()` 메서드

`map()` 메서드는 배열의 각 요소를 변형하여 **새로운 배열**을 만들 때 사용합니다. 원본 배열은 변경되지 않습니다.

```javascript
const originalNumbers = [1, 2, 3, 4, 5];

// map()을 사용하여 각 숫자에 2를 곱한 새로운 배열 만들기
const doubledNumbers = originalNumbers.map(n => n * 2); // n은 originalNumbers의 각 요소를 의미합니다.

console.log(originalNumbers); // 출력: [1, 2, 3, 4, 5] (원본 배열은 그대로 유지됩니다.)
console.log(doubledNumbers);  // 출력: [2, 4, 6, 8, 10] (새로운 배열이 생성되었습니다.)

// map()을 사용하여 각 숫자를 문자열로 변형하기
const stringNumbers = originalNumbers.map(n => `숫자 ${n}`);
console.log(stringNumbers); // 출력: ['숫자 1', '숫자 2', '숫자 3', '숫자 4', '숫자 5']
```

### 3-2) `filter()` 메서드

`filter()` 메서드는 배열의 요소들 중에서 특정 조건을 만족하는 요소들만 골라내어 **새로운 배열**을 만들 때 사용합니다.

```javascript
const allNumbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// filter()를 사용하여 짝수만 골라내기
const evenNumbers = allNumbers.filter(n => n % 2 === 0); // n % 2 === 0은 n이 짝수인지 확인하는 조건입니다.

console.log(allNumbers);   // 출력: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10] (원본 배열은 그대로 유지됩니다.)
console.log(evenNumbers);  // 출력: [2, 4, 6, 8, 10] (새로운 배열이 생성되었습니다.)

// filter()를 사용하여 5보다 큰 숫자만 골라내기
const greaterThanFive = allNumbers.filter(n => n > 5);
console.log(greaterThanFive); // 출력: [6, 7, 8, 9, 10]
```

> [!TIP]
> `map()`과 `filter()` 메서드 안에는 주로 화살표 함수(`=>`)를 사용합니다. 화살표 함수는 코드를 간결하게 만들어주기 때문에, 배열의 각 요소를 처리하는 로직을 한눈에 파악하기 쉽게 해줍니다. 이 두 메서드는 원본 배열을 변경하지 않고 새로운 배열을 반환한다는 중요한 특징이 있습니다. 이는 원본 데이터를 보호하면서 필요한 데이터를 가공할 수 있게 해줍니다.

### 3-3) `reduce()` 메서드

`reduce()` 메서드는 배열의 모든 요소를 하나로 **축소(reduce)**하여 단일 값을 생성할 때 사용합니다. 예를 들어, 배열의 모든 숫자를 더하거나, 평균을 구하거나, 특정 조건에 맞는 요소의 개수를 세는 등의 작업에 활용됩니다.

```javascript
const numbersToReduce = [1, 2, 3, 4, 5];

// 모든 숫자의 합계 구하기
// accumulator: 이전 콜백의 반환 값 (또는 초기값)
// currentValue: 현재 처리 중인 배열 요소
const sum = numbersToReduce.reduce((accumulator, currentValue) => {
  return accumulator + currentValue;
}, 0); // 0은 accumulator의 초기값입니다.

console.log(sum); // 출력: 15 (1+2+3+4+5)

// 배열의 모든 요소를 문자열로 연결하기
const words = ['Hello', ' ', 'World', '!'];
const sentence = words.reduce((acc, current) => acc + current, '');
console.log(sentence); // 출력: Hello World!
```

> [!NOTE]
> `reduce()`는 배열의 요소를 순회하며 콜백 함수를 실행하고, 그 결과를 다음 순회에 전달합니다. 초기값을 제공하지 않으면 배열의 첫 번째 요소가 초기값으로 사용됩니다.

### 3-4) `forEach()` 메서드

`forEach()` 메서드는 배열의 각 요소에 대해 주어진 함수를 한 번씩 실행합니다. `for` 반복문과 비슷하게 배열의 모든 요소를 순회하지만, `forEach()`는 `return` 값을 반환하지 않고 단순히 각 요소에 대한 작업을 수행할 때 사용됩니다.

```javascript
const fruits = ['사과', '바나나', '오렌지'];

fruits.forEach((fruit, index) => {
  console.log(`${index}: ${fruit}`);
});
// 출력:
// 0: 사과
// 1: 바나나
// 2: 오렌지
```

### 3-5) `indexOf()`와 `includes()` 메서드

*   **`indexOf()`**: 배열에서 특정 요소가 처음으로 나타나는 인덱스(위치)를 반환합니다. 요소가 배열에 없으면 `-1`을 반환합니다.
*   **`includes()`**: 배열에 특정 요소가 포함되어 있는지 여부를 `true` 또는 `false`로 반환합니다.

```javascript
const animals = ['개', '고양이', '새', '고양이'];

console.log(animals.indexOf('고양이')); // 출력: 1 (첫 번째 '고양이'의 인덱스)
console.log(animals.indexOf('토끼'));   // 출력: -1 (배열에 '토끼'가 없음)

console.log(animals.includes('새'));    // 출력: true
console.log(animals.includes('토끼'));   // 출력: false
```

### 3-6) `concat()` 메서드

`concat()` 메서드는 두 개 이상의 배열을 합쳐서 **새로운 배열**을 만듭니다. 원본 배열은 변경되지 않습니다. 스프레드 연산자(`...`)와 유사한 기능을 합니다.

```javascript
const arrA = [1, 2];
const arrB = [3, 4];
const arrC = [5, 6];

const combined = arrA.concat(arrB, arrC);
console.log(combined); // 출력: [1, 2, 3, 4, 5, 6]
console.log(arrA);     // 출력: [1, 2] (원본 배열은 그대로)
```

### 3-7) `join()` 메서드

`join()` 메서드는 배열의 모든 요소를 연결하여 하나의 **문자열**로 만듭니다. 이때 각 요소 사이에 지정된 구분자(separator)를 삽입할 수 있습니다.

```javascript
const words = ['안녕', '하세요', '!'];
const sentence = words.join(''); // 구분자 없이 연결
console.log(sentence); // 출력: 안녕하세요!

const csvData = ['사과', '바나나', '오렌지'];
const csvString = csvData.join(','); // 쉼표로 연결
console.log(csvString); // 출력: 사과,바나나,오렌지
```

### 3-8) `slice()`와 `splice()` 메서드

이 두 메서드는 이름이 비슷하지만 기능이 매우 다릅니다.

*   **`slice()`**: 배열의 특정 부분을 잘라내어 **새로운 배열**을 반환합니다. 원본 배열은 변경되지 않습니다.
    *   `array.slice(시작인덱스, 끝인덱스)`: `시작인덱스`부터 `끝인덱스` 전까지의 요소를 복사합니다.
    *   `끝인덱스`를 생략하면 `시작인덱스`부터 배열의 끝까지 복사합니다.

    ```javascript
    const originalArray = ['a', 'b', 'c', 'd', 'e'];

    const sliced1 = originalArray.slice(1, 4); // 인덱스 1부터 4 전까지
    console.log(sliced1);      // 출력: ['b', 'c', 'd']
    console.log(originalArray); // 출력: ['a', 'b', 'c', 'd', 'e'] (원본 변경 없음)

    const sliced2 = originalArray.slice(2); // 인덱스 2부터 끝까지
    console.log(sliced2);      // 출력: ['c', 'd', 'e']
    ```

*   **`splice()`**: 배열의 기존 요소를 **제거하거나 교체**하고, 새로운 요소를 **추가**하여 배열의 내용을 변경합니다. 원본 배열을 직접 변경합니다.
    *   `array.splice(시작인덱스, 제거할개수, 추가할요소1, 추가할요소2, ...)`

    ```javascript
    const fruits = ['사과', '바나나', '오렌지', '딸기'];

    // 요소 제거: 인덱스 1부터 2개 제거
    const removed = fruits.splice(1, 2); // '바나나', '오렌지' 제거
    console.log(fruits);  // 출력: ['사과', '딸기'] (원본 배열 변경됨)
    console.log(removed); // 출력: ['바나나', '오렌지'] (제거된 요소들)

    // 요소 추가: 인덱스 1에 '포도' 추가 (제거할 개수 0)
    fruits.splice(1, 0, '포도');
    console.log(fruits); // 출력: ['사과', '포도', '딸기']

    // 요소 교체: 인덱스 0부터 1개 제거하고 '수박' 추가
    fruits.splice(0, 1, '수박');
    console.log(fruits); // 출력: ['수박', '포도', '딸기']
    ```

> [!NOTE]
> `slice()`는 원본 배열을 건드리지 않고 새로운 배열을 반환하는 반면, `splice()`는 원본 배열을 직접 변경한다는 점을 명심해야 합니다.

## 4) 객체(Object)이란 무엇인가?

객체는 '속성(key)과 값(value)의 쌍'으로 이루어진 데이터 집합입니다.

```javascript
// 객체 만들기: 중괄호({}) 안에 key: value 형식으로 작성합니다.
const person = {
  name: '철수',         // name이라는 속성(key)에 '철수'라는 값(value)이 있습니다.
  age: 13,            // age라는 속성(key)에 13이라는 값(value)이 있습니다.
  hobbies: ['축구', '독서'] // hobbies라는 속성에는 배열이 값으로 들어있습니다.
};

// 속성 꺼내기 (두 가지 방법이 있습니다.)
console.log(person.name);   // 점(.)을 사용하여 속성 이름으로 접근: 출력: '철수'
console.log(person['age']); // 대괄호([]) 안에 문자열로 속성 이름을 넣어 접근: 출력: 13
console.log(person.hobbies[0]); // 객체 안의 배열 요소에 접근: 출력: '축구'
```

> [!NOTE]
> 객체의 속성 이름(key)은 보통 문자열로 작성하지만, 따옴표를 생략할 수도 있습니다. 하지만 속성 이름에 공백이나 특수 문자가 포함될 경우 반드시 따옴표로 감싸야 합니다. 객체는 순서가 중요하지 않습니다. 각 속성은 고유한 이름으로 구분됩니다.

---

## 5) 객체 수정과 추가/삭제

객체도 배열처럼 한 번 만들면 끝이 아니라, 필요에 따라 속성을 추가하거나, 기존 속성의 값을 수정하거나, 속성을 삭제할 수 있습니다.

*   **추가 (add)**: 새로운 속성 이름에 값을 할당하면 객체에 해당 속성이 추가됩니다.
*   **수정 (update)**: 이미 존재하는 속성 이름에 새로운 값을 할당하면 기존 값이 새 값으로 덮어씌워집니다.
*   **삭제 (delete)**: `delete` 키워드를 사용하면 객체에서 특정 속성을 완전히 제거할 수 있습니다.

```javascript
const person = {
  name: '철수',
  age: 13
};
console.log("초기 객체:", person); // 출력: 초기 객체: { name: '철수', age: 13 }

// 속성 추가
person.school = '중학교'; // school이라는 새로운 속성을 추가하고 '중학교' 값을 할당합니다.
console.log("school 추가 후:", person); // 출력: school 추가 후: { name: '철수', age: 13, school: '중학교' }

// 속성 수정
person.age = 14; // age 속성의 값을 13에서 14로 변경합니다.
console.log("age 수정 후:", person); // 출력: age 수정 후: { name: '철수', age: 14, school: '중학교' }

// 속성 삭제
delete person.school; // school 속성을 객체에서 제거합니다.
console.log("school 삭제 후:", person); // 출력: school 삭제 후: { name: '철수', age: 14 }

// 존재하지 않는 속성에 접근하면 undefined가 반환됩니다.
console.log(person.hobbies); // 출력: undefined
```

> [!TIP]
> 객체는 데이터를 구조화하고 관리하는 데 매우 강력한 도구입니다. 실제 웹 개발에서는 사용자 정보, 상품 정보, 설정 값 등 다양한 종류의 데이터를 객체 형태로 다루게 됩니다. 객체의 속성이 너무 많아지면 관리하기 어려워질 수 있으므로, 필요한 속성만 유지하고 관련성이 높은 속성끼리 묶는 연습을 하는 것이 좋습니다.

## 6) 객체 관련 유용한 메서드

JavaScript의 `Object` 객체는 객체를 다루는 데 유용한 여러 정적 메서드를 제공합니다. 이 메서드들은 객체 자체를 변경하지 않고, 객체의 속성(key)이나 값(value)을 배열 형태로 반환하거나, 특정 속성의 존재 여부를 확인하는 등의 작업을 수행합니다.

### 6-1) `Object.keys()`

`Object.keys()` 메서드는 객체의 모든 속성 이름(key)을 배열로 반환합니다.

```javascript
const car = {
  brand: 'Hyundai',
  model: 'Sonata',
  year: 2023
};

const keys = Object.keys(car);
console.log(keys); // 출력: ['brand', 'model', 'year']
```

### 6-2) `Object.values()`

`Object.values()` 메서드는 객체의 모든 속성 값(value)을 배열로 반환합니다.

```javascript
const car = {
  brand: 'Hyundai',
  model: 'Sonata',
  year: 2023
};

const values = Object.values(car);
console.log(values); // 출력: ['Hyundai', 'Sonata', 2023]
```

### 6-3) `Object.entries()`

`Object.entries()` 메서드는 객체의 모든 속성(key)과 값(value)의 쌍을 `[key, value]` 형태의 배열로 묶어, 다시 이 배열들을 요소로 하는 새로운 배열을 반환합니다.

```javascript
const car = {
  brand: 'Hyundai',
  model: 'Sonata',
  year: 2023
};

const entries = Object.entries(car);
console.log(entries);
// 출력:
// [
//   ['brand', 'Hyundai'],
//   ['model', 'Sonata'],
//   ['year', 2023]
// ]

// for...of 반복문과 함께 사용하면 객체를 쉽게 순회할 수 있습니다.
for (const [key, value] of Object.entries(car)) {
  console.log(`${key}: ${value}`);
}
// 출력:
// brand: Hyundai
// model: Sonata
// year: 2023
```

### 6-4) `hasOwnProperty()`

`hasOwnProperty()` 메서드는 객체가 특정 속성(key)을 직접 가지고 있는지 여부를 `true` 또는 `false`로 반환합니다.

```javascript
const person = {
  name: '철수',
  age: 17
};

console.log(person.hasOwnProperty('name'));    // 출력: true
console.log(person.hasOwnProperty('age'));     // 출력: true
console.log(person.hasOwnProperty('gender'));  // 출력: false

// 객체에 직접 정의되지 않은 속성 (예: toString은 모든 객체가 상속받는 메서드)
console.log(person.hasOwnProperty('toString')); // 출력: false
```

> [!TIP]
> `Object.keys()`, `Object.values()`, `Object.entries()`는 객체의 내용을 배열로 변환하여 `forEach`, `map`, `filter` 등 배열 메서드를 사용하여 객체 데이터를 더 유연하게 다룰 수 있게 해줍니다.

## 7) 구조 분해 할당 (Destructuring Assignment)

구조 분해 할당은 배열이나 객체에서 필요한 값만 뽑아서 새로운 변수에 할당하는 편리한 문법입니다. 코드를 더 간결하고 읽기 쉽게 만들어줍니다.

### 7-1) 배열 구조 분해

배열 구조 분해는 배열의 요소를 순서대로 변수에 할당합니다.

```javascript
const colors = ["빨강", "파랑", "초록"];

// 배열의 첫 번째와 두 번째 요소를 각각 firstColor, secondColor 변수에 할당합니다.
const [firstColor, secondColor] = colors;
console.log(firstColor);  // 출력: 빨강
console.log(secondColor); // 출력: 파랑

// 필요한 요소만 건너뛸 수도 있습니다.
const [,, thirdColor] = colors; // 첫 번째와 두 번째 요소를 건너뛰고 세 번째 요소만 할당
console.log(thirdColor); // 출력: 초록

// 기본값 설정
const [name, age, city = "미정"] = ["김철수", 17];
console.log(name); // 출력: 김철수
console.log(age);  // 출력: 17
console.log(city); // 출력: 미정 (city는 배열에 없으므로 기본값 사용)
```

### 7-2) 객체 구조 분해

객체 구조 분해는 객체의 속성 이름(key)을 사용하여 해당 속성의 값을 변수에 할당합니다. 순서는 중요하지 않습니다.

```javascript
const person = {
  name: "영희",
  age: 18,
  city: "서울"
};

// 객체의 name과 age 속성을 각각 name, age 변수에 할당합니다.
const { name, age } = person;
console.log(name); // 출력: 영희
console.log(age);  // 출력: 18

// 다른 이름으로 변수를 선언할 수도 있습니다.
const { city: currentCity } = person;
console.log(currentCity); // 출력: 서울

// 기본값 설정
const { job = "학생", name: personName } = person;
console.log(job);        // 출력: 학생 (job은 객체에 없으므로 기본값 사용)
console.log(personName); // 출력: 영희

// 함수 매개변수에서도 활용
function displayPersonInfo({ name, city }) {
  console.log(`${name}은 ${city}에 삽니다.`);
}
displayPersonInfo(person); // 출력: 영희은 서울에 삽니다.
```

> [!TIP]
> 구조 분해 할당은 배열이나 객체에서 필요한 데이터만 깔끔하게 추출할 때 매우 유용합니다. 특히 함수의 매개변수로 객체를 받을 때 특정 속성만 사용하고 싶을 때 자주 사용됩니다.

## 8) 스프레드 연산자 (`...`) (Spread Operator)

스프레드 연산자 (`...`)는 배열이나 객체의 내용을 '펼쳐서' 새로운 배열이나 객체를 만들거나, 함수의 인수로 전달할 때 사용합니다.

### 8-1) 배열에서 스프레드 연산자 사용

배열의 모든 요소를 개별적인 요소로 펼쳐줍니다.

```javascript
// 배열 합치기
const arr1 = [1, 2];
const arr2 = [3, 4];
const combinedArr = [...arr1, ...arr2]; // [1, 2, 3, 4]
console.log(combinedArr);

// 배열 복사 (얕은 복사)
const originalArr = [10, 20];
const copiedArr = [...originalArr]; // [10, 20]
copiedArr.push(30); // 복사된 배열에만 30을 추가합니다.
console.log(originalArr); // 출력: [10, 20] (원본 배열은 변하지 않습니다.)
console.log(copiedArr);   // 출력: [10, 20, 30]

// 배열에 요소 추가
const numbers = [1, 2, 3];
const newNumbers = [0, ...numbers, 4, 5]; // [0, 1, 2, 3, 4, 5]
console.log(newNumbers);
```

### 8-2) 객체에서 스프레드 연산자 사용

객체의 모든 속성을 개별적인 속성으로 펼쳐줍니다.

```javascript
// 객체 합치기
const obj1 = { a: 1, b: 2 };
const obj2 = { c: 3, d: 4 };
const combinedObj = { ...obj1, ...obj2 }; // { a: 1, b: 2, c: 3, d: 4 }
console.log(combinedObj);

// 객체 복사 (얕은 복사)
const originalObj = { name: "철수", age: 17 };
const copiedObj = { ...originalObj };
copiedObj.age = 18; // 복사된 객체의 age만 변경합니다.
console.log(originalObj); // 출력: { name: "철수", age: 17 } (원본 객체는 변하지 않습니다.)
console.log(copiedObj);   // 출력: { name: "철수", age: 18 }

// 객체에 속성 추가 또는 덮어쓰기
const user = { id: 1, name: "영희" };
const updatedUser = { ...user, name: "김영희", email: "kim@example.com" };
console.log(updatedUser); // 출력: { id: 1, name: "김영희", email: "kim@example.com" }
```

### 8-3) 함수에서 스프레드 연산자 사용

배열의 요소들을 함수의 개별적인 인수로 전달할 때 사용합니다.

```javascript
function sum(a, b, c) {
  return a + b + c;
}

const numbersToSum = [1, 2, 3];
// sum(1, 2, 3)과 동일하게 동작합니다.
console.log(sum(...numbersToSum)); // 출력: 6
```

> [!NOTE]
> 스프레드 연산자는 배열이나 객체를 복사할 때 '얕은 복사'를 수행합니다. 즉, 중첩된 배열이나 객체가 있을 경우, 가장 바깥쪽만 복사되고 안쪽의 객체들은 여전히 원본을 참조합니다.

---

- [목차로 돌아가기](../README.md)
- [이전 강의로 이동](06-ES6-Functions.md)
- [다음 강의로 이동](08-ES6-Classes.md)
```

> [!TIP]
> 객체는 데이터를 구조화하고 관리하는 데 매우 강력한 도구입니다. 실제 웹 개발에서는 사용자 정보, 상품 정보, 설정 값 등 다양한 종류의 데이터를 객체 형태로 다루게 됩니다. 객체의 속성이 너무 많아지면 관리하기 어려워질 수 있으므로, 필요한 속성만 유지하고 관련성이 높은 속성끼리 묶는 연습을 하는 것이 좋습니다.

---

## 6) 구조 분해 할당 (Destructuring Assignment)

구조 분해 할당은 배열이나 객체에서 필요한 값만 뽑아서 새로운 변수에 할당하는 편리한 문법입니다. 코드를 더 간결하고 읽기 쉽게 만들어줍니다.

### 6-1) 배열 구조 분해

배열 구조 분해는 배열의 요소를 순서대로 변수에 할당합니다.

```javascript
const colors = ["빨강", "파랑", "초록"];

// 배열의 첫 번째와 두 번째 요소를 각각 firstColor, secondColor 변수에 할당합니다.
const [firstColor, secondColor] = colors;
console.log(firstColor);  // 출력: 빨강
console.log(secondColor); // 출력: 파랑

// 필요한 요소만 건너뛸 수도 있습니다.
const [,, thirdColor] = colors; // 첫 번째와 두 번째 요소를 건너뛰고 세 번째 요소만 할당
console.log(thirdColor); // 출력: 초록

// 기본값 설정
const [name, age, city = "미정"] = ["김철수", 17];
console.log(name); // 출력: 김철수
console.log(age);  // 출력: 17
console.log(city); // 출력: 미정 (city는 배열에 없으므로 기본값 사용)
```

### 6-2) 객체 구조 분해

객체 구조 분해는 객체의 속성 이름(key)을 사용하여 해당 속성의 값을 변수에 할당합니다. 순서는 중요하지 않습니다.

```javascript
const person = {
  name: "영희",
  age: 18,
  city: "서울"
};

// 객체의 name과 age 속성을 각각 name, age 변수에 할당합니다.
const { name, age } = person;
console.log(name); // 출력: 영희
console.log(age);  // 출력: 18

// 다른 이름으로 변수를 선언할 수도 있습니다.
const { city: currentCity } = person;
console.log(currentCity); // 출력: 서울

// 기본값 설정
const { job = "학생", name: personName } = person;
console.log(job);        // 출력: 학생 (job은 객체에 없으므로 기본값 사용)
console.log(personName); // 출력: 영희

// 함수 매개변수에서도 활용
function displayPersonInfo({ name, city }) {
  console.log(`${name}은 ${city}에 삽니다.`);
}
displayPersonInfo(person); // 출력: 영희은 서울에 삽니다.
```

> [!TIP]
> 구조 분해 할당은 배열이나 객체에서 필요한 데이터만 깔끔하게 추출할 때 매우 유용합니다. 특히 함수의 매개변수로 객체를 받을 때 특정 속성만 사용하고 싶을 때 자주 사용됩니다.

## 7) 스프레드 연산자 (`...`) (Spread Operator)

스프레드 연산자 (`...`)는 배열이나 객체의 내용을 '펼쳐서' 새로운 배열이나 객체를 만들거나, 함수의 인수로 전달할 때 사용합니다.

### 7-1) 배열에서 스프레드 연산자 사용

배열의 모든 요소를 개별적인 요소로 펼쳐줍니다.

```javascript
// 배열 합치기
const arr1 = [1, 2];
const arr2 = [3, 4];
const combinedArr = [...arr1, ...arr2]; // [1, 2, 3, 4]
console.log(combinedArr);

// 배열 복사 (얕은 복사)
const originalArr = [10, 20];
const copiedArr = [...originalArr]; // [10, 20]
copiedArr.push(30); // 복사된 배열에만 30을 추가합니다.
console.log(originalArr); // 출력: [10, 20] (원본 배열은 변하지 않습니다.)
console.log(copiedArr);   // 출력: [10, 20, 30]

// 배열에 요소 추가
const numbers = [1, 2, 3];
const newNumbers = [0, ...numbers, 4, 5]; // [0, 1, 2, 3, 4, 5]
console.log(newNumbers);
```

### 7-2) 객체에서 스프레드 연산자 사용

객체의 모든 속성을 개별적인 속성으로 펼쳐줍니다.

```javascript
// 객체 합치기
const obj1 = { a: 1, b: 2 };
const obj2 = { c: 3, d: 4 };
const combinedObj = { ...obj1, ...obj2 }; // { a: 1, b: 2, c: 3, d: 4 }
console.log(combinedObj);

// 객체 복사 (얕은 복사)
const originalObj = { name: "철수", age: 17 };
const copiedObj = { ...originalObj };
copiedObj.age = 18; // 복사된 객체의 age만 변경합니다.
console.log(originalObj); // 출력: { name: "철수", age: 17 } (원본 객체는 변하지 않습니다.)
console.log(copiedObj);   // 출력: { name: "철수", age: 18 }

// 객체에 속성 추가 또는 덮어쓰기
const user = { id: 1, name: "영희" };
const updatedUser = { ...user, name: "김영희", email: "kim@example.com" };
console.log(updatedUser); // 출력: { id: 1, name: "김영희", email: "kim@example.com" }
```

### 7-3) 함수에서 스프레드 연산자 사용

배열의 요소들을 함수의 개별적인 인수로 전달할 때 사용합니다.

```javascript
function sum(a, b, c) {
  return a + b + c;
}

const numbersToSum = [1, 2, 3];
// sum(1, 2, 3)과 동일하게 동작합니다.
console.log(sum(...numbersToSum)); // 출력: 6
```

> [!NOTE]
> 스프레드 연산자는 배열이나 객체를 복사할 때 '얕은 복사'를 수행합니다. 즉, 중첩된 배열이나 객체가 있을 경우, 가장 바깥쪽만 복사되고 안쪽의 객체들은 여전히 원본을 참조합니다.

---

- [목차로 돌아가기](../README.md)
- [이전 강의로 이동](06-ES6-Functions.md)
- [다음 강의로 이동](08-ES6-Classes.md)
