# 08. ES6+ 클래스

## 목차
1. [클래스](#1-클래스)
2. [생성자(constructor)](#2-생성자constructor)
3. [클래스 속성 (Class Properties)](#3-클래스-속성-class-properties)
4. [메서드(method)](#4-메서드method)
5. [상속(Inheritance)](#5-상속inheritance)
6. [정적 메서드(static)](#6-정적-메서드static)
7. [게터(getter)와 세터(setter)](#7-게터getter와-세터setter)

## 1) 클래스

클래스는 객체를 만들기 위한 '설계도' 또는 '틀'이라고 생각할 수 있습니다. 우리가 붕어빵을 만들 때 붕어빵 틀이 필요한 것처럼, JavaScript에서 비슷한 형태의 객체들을 여러 개 만들고 싶을 때 클래스를 사용합니다. 클래스는 객체가 가져야 할 속성(데이터)과 메서드(행동)를 정의합니다.

```javascript
// Person이라는 클래스를 만들기: 사람 객체를 만들기 위한 설계도입니다.
class Person {
  // constructor는 이 설계도를 바탕으로 실제 사람 객체를 만들 때 실행되는 부분입니다.
  // name과 age는 이 사람 객체가 가질 속성입니다.
  constructor(name, age) {      // 생성자: 객체가 만들어질 때 초기 설정을 해줍니다.
    this.name = name;            // this.name은 만들어질 객체의 name 속성을 의미합니다.
    this.age = age;              // this.age는 만들어질 객체의 age 속성을 의미합니다.
  }

  // sayHello는 이 사람 객체가 할 수 있는 행동(메서드)입니다.
  sayHello() {
    console.log(`안녕하세요, 저는 ${this.name}입니다. ${this.age}살이에요.`);
  }
}

// 클래스로부터 객체(instance)를 만들어 사용: 설계도를 바탕으로 실제 붕어빵을 만드는 과정입니다.
const p1 = new Person('철수', 14); // new Person()을 통해 Person 설계도로 철수라는 객체를 만듭니다.
console.log(p1.name); // 출력: '철수' (p1 객체의 name 속성)
console.log(p1.age);  // 출력: 14 (p1 객체의 age 속성)
p1.sayHello();        // 출력: 안녕하세요, 저는 철수입니다. 14살이에요.

const p2 = new Person('영희', 15); // 같은 설계도로 영희라는 또 다른 객체를 만듭니다.
console.log(p2.name); // 출력: '영희'
p2.sayHello();        // 출력: 안녕하세요, 저는 영희입니다. 15살이에요.
```

> [!TIP]
> 클래스 이름은 첫 글자를 대문자로 쓰는 것이 관습입니다 (PascalCase). 예를 들어, `Person`, `Car`, `Product`와 같이 작성합니다. 이는 일반 변수나 함수 이름과 구분하여 클래스임을 쉽게 알 수 있도록 돕는 규칙입니다.

## 2) 생성자(constructor)

`constructor`는 클래스를 통해 새로운 객체(인스턴스)를 만들 때 **가장 먼저 자동으로 실행되는 특별한 함수**입니다. 이 함수의 주된 역할은 새로 만들어지는 객체의 초기 상태를 설정하는 것입니다. 예를 들어, `Person` 클래스로 사람 객체를 만들 때, 그 사람의 이름과 나이를 처음부터 정해주는 역할을 합니다.

`constructor` 안에서 자주 보게 될 키워드가 바로 `this`입니다. `this`는 현재 만들어지고 있는 **객체 자신**을 가리킵니다. `this.name = name;`이라는 코드는 '지금 만들고 있는 이 객체의 `name`이라는 속성에, `constructor`로 전달받은 `name` 값을 넣어라'는 의미입니다.

```javascript
class Book {
  // Book 클래스로 새로운 책 객체를 만들 때, title이라는 정보를 받아서 초기화합니다.
  constructor(title) {
    this.title = title;        // this.title: 새로 만들어질 Book 객체에 title이라는 속성을 추가하고, 전달받은 title 값으로 설정합니다.
  }

  // 책의 정보를 출력하는 메서드
  getInfo() {
    console.log(`책 제목: ${this.title}`);
  }
}

const book1 = new Book('자바스크립트 교과서'); // '자바스크립트 교과서'라는 제목으로 book1 객체를 만듭니다.
console.log(book1.title); // 출력: '자바스크립트 교과서'
book1.getInfo();          // 출력: 책 제목: 자바스크립트 교과서

const book2 = new Book('HTML/CSS 입문'); // 또 다른 책 객체를 만듭니다.
console.log(book2.title); // 출력: 'HTML/CSS 입문'
```

> [!NOTE]
> `constructor`는 클래스당 하나만 존재할 수 있습니다. 만약 `constructor`를 명시적으로 작성하지 않으면, JavaScript는 비어있는 기본 `constructor`를 자동으로 추가해줍니다.

---

## 3) 클래스 속성 (Class Properties)

ES6 이후에 도입된 **클래스 속성(Class Properties)** 문법을 사용하면 `constructor` 내부에서 `this`를 사용하여 속성을 정의하는 것 외에, 클래스 본문에서 직접 속성을 선언할 수 있습니다. 이는 코드를 더 간결하게 만들고, 특히 초기값이 고정된 속성을 정의할 때 유용합니다.

클래스 속성은 두 가지 방식으로 선언할 수 있습니다:

1.  **인스턴스 속성 (Instance Properties)**: `constructor`에서 `this.속성명 = 값`으로 정의하는 것과 동일하게, 클래스로부터 생성된 **각각의 객체(인스턴스)**가 자신만의 속성 값을 가지게 됩니다.
2.  **정적 속성 (Static Properties)**: `static` 키워드를 사용하여 선언하며, 클래스 자체에 속하는 속성입니다. 객체를 생성하지 않고 **클래스 이름으로 직접 접근**할 수 있습니다.

```javascript
class Product {
  // 1. 인스턴스 속성 (클래스 속성 문법 사용)
  // 이 속성들은 Product 클래스로 만들어지는 모든 객체가 자신만의 값을 가집니다.
  // constructor에서 this.name = name; this.price = price; 와 동일한 효과를 냅니다.
  name = '기본 상품';
  price = 0;
  category = '기타';

  constructor(name, price) {
    // constructor가 있다면, 여기서 초기화된 값이 클래스 속성보다 우선합니다.
    this.name = name;
    this.price = price;
    // category는 constructor에서 초기화하지 않았으므로, 위에서 선언된 '기타'가 기본값으로 사용됩니다.
  }

  // 2. 정적 속성 (static class property)
  // 이 속성은 Product 클래스 자체에 속하며, 모든 Product 객체가 공유하는 값이 아닙니다.
  // Product.MAX_PRICE 처럼 클래스 이름으로 직접 접근합니다.
  static MAX_PRICE = 100000;
  static DESCRIPTION = '모든 상품의 기본 설명입니다.';

  displayInfo() {
    console.log(`상품명: ${this.name}, 가격: ${this.price}원, 카테고리: ${this.category}`);
  }

  static getProductCount() {
    // 정적 메서드 안에서는 정적 속성에 접근할 수 있습니다.
    // 하지만 this.name과 같은 인스턴스 속성에는 직접 접근할 수 없습니다.
    console.log(`최대 가격: ${Product.MAX_PRICE}원`);
    return '상품 개수를 계산하는 로직 (예시)';
  }
}

const item1 = new Product('노트북', 1200000);
item1.displayInfo(); // 출력: 상품명: 노트북, 가격: 1200000원, 카테고리: 기타

const item2 = new Product('키보드', 50000);
item2.displayInfo(); // 출력: 상품명: 키보드, 가격: 50000원, 카테고리: 기타

console.log(item1.name); // 출력: 노트북
console.log(item2.name); // 출력: 키보드

// 정적 속성 접근
console.log(Product.MAX_PRICE);     // 출력: 100000
console.log(Product.DESCRIPTION);   // 출력: 모든 상품의 기본 설명입니다.
console.log(Product.getProductCount()); // 출력: 최대 가격: 100000원, 상품 개수를 계산하는 로직 (예시)

// 인스턴스에서 정적 속성에 직접 접근할 수 없습니다.
// console.log(item1.MAX_PRICE); // undefined
```

> [!NOTE]
> 클래스 속성 문법은 `constructor`를 사용하지 않고도 인스턴스 속성을 선언할 수 있게 해주어 코드를 더 간결하게 만듭니다. 특히 초기값이 고정된 속성이나, `constructor`에서 복잡한 로직 없이 단순히 속성을 초기화할 때 유용합니다.
>
> 정적 속성은 클래스 자체에 속하는 데이터로, 모든 인스턴스가 공유하는 값이 아닙니다. 클래스 레벨의 상수나 유틸리티 정보 등을 저장할 때 사용됩니다.

---

## 4) 메서드(method)
```

> [!NOTE]
> `constructor`는 클래스당 하나만 존재할 수 있습니다. 만약 `constructor`를 명시적으로 작성하지 않으면, JavaScript는 비어있는 기본 `constructor`를 자동으로 추가해줍니다.

---

## 3) 클래스 속성 (Class Properties)

ES6 이후에 도입된 **클래스 속성(Class Properties)** 문법을 사용하면 `constructor` 내부에서 `this`를 사용하여 속성을 정의하는 것 외에, 클래스 본문에서 직접 속성을 선언할 수 있습니다. 이는 코드를 더 간결하게 만들고, 특히 초기값이 고정된 속성을 정의할 때 유용합니다.

클래스 속성은 두 가지 방식으로 선언할 수 있습니다:

1.  **인스턴스 속성 (Instance Properties)**: `constructor`에서 `this.속성명 = 값`으로 정의하는 것과 동일하게, 클래스로부터 생성된 **각각의 객체(인스턴스)**가 자신만의 속성 값을 가지게 됩니다.
2.  **정적 속성 (Static Properties)**: `static` 키워드를 사용하여 선언하며, 클래스 자체에 속하는 속성입니다. 객체를 생성하지 않고 **클래스 이름으로 직접 접근**할 수 있습니다.

```javascript
class Product {
  // 1. 인스턴스 속성 (클래스 속성 문법 사용)
  // 이 속성들은 Product 클래스로 만들어지는 모든 객체가 자신만의 값을 가집니다.
  // constructor에서 this.name = name; this.price = price; 와 동일한 효과를 냅니다.
  name = '기본 상품';
  price = 0;
  category = '기타';

  constructor(name, price) {
    // constructor가 있다면, 여기서 초기화된 값이 클래스 속성보다 우선합니다.
    this.name = name;
    this.price = price;
    // category는 constructor에서 초기화하지 않았으므로, 위에서 선언된 '기타'가 기본값으로 사용됩니다.
  }

  // 2. 정적 속성 (static class property)
  // 이 속성은 Product 클래스 자체에 속하며, 모든 Product 객체가 공유하는 값이 아닙니다.
  // Product.MAX_PRICE 처럼 클래스 이름으로 직접 접근합니다.
  static MAX_PRICE = 100000;
  static DESCRIPTION = '모든 상품의 기본 설명입니다.';

  displayInfo() {
    console.log(`상품명: ${this.name}, 가격: ${this.price}원, 카테고리: ${this.category}`);
  }

  static getProductCount() {
    // 정적 메서드 안에서는 정적 속성에 접근할 수 있습니다.
    // 하지만 this.name과 같은 인스턴스 속성에는 직접 접근할 수 없습니다.
    console.log(`최대 가격: ${Product.MAX_PRICE}원`);
    return '상품 개수를 계산하는 로직 (예시)';
  }
}

const item1 = new Product('노트북', 1200000);
item1.displayInfo(); // 출력: 상품명: 노트북, 가격: 1200000원, 카테고리: 기타

const item2 = new Product('키보드', 50000);
item2.displayInfo(); // 출력: 상품명: 키보드, 가격: 50000원, 카테고리: 기타

console.log(item1.name); // 출력: 노트북
console.log(item2.name); // 출력: 키보드

// 정적 속성 접근
console.log(Product.MAX_PRICE);     // 출력: 100000
console.log(Product.DESCRIPTION);   // 출력: 모든 상품의 기본 설명입니다.
console.log(Product.getProductCount()); // 출력: 최대 가격: 100000원, 상품 개수를 계산하는 로직 (예시)

// 인스턴스에서 정적 속성에 직접 접근할 수 없습니다.
// console.log(item1.MAX_PRICE); // undefined
```

> [!NOTE]
> 클래스 속성 문법은 `constructor`를 사용하지 않고도 인스턴스 속성을 선언할 수 있게 해주어 코드를 더 간결하게 만듭니다. 특히 초기값이 고정된 속성이나, `constructor`에서 복잡한 로직 없이 단순히 속성을 초기화할 때 유용합니다.
>
> 정적 속성은 클래스 자체에 속하는 데이터로, 모든 인스턴스가 공유하는 값이 아닙니다. 클래스 레벨의 상수나 유틸리티 정보 등을 저장할 때 사용됩니다.

---

## 4) 메서드(method)

클래스 안에 정의된 함수를 '메서드'라고 부릅니다. 메서드는 해당 클래스로 만들어진 객체가 수행할 수 있는 '행동'을 정의합니다. 예를 들어, `Person` 클래스에 `sayHello`라는 메서드를 만들면, `Person` 객체는 `sayHello`라는 행동을 할 수 있게 됩니다.

```javascript
class Person {
  constructor(name) {
    this.name = name;
  }

  // sayHello는 Person 클래스의 메서드입니다.
  // 이 메서드는 Person 객체의 name 속성을 사용하여 인사말을 출력합니다.
  sayHello() {                  // 메서드 정의: function 키워드 없이 바로 함수 이름과 괄호를 사용합니다.
    console.log('안녕하세요, 저는 ' + this.name + '입니다.');
  }

  // 또 다른 메서드: introduce
  introduce(job) {
    console.log(`제 이름은 ${this.name}이고, 직업은 ${job}입니다.`);
  }
}

const p2 = new Person('영희'); // 영희라는 이름의 Person 객체를 만듭니다.
p2.sayHello();                  // p2 객체의 sayHello 메서드를 호출합니다. 출력: '안녕하세요, 저는 영희입니다.'
p2.introduce('학생');           // p2 객체의 introduce 메서드를 호출합니다. 출력: '제 이름은 영희이고, 직업은 학생입니다.'
```

> [!NOTE]
> 메서드 안에서도 `this` 키워드를 사용하여 해당 객체의 속성(예: `this.name`)에 접근할 수 있습니다. 메서드는 객체의 데이터를 조작하거나, 객체와 관련된 특정 작업을 수행할 때 사용됩니다.

---

## 5) 상속(Inheritance)

상속은 이미 만들어진 클래스(부모 클래스)의 속성과 메서드를 물려받아 새로운 클래스(자식 클래스)를 만드는 기능입니다. 마치 부모가 자식에게 유전자를 물려주는 것과 같습니다. 자식은 부모의 특징을 가지면서도 자신만의 새로운 특징을 추가할 수 있습니다. 상속을 사용하면 코드의 중복을 줄이고, 더 효율적으로 코드를 관리할 수 있습니다.

상속을 구현할 때는 `extends` 키워드를 사용합니다.

```javascript
// 부모 클래스: Animal
class Animal {
  constructor(type) {
    this.type = type; // 동물의 종류 (예: 개, 고양이)
  }
  speak() {
    console.log(this.type + '가 소리를 냅니다.');
  }
}

// 자식 클래스: Dog (Animal 클래스를 상속받습니다.)
class Dog extends Animal {
  constructor(name) {
    // super()는 부모 클래스(Animal)의 constructor를 호출합니다.
    // 자식 클래스에서 constructor를 사용할 때는 반드시 super()를 먼저 호출해야 합니다.
    super('개');               // 부모 클래스인 Animal의 constructor에 '개'를 전달하여 type을 설정합니다.
    this.name = name;          // Dog 클래스만의 새로운 속성인 name을 설정합니다.
  }

  // speak 메서드를 재정의(Override)합니다.
  // 부모 클래스의 speak 메서드 대신 Dog 클래스의 speak 메서드가 실행됩니다.
  speak() {                    
    console.log(this.name + '가 멍멍!');
  }

  // Dog 클래스만의 새로운 메서드
  fetch() {
    console.log(this.name + '가 공을 가져옵니다.');
  }
}

const dog1 = new Dog('바둑이'); // Dog 클래스로 바둑이 객체를 만듭니다.
dog1.speak();                  // 출력: '바둑이가 멍멍!' (Dog 클래스의 speak 메서드 실행)
dog1.fetch();                  // 출력: '바둑이가 공을 가져옵니다.'

const cat1 = new Animal('고양이'); // Animal 클래스로 고양이 객체를 만듭니다.
cat1.speak();                  // 출력: '고양이가 소리를 냅니다.' (Animal 클래스의 speak 메서드 실행)
// cat1.fetch(); // 오류! Animal 클래스에는 fetch 메서드가 없습니다.
```

> [!TIP]
> `super()`를 사용해 부모 클래스의 생성자를 먼저 실행해야 `this`를 사용할 수 있습니다. 이는 자식 클래스가 부모 클래스의 속성들을 먼저 초기화해야 하기 때문입니다. 상속은 객체 지향 프로그래밍의 중요한 개념 중 하나로, 코드의 재사용성과 유지보수성을 높여줍니다.

## 6) 정적 메서드(static)

일반적인 메서드는 클래스로부터 객체(인스턴스)를 만들어야만 사용할 수 있습니다. 하지만 '정적 메서드(static method)'는 객체를 만들 필요 없이 **클래스 이름으로 바로 호출할 수 있는 메서드**입니다.

정적 메서드는 주로 클래스에 관련된 유틸리티 함수나, 객체와 독립적으로 동작하는 기능을 정의할 때 사용됩니다.

```javascript
class MathUtil {
  // static 키워드를 사용하여 정적 메서드를 정의합니다.
  static multiply(a, b) {
    return a * b;
  }

  static add(a, b) {
    return a + b;
  }

  static getPi() {
    return 3.14159;
  }
}

// 객체를 생성하지 않고 클래스 이름으로 바로 정적 메서드를 호출합니다.
console.log(MathUtil.multiply(3, 4)); // 출력: 12
console.log(MathUtil.add(10, 20));    // 출력: 30
console.log(MathUtil.getPi());        // 출력: 3.14159

// const myMath = new MathUtil(); // 정적 메서드는 객체를 만들 필요가 없습니다.
// myMath.multiply(1, 2); // 오류! 객체에서는 정적 메서드를 호출할 수 없습니다.
```

> [!NOTE]
> 정적 메서드 안에서는 `this` 키워드를 사용할 수 없습니다. 왜냐하면 정적 메서드는 특정 객체에 속한 것이 아니라 클래스 자체에 속하기 때문입니다. 따라서 `this`가 가리킬 객체가 없습니다.

---

## 7) 게터(getter)와 세터(setter)

게터(getter)와 세터(setter)는 객체의 속성(property)에 접근하는 특별한 메서드입니다. 이들은 겉으로는 일반 속성처럼 보이지만, 실제로는 함수처럼 동작하여 속성 값을 읽거나 설정할 때 추가적인 로직을 수행할 수 있게 해줍니다.

### 게터 (Getter)

게터는 객체의 속성 값을 '읽을' 때 사용됩니다. `get` 키워드를 사용하며, 함수처럼 호출하는 것이 아니라 일반 속성처럼 접근합니다. 게터 안에서 계산된 값을 반환하거나, 내부 속성을 외부로 노출하기 전에 가공하는 등의 작업을 할 수 있습니다.

```javascript
class Rectangle {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }

  // area는 게터입니다. rect.area처럼 속성처럼 접근하면 이 함수가 실행됩니다.
  get area() {                  
    console.log('면적을 계산합니다...'); // 속성 접근 시 추가적인 작업 수행
    return this.width * this.height;
  }
}

const rect = new Rectangle(3, 4);
console.log(rect.area);        // 출력: 면적을 계산합니다...
                               // 출력: 12 (area()가 아닌 속성처럼 사용)

const rect2 = new Rectangle(5, 10);
console.log(rect2.area);       // 출력: 면적을 계산합니다...
                               // 출력: 50
```

### 세터 (Setter)

세터는 객체의 속성 값을 '설정할' 때 사용됩니다. `set` 키워드를 사용하며, 속성에 값을 할당하는 것처럼 보이지만 실제로는 함수가 실행됩니다. 세터 안에서 유효성 검사를 하거나, 속성 값이 변경될 때 다른 작업을 수행하는 등의 로직을 추가할 수 있습니다.

```javascript
class User {
  constructor(name) {
    this._name = name; // 실제 이름을 저장하는 내부 속성 (보통 _를 붙여 내부용임을 표시)
  }

  get name() {
    return this._name;
  }

  // name은 세터입니다. user.name = '새이름'처럼 값을 할당하면 이 함수가 실행됩니다.
  set name(value) {            
    if (value.length < 2) {
      console.log('이름은 두 글자 이상이어야 합니다.');
      return; // 유효성 검사 실패 시 값 변경을 막습니다.
    }
    this._name = value;
    console.log(`이름이 ${value}로 변경되었습니다.`);
  }
}

const user = new User('홍길동');
console.log(user.name); // 출력: 홍길동

user.name = '김'; // 세터가 호출되고 유효성 검사에 실패합니다.
// 출력: 이름은 두 글자 이상이어야 합니다.
console.log(user.name); // 출력: 홍길동 (이름이 변경되지 않았습니다.)

user.name = '김철수'; // 세터가 호출되고 이름이 변경됩니다.
// 출력: 이름이 김철수로 변경되었습니다.
console.log(user.name); // 출력: 김철수
```

> [!NOTE]
> 게터와 세터는 객체의 내부 구현을 숨기고, 속성 접근을 제어할 수 있게 해주는 '캡슐화'라는 객체 지향 개념과 관련이 있습니다. 이를 통해 코드를 더 안전하고 유연하게 만들 수 있습니다.

---

- [목차로 돌아가기](../README.md)
- [이전 강의로 이동](07-ES6-Arrays-and-Objects.md)
- [다음 강의로 이동](09-ES6-Modules.md)
