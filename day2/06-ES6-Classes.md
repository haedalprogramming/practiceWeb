# 06. ES6+ 클래스

## 1) 클래스
- 클래스는 객체를 만들기 위한 설계도라고 생각할 수 있습니다.

```javascript
// Person이라는 클래스를 만들기
class Person {
  constructor(name, age) {      // 생성자
    this.name = name;            // 객체 속성 설정
    this.age = age;
  }
}

// 클래스로부터 객체(instance)를 만들어 사용
const p1 = new Person('철수', 14);
console.log(p1.name); // '철수'
console.log(p1.age);  // 14
```

> [!TIP]
> 클래스 이름은 첫 글자를 대문자로 쓰는 것이 관습입니다 (PascalCase).

## 2) 생성자(constructor)
- `constructor` 함수는 객체를 만들 때 자동으로 실행되어 초기값을 설정해 줍니다.
- `this`는 방금 만든 객체 자신을 가리킵니다.

```javascript
class Book {
  constructor(title) {
    this.title = title;        // this.title: 객체에 title 속성 추가
  }
}
const book = new Book('자바스크립트 교과서');
console.log(book.title); // '자바스크립트 교과서'
```

## 3) 메서드(method)
- 클래스 안에 정의된 함수는 '메서드'라고 부릅니다.

```javascript
class Person {
  constructor(name) {
    this.name = name;
  }
  sayHello() {                  // 메서드 정의
    console.log('안녕하세요, 저는 ' + this.name + '입니다.');
  }
}
const p2 = new Person('영희');
p2.sayHello();                  // '안녕하세요, 저는 영희입니다.'
```

## 4) 상속(Inheritance)
- 이미 만든 클래스를 **확장**하여 새로운 클래스를 만들 수 있어요.
- `extends` 키워드를 사용합니다.

```javascript
class Animal {
  constructor(type) {
    this.type = type;
  }
  speak() {
    console.log(this.type + '가 소리를 냅니다.');
  }
}

class Dog extends Animal {
  constructor(name) {
    super('개');               // 부모 클래스 생성자 호출
    this.name = name;
  }
  speak() {                    // 메서드 재정의
    console.log(this.name + '가 멍멍!');
  }
}

const dog1 = new Dog('바둑이');
dog1.speak();                  // '바둑이가 멍멍!'
```

> [!TIP]
> `super(…)`를 사용해 부모 클래스의 생성자를 먼저 실행해야 `this`를 사용할 수 있습니다.

## 5) 정적 메서드(static)
- 객체를 만들 필요 없이 클래스에서 바로 호출할 수 있는 메서드입니다.

```javascript
class MathUtil {
  static multiply(a, b) {
    return a * b;
  }
}
console.log(MathUtil.multiply(3, 4)); // 12
```

## 6) 게터(getter)와 세터(setter)
- 객체 속성을 읽거나 설정할 때 **함수처럼 동작**하도록 만들 수 있습니다.

```javascript
class Rectangle {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }
  get area() {                  // 게터
    return this.width * this.height;
  }
  set area(value) {            // 세터 (실제로는 값 변경을 막는 예)
    console.log('area는 읽기 전용입니다.');
  }
}
const rect = new Rectangle(3, 4);
console.log(rect.area);        // 12 (area()가 아닌 속성처럼 사용)
rect.area = 100;               // 'area는 읽기 전용입니다.'
```

---