# 05. 배열과 객체 이해하기

## 1) 배열(Array)
- 배열은 여러 개의 값을 순서대로 보관하는 자료 구조입니다.

```javascript
// 배열 만들기: 대괄호([]) 안에 값을 쉼표로 구분하여 작성
const fruits = ['사과', '바나나', '오렌지'];

// 첫 번째 요소 꺼내기 (인덱스 0)
console.log(fruits[0]); // '사과'
// 배열 길이 확인하기
console.log(fruits.length); // 3
```

> [!TIP]
> 배열의 인덱스는 0부터 시작합니다. 마지막 요소는 `length - 1` 인덱스로 꺼낼 수 있습니다.

## 2) 배열 조작하기 (추가, 제거, 변경)
- **추가(add)**: `push()` 메서드를 사용해 끝에 값을 추가합니다.
- **제거(remove)**: `pop()` 메서드를 사용해 끝의 값을 꺼냅니다.
- **변경(update)**: 특정 인덱스에 새 값을 덮어씁니다.

```javascript
const nums = [1, 2, 3];

// 값 추가
nums.push(4);
console.log(nums); // [1, 2, 3, 4]

// 값 제거
const last = nums.pop();
console.log(last); // 4
console.log(nums); // [1, 2, 3]

// 값 변경
nums[1] = 20;
console.log(nums); // [1, 20, 3]
```

## 3) 배열 반복과 주요 메서드
- **for** 반복문: 배열을 하나씩 꺼내서 처리
- **map()**: 배열의 각 값을 변형해 새 배열을 만듦
- **filter()**: 조건에 맞는 값만 골라 새 배열을 만듦

```javascript
const numbers = [1, 2, 3, 4, 5];

// for 반복문
for (let i = 0; i < numbers.length; i++) {
  console.log(numbers[i]);
}

// map(): 값에 *2 연산 적용
const doubled = numbers.map(n => n * 2);
console.log(doubled); // [2, 4, 6, 8, 10]

// filter(): 짝수만 걸러내기
const evens = numbers.filter(n => n % 2 === 0);
console.log(evens); // [2, 4]
```

> [!TIP]
> `map`과 `filter` 뒤에는 화살표 함수(`=>`)를 사용해 간결한 코드를 작성할 수 있습니다.

## 4) 객체(Object)이란 무엇인가?
- 객체는 '속성(key)과 값(value)의 쌍'으로 이루어진 상자입니다.
- 마치 이름표가 붙은 사물함처럼 생각하면 쉽습니다.

```javascript
// 객체 만들기: 중괄호({}) 안에 key: value 형식으로 작성
const person = {
  name: '철수',
  age: 13,
  hobbies: ['축구', '독서']
};

// 속성 꺼내기
console.log(person.name);   // '철수'
console.log(person['age']); // 13
```

## 5) 객체 수정과 추가/삭제
- **추가(add)**: 새로운 key에 값을 할당
- **수정(update)**: 기존 key에 새 값을 덮어쓰기
- **삭제(delete)**: `delete` 키워드를 사용

```javascript
// 속성 추가
person.school = '중학교';
console.log(person.school); // '중학교'

// 속성 수정
person.age = 14;
console.log(person.age);    // 14

// 속성 삭제
delete person.hobbies;
console.log(person.hobbies); // undefined
```

> [!TIP]
> 객체의 속성이 너무 많아지면 관리하기 어려워질 수 있습니다. 필요한 속성만 유지하세요.

---