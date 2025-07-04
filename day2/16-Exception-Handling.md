# 16. 예외 처리 (Exception Handling)

## 목차
1. [`try...catch` 문](#1-trycatch-문)
2. [`finally` 블록](#2-finally-블록)
3. [사용자 정의 예외 (Custom Errors)](#3-사용자-정의-예외-custom-errors)
4. [예외 처리의 중요성](#4-예외-처리의-중요성)

---

프로그램을 만들다 보면 예상치 못한 문제나 오류가 발생할 수 있습니다. 예를 들어, 존재하지 않는 파일을 읽으려 하거나, 숫자가 아닌 값을 가지고 나누기 연산을 하려 할 때 등이 있습니다. 이러한 예상치 못한 문제를 **예외(Exception)** 또는 **오류(Error)**라고 부릅니다. 예외가 발생하면 프로그램이 갑자기 멈춰버리거나 오작동할 수 있습니다.

**예외 처리(Exception Handling)**는 프로그램이 예외 상황에서도 멈추지 않고 안정적으로 동작하도록 미리 대비하는 방법입니다.

## 1) `try...catch` 문

JavaScript에서 예외를 처리하는 가장 기본적인 방법은 `try...catch` 문을 사용하는 것입니다. `try...catch` 문은 다음과 같이 작동합니다.

*   **`try` 블록**: 예외가 발생할 가능성이 있는 코드를 이 안에 작성합니다. 프로그램은 이 `try` 블록 안의 코드를 실행하다가 예외가 발생하면 즉시 `catch` 블록으로 이동합니다.
*   **`catch` 블록**: `try` 블록에서 예외가 발생했을 때 실행되는 코드입니다. `catch` 괄호 `()` 안에는 발생한 예외 객체(Error 객체)가 전달되어, 어떤 종류의 예외가 발생했는지 정보를 얻을 수 있습니다.

```javascript
console.log('--- 예외 처리 시작 ---');

try {
  // 이 블록 안에 예외가 발생할 수 있는 코드를 작성합니다.
  // 예를 들어, 존재하지 않는 변수에 접근하는 경우
  console.log(nonExistentVariable); // nonExistentVariable은 정의되지 않았으므로 여기서 ReferenceError 발생
  console.log('이 메시지는 출력되지 않습니다.'); // 예외 발생 시 이 아래 코드는 건너뜁니다.
} catch (error) {
  // `try` 블록에서 예외가 발생하면 이 `catch` 블록이 실행됩니다.
  // `error` 변수에는 발생한 예외에 대한 정보가 담겨 있습니다.
  console.error('예외가 발생했습니다!');
  console.error('오류 이름:', error.name);    // 예: ReferenceError, TypeError 등
  console.error('오류 메시지:', error.message); // 예: nonExistentVariable is not defined
}

console.log('--- 예외 처리 끝 (프로그램이 멈추지 않았습니다) ---');

// 0으로 나누는 예시
function divide(a, b) {
  try {
    if (b === 0) {
      // throw 키워드를 사용하여 의도적으로 예외를 발생시킬 수 있습니다.
      // Error 객체를 생성하여 메시지를 전달합니다.
      throw new Error('0으로 나눌 수 없습니다.');
    }
    return a / b;
  } catch (error) {
    console.error('나누기 연산 중 오류 발생:', error.message);
    return NaN; // Not a Number (숫자가 아님) 반환
  }
}

console.log(divide(10, 2)); // 출력: 5
console.log(divide(10, 0)); // 출력: 나누기 연산 중 오류 발생: 0으로 나눌 수 없습니다.
                            // 출력: NaN
```

**코드 상세 설명:**

*   **`try { ... }`**: 예외 발생 가능성이 있는 코드를 감싸는 블록입니다. JavaScript 엔진은 이 블록 안의 코드를 실행하다가 문제가 생기면 즉시 실행을 멈추고 `catch` 블록으로 제어권을 넘깁니다. `try` 블록 내에서 예외가 발생한 지점 이후의 코드는 실행되지 않습니다.
*   **`catch (error) { ... }`**: `try` 블록에서 예외가 발생했을 때만 실행되는 블록입니다. `error` 매개변수는 발생한 예외에 대한 정보를 담고 있는 `Error` 객체입니다. 이 객체는 `name` (예외의 종류, 예: `ReferenceError`, `TypeError`)과 `message` (예외에 대한 상세 설명) 속성을 가집니다.
*   **`console.error(...)`**: `console.log`와 비슷하지만, 오류 메시지를 출력할 때 사용됩니다. 대부분의 개발자 도구에서는 `console.error`로 출력된 메시지를 빨간색으로 표시하여 쉽게 구분할 수 있도록 돕습니다.
*   **`throw new Error('메시지');`**: `throw` 키워드를 사용하면 개발자가 의도적으로 예외를 발생시킬 수 있습니다. `new Error('메시지')`는 새로운 `Error` 객체를 생성하며, 이 객체에 오류에 대한 설명을 담을 수 있습니다. 이렇게 발생시킨 예외는 가장 가까운 `catch` 블록에 의해 잡히게 됩니다.

> [!NOTE]
> `try...catch` 문을 사용하면 프로그램이 예외로 인해 갑자기 종료되는 것을 막고, 예외 상황에 대한 적절한 대응 로직(예: 사용자에게 오류 메시지 표시, 로그 기록, 기본값 반환 등)을 구현할 수 있습니다.

## 2) `finally` 블록

`try...catch` 문에는 선택적으로 `finally` 블록을 추가할 수 있습니다. `finally` 블록은 `try` 블록의 코드가 성공적으로 실행되었든, `catch` 블록에서 예외가 처리되었든 상관없이 **항상 실행되는 코드**를 포함합니다.

주로 파일 닫기, 네트워크 연결 해제, 리소스 정리와 같이 예외 발생 여부와 관계없이 반드시 수행되어야 하는 작업들을 `finally` 블록에 작성합니다.

```javascript
console.log('--- finally 예시 시작 ---');

function processData(data) {
  try {
    console.log('데이터 처리 시작...');
    if (typeof data !== 'number') {
      throw new TypeError('입력 데이터는 숫자여야 합니다.');
    }
    const result = data * 2;
    console.log('처리 결과:', result);
    return result;
  } catch (error) {
    console.error('오류 발생:', error.message);
    return null;
  } finally {
    // 이 블록은 try 또는 catch 블록 실행 후 항상 실행됩니다.
    console.log('데이터 처리 완료 (finally 블록 실행).');
  }
}

processData(10);   // 정상 실행
// 출력:
// 데이터 처리 시작...
// 처리 결과: 20
// 데이터 처리 완료 (finally 블록 실행).

processData('abc'); // 예외 발생
// 출력:
// 데이터 처리 시작...
// 오류 발생: 입력 데이터는 숫자여야 합니다.
// 데이터 처리 완료 (finally 블록 실행).

console.log('--- finally 예시 끝 ---');
```

**코드 상세 설명:**

*   **`finally { ... }`**: `try` 블록이 성공적으로 완료되거나, `catch` 블록에서 예외가 처리된 후에 항상 실행되는 코드 블록입니다. `return` 문이 `try` 또는 `catch` 블록에 있더라도 `finally` 블록은 실행된 후에 함수가 종료됩니다.
*   예시에서는 `processData` 함수가 숫자를 받으면 정상적으로 처리하고, 숫자가 아닌 값을 받으면 `TypeError`를 발생시킵니다. 어떤 경우든 `finally` 블록의 `console.log('데이터 처리 완료 (finally 블록 실행).');`는 항상 출력되는 것을 확인할 수 있습니다.

> [!TIP]
> `finally` 블록은 리소스 누수를 방지하는 데 매우 중요합니다. 예를 들어, 파일을 열었으면 예외 발생 여부와 관계없이 반드시 닫아야 할 때 `finally`에 파일 닫기 코드를 넣을 수 있습니다.

## 3) 사용자 정의 예외 (Custom Errors)

JavaScript는 `Error`, `TypeError`, `ReferenceError` 등 여러 내장 예외 타입을 제공합니다. 하지만 때로는 프로그램의 특정 상황을 더 명확하게 나타내기 위해 개발자가 직접 예외 타입을 정의해야 할 필요가 있습니다. 예를 들어, '사용자 인증 실패'나 '재고 부족'과 같은 특정 비즈니스 로직 오류를 나타내고 싶을 때 사용자 정의 예외를 만들 수 있습니다.

사용자 정의 예외는 일반적으로 내장 `Error` 클래스를 상속받아 만듭니다.

```javascript
// CustomError는 Error 클래스를 상속받습니다.
class CustomError extends Error {
  constructor(message, code) {
    super(message); // 부모 클래스(Error)의 constructor를 호출하여 message를 전달합니다.
    this.name = 'CustomError'; // 예외의 이름을 CustomError로 설정합니다.
    this.code = code; // 사용자 정의 오류 코드를 추가할 수 있습니다.
  }
}

function checkAge(age) {
  if (age < 0) {
    // 나이가 음수일 경우 CustomError를 발생시킵니다.
    throw new CustomError('나이는 음수일 수 없습니다.', 'INVALID_AGE');
  } else if (age < 18) {
    throw new CustomError('미성년자는 접근할 수 없습니다.', 'ACCESS_DENIED');
  }
  console.log('접근 허용.');
}

try {
  checkAge(-5);
} catch (error) {
  console.error('오류 발생:', error.name);
  console.error('메시지:', error.message);
  console.error('코드:', error.code); // 사용자 정의 코드 출력
}
// 출력:
// 오류 발생: CustomError
// 메시지: 나이는 음수일 수 없습니다.
// 코드: INVALID_AGE

try {
  checkAge(15);
} catch (error) {
  console.error('오류 발생:', error.name);
  console.error('메시지:', error.message);
  console.error('코드:', error.code);
}
// 출력:
// 오류 발생: CustomError
// 메시지: 미성년자는 접근할 수 없습니다.
// 코드: ACCESS_DENIED

try {
  checkAge(20);
} catch (error) {
  console.error('오류 발생:', error.name);
  console.error('메시지:', error.message);
}
// 출력: 접근 허용.
```

**코드 상세 설명:**

*   **`class CustomError extends Error { ... }`**: `CustomError`라는 새로운 클래스를 정의하고, JavaScript의 내장 `Error` 클래스를 `extends` 키워드를 사용하여 상속받습니다. 이렇게 하면 `CustomError`는 `Error` 클래스의 모든 기능을 물려받게 됩니다.
*   **`constructor(message, code) { ... }`**: `CustomError` 클래스의 생성자입니다.
    *   **`super(message);`**: 자식 클래스의 생성자에서는 반드시 `super()`를 호출하여 부모 클래스(`Error`)의 생성자를 먼저 실행해야 합니다. `message`는 `Error` 객체의 `message` 속성으로 전달됩니다.
    *   **`this.name = 'CustomError';`**: 예외의 `name` 속성을 `CustomError`로 설정합니다. 이렇게 하면 `catch` 블록에서 `error.name`을 통해 어떤 종류의 사용자 정의 예외인지 쉽게 식별할 수 있습니다.
    *   **`this.code = code;`**: `code`와 같은 추가적인 속성을 정의하여 예외에 대한 더 구체적인 정보를 담을 수 있습니다. 이는 오류를 분류하거나 특정 오류에 대한 처리를 다르게 할 때 유용합니다.
*   **`throw new CustomError(...)`**: `checkAge` 함수 내에서 특정 조건(`age < 0` 또는 `age < 18`)이 만족될 때 `CustomError`의 새로운 인스턴스를 생성하여 `throw`합니다. 이 예외는 `try...catch` 블록에 의해 잡히게 됩니다.

> [!TIP]
> 사용자 정의 예외는 코드의 가독성을 높이고, 특정 오류 상황에 대한 처리를 더 명확하게 분리할 수 있게 해줍니다. 특히 복잡한 애플리케이션에서 다양한 종류의 오류를 체계적으로 관리할 때 유용합니다.

## 4) 예외 처리의 중요성

예외 처리는 안정적이고 견고한 프로그램을 만드는 데 매우 중요합니다. 예외 처리를 제대로 하지 않으면 다음과 같은 문제가 발생할 수 있습니다.

*   **프로그램 비정상 종료**: 예상치 못한 오류로 인해 프로그램이 갑자기 멈춰버려 사용자에게 불편을 주거나 중요한 데이터가 손실될 수 있습니다.
*   **잘못된 결과**: 오류가 발생했음에도 불구하고 프로그램이 계속 실행되어 잘못된 계산 결과나 데이터가 생성될 수 있습니다.
*   **보안 취약점**: 오류 메시지에 민감한 정보(예: 데이터베이스 비밀번호, 서버 경로)가 포함되어 노출될 경우 보안 문제가 발생할 수 있습니다.
*   **사용자 경험 저하**: 프로그램이 자주 멈추거나 알 수 없는 오류 메시지를 보여주면 사용자는 불편함을 느끼고 프로그램을 사용하지 않게 될 수 있습니다.

예외 처리를 통해 개발자는 오류 상황을 예측하고, 이에 대한 적절한 대응 로직을 구현하여 프로그램의 안정성을 높이고 사용자에게 더 나은 경험을 제공할 수 있습니다.

---

- [목차로 돌아가기](../README.md)
- [이전 강의로 이동](15-Node.js-JavaScript.md)
- [다음 강의로 이동](Lab1-Simple-Calculator.md)
