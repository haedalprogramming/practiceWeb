# 09. ES6+ 비동기 처리

## 1) 동기(Synchronous)와 비동기(Asynchronous)

프로그래밍에서 '동기(Synchronous)'와 '비동기(Asynchronous)'는 코드가 실행되는 방식과 순서를 나타내는 중요한 개념입니다. 이 개념을 이해하는 것은 JavaScript, 특히 웹 개발에서 매우 중요합니다.

### 1-1) 동기 (Synchronous)

동기 방식은 컴퓨터가 코드를 **한 줄씩 순서대로 실행**하는 것을 의미합니다. 마치 여러분이 은행에서 줄을 서서 기다리는 것과 같습니다. 앞 사람이 업무를 마쳐야만 다음 사람이 업무를 시작할 수 있습니다. 어떤 작업이 시작되면 그 작업이 완전히 끝날 때까지 다음 작업은 기다려야 합니다.

```javascript
console.log('1. 커피를 주문합니다.');
console.log('2. 커피가 나올 때까지 기다립니다.'); // 이 작업이 끝나야 다음 작업으로 넘어갑니다.
console.log('3. 커피를 받습니다.');
// 실행 결과:
// 1. 커피를 주문합니다.
// 2. 커피가 나올 때까지 기다립니다.
// 3. 커피를 받습니다.
```

### 1-2) 비동기 (Asynchronous)

비동기 방식은 시간이 오래 걸리는 작업(예: 인터넷에서 데이터를 가져오는 작업, 큰 파일 읽기 등)을 다른 작업과 **함께** 시작하고, 그 작업이 완료되면 나중에 결과를 알려주는 방식입니다. 마치 식당에서 음식을 주문하고, 음식이 나올 때까지 기다리지 않고 다른 일을 하다가 음식이 나오면 알려주는 것과 같습니다.

```javascript
console.log('A. 웹 페이지를 로드합니다.');

// setTimeout은 비동기 함수입니다. 1초 후에 'B'를 출력하라고 예약만 해두고 바로 다음 코드로 넘어갑니다.
setTimeout(() => {
  console.log('B. 서버에서 데이터를 성공적으로 가져왔습니다.');
}, 1000); // 1000밀리초 = 1초 후에 실행

console.log('C. 웹 페이지의 다른 부분을 표시합니다.');

// 실행 결과:
// A. 웹 페이지를 로드합니다.
// C. 웹 페이지의 다른 부분을 표시합니다.
// (1초 후)
// B. 서버에서 데이터를 성공적으로 가져왔습니다.
```

> [!NOTE]
> 웹 개발에서 비동기 처리가 중요한 이유는, 웹 페이지가 서버에서 데이터를 가져오거나 이미지를 로드하는 동안 사용자가 아무것도 할 수 없게 멈춰버리면 안 되기 때문입니다. 비동기 처리를 통해 웹 페이지는 데이터를 기다리는 동안에도 다른 부분을 계속 보여주거나 사용자의 입력을 받을 수 있어, 더 부드럽고 반응성이 좋은 사용자 경험을 제공할 수 있습니다.

---

## 2) 콜백 함수(Callback)

콜백 함수는 비동기 작업이 끝난 후 **호출될 함수**를 미리 전달하는 방식입니다. 마치 친구에게 '내가 숙제를 다 하면, 네가 이어서 게임을 시작해줘'라고 부탁하는 것과 같습니다. 여기서 '네가 이어서 게임을 시작하는 것'이 콜백 함수가 됩니다.

JavaScript에서 비동기 작업을 처리하는 가장 기본적인 방법 중 하나입니다.

```javascript
function fetchData(callback) {
  console.log('데이터를 요청합니다...');
  // setTimeout은 비동기 작업을 흉내 내기 위해 사용합니다. 실제로는 서버 통신 등이 될 수 있습니다.
  setTimeout(() => {
    const data = '서버에서 가져온 데이터';
    console.log('데이터 수신 완료!');
    callback(data); // 데이터 수신이 완료되면, 미리 전달받은 callback 함수를 실행합니다.
  }, 2000); // 2초 후에 데이터가 도착했다고 가정합니다.
}

// fetchData 함수를 호출하면서, 데이터가 도착하면 실행될 콜백 함수를 전달합니다.
fetchData((receivedData) => {
  console.log('콜백 함수 실행: ', receivedData); // '콜백 함수 실행: 서버에서 가져온 데이터'
});

console.log('데이터를 기다리는 동안 다른 작업을 수행합니다.');
// 실행 순서:
// 1. 데이터를 요청합니다...
// 2. 데이터를 기다리는 동안 다른 작업을 수행합니다.
// (2초 후)
// 3. 데이터 수신 완료!
// 4. 콜백 함수 실행: 서버에서 가져온 데이터
```

> [!WARNING]
> 콜백 함수는 비동기 처리에 유용하지만, 여러 비동기 작업이 순차적으로 이어질 경우 콜백 함수가 계속 중첩되어 코드가 깊어지고 복잡해지는 현상이 발생할 수 있습니다. 이를 **콜백 헬(Callback Hell)** 또는 '지옥의 피라미드'라고 부릅니다. 콜백 헬은 코드의 가독성을 해치고 유지보수를 어렵게 만듭니다. 이러한 문제를 해결하기 위해 Promise와 async/await가 등장했습니다.

## 3) Promise

**Promise**는 비동기 작업의 성공/실패를 다루기 위한 JavaScript의 객체입니다. 콜백 헬 문제를 해결하고 비동기 코드를 더 깔끔하고 체계적으로 작성할 수 있도록 도와줍니다. Promise는 '미래의 어떤 시점에 결과를 제공하겠다는 약속'과 같습니다.

### Promise의 상태

Promise는 다음 세 가지 상태 중 하나를 가집니다.

*   **대기 (Pending)**: 비동기 작업이 아직 완료되지 않은 초기 상태입니다.
*   **이행 (Fulfilled)**: 비동기 작업이 성공적으로 완료된 상태입니다. 결과 값을 반환합니다.
*   **거부 (Rejected)**: 비동기 작업이 실패한 상태입니다. 오류 정보를 반환합니다.

한 번 이행되거나 거부된 Promise는 다시 대기 상태로 돌아갈 수 없습니다. 즉, 한 번 결정된 Promise는 변하지 않습니다.

### Promise 사용하기

Promise는 `new Promise()` 생성자를 통해 만들 수 있습니다. 생성자는 `resolve`와 `reject`라는 두 개의 함수를 인자로 받는 콜백 함수를 가집니다. `resolve`는 작업이 성공했을 때 호출하고, `reject`는 작업이 실패했을 때 호출합니다.

```javascript
const myPromise = new Promise((resolve, reject) => {
  // 비동기 작업을 수행합니다. (예: 서버에서 데이터 가져오기)
  setTimeout(() => {
    const success = Math.random() > 0.5; // 50% 확률로 성공 또는 실패
    if (success) {
      resolve('데이터 로드 성공!'); // 작업 성공 시 resolve 호출
    } else {
      reject('데이터 로드 실패...'); // 작업 실패 시 reject 호출
    }
  }, 1500); // 1.5초 후에 작업 완료
});

// Promise의 결과를 처리합니다.
myPromise
  .then(result => { // 작업이 성공적으로 이행(resolve)되었을 때 실행됩니다.
    console.log('성공: ', result); 
  })
  .catch(error => { // 작업이 거부(reject)되었을 때 실행됩니다.
    console.error('실패: ', error); 
  })
  .finally(() => { // 작업의 성공/실패 여부와 상관없이 항상 마지막에 실행됩니다.
    console.log('작업 완료!');
  });

console.log('Promise 작업 시작!');
```

> [!TIP]
> Promise를 사용하면 비동기 코드를 순차적으로 연결하여 작성할 수 있어 콜백 헬을 피할 수 있습니다. `.then()` 메서드를 체인처럼 연결하여 여러 비동기 작업을 순서대로 처리할 수 있습니다. 항상 `.catch()`를 붙여 에러를 처리하는 습관을 들이는 것이 중요합니다. 이는 예상치 못한 오류로 인해 프로그램이 멈추는 것을 방지해줍니다.

---

## 4) async/await

`async`와 `await`는 ES8(ECMAScript 2017)에서 도입된 비동기 코드를 작성하는 가장 현대적이고 강력한 방법입니다. Promise를 기반으로 하지만, 마치 동기 코드처럼 비동기 코드를 읽고 쓸 수 있게 해주어 코드의 가독성을 크게 향상시킵니다.

### `async` 함수

`async` 키워드는 함수 앞에 붙여서 해당 함수가 비동기 함수임을 선언합니다. `async` 함수는 항상 Promise를 반환합니다. 함수 안에서 `await` 키워드를 사용할 수 있게 해줍니다.

```javascript
async function fetchUserData() {
  // 이 함수는 비동기 작업을 수행할 것입니다.
  return "사용자 데이터";
}

fetchUserData().then(data => console.log(data)); // 출력: 사용자 데이터
```

### `await` 키워드

`await` 키워드는 `async` 함수 안에서만 사용할 수 있습니다. `await`는 Promise가 '이행(fulfilled)'되거나 '거부(rejected)'될 때까지 함수의 실행을 **일시 중지**시킵니다. Promise가 완료되면, `await`는 Promise의 결과 값을 반환합니다. 마치 비동기 작업이 끝날 때까지 잠시 멈춰서 기다려주는 것과 같습니다.

```javascript
// Promise를 반환하는 가상의 함수
function delay(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

async function processData() {
  console.log('데이터 처리 시작...');
  await delay(2000); // 2초 동안 기다립니다.
  console.log('2초 대기 후 데이터 처리 계속...');

  try {
    const response = await new Promise((resolve, reject) => {
      setTimeout(() => {
        const success = Math.random() > 0.5;
        if (success) {
          resolve('성공적으로 데이터를 가져왔습니다!');
        } else {
          reject('데이터 가져오기 실패!');
        }
      }, 1500);
    });
    console.log(response); // 성공 메시지 출력
  } catch (error) {
    console.error('오류 발생: ', error); // 실패 메시지 출력
  }
  console.log('데이터 처리 완료.');
}

processData();
// 실행 순서:
// 1. 데이터 처리 시작...
// (2초 대기)
// 2. 2초 대기 후 데이터 처리 계속...
// 3. 성공적으로 데이터를 가져왔습니다! 또는 오류 발생: 데이터 가져오기 실패!
// 4. 데이터 처리 완료.
```

> [!NOTE]
> `async/await`는 Promise의 문법적 설탕(syntactic sugar)이라고 불립니다. 즉, Promise를 더 쉽게 사용할 수 있도록 도와주는 문법일 뿐, Promise의 기본적인 동작 방식은 동일합니다. `try...catch` 블록을 사용하여 `await`로 기다리는 Promise에서 발생할 수 있는 오류를 처리하는 것이 중요합니다.

## 5) Promise.all과 Promise.race

여러 개의 비동기 작업을 동시에 처리해야 할 때 `Promise.all()`과 `Promise.race()`와 같은 유용한 메서드를 사용할 수 있습니다. 이들은 여러 Promise를 한 번에 관리하고, 그 결과에 따라 다른 동작을 수행할 수 있게 해줍니다.

### 5-1) `Promise.all()`

`Promise.all()`은 여러 개의 Promise를 배열로 받아서, **모든 Promise가 성공적으로 완료될 때까지 기다립니다.** 모든 Promise가 성공하면, 각 Promise의 결과 값을 배열로 반환합니다. 만약 하나라도 실패하면, 전체 `Promise.all()`은 실패하고 가장 먼저 실패한 Promise의 오류를 반환합니다.

```javascript
const p1 = new Promise(resolve => setTimeout(() => resolve('첫 번째 데이터'), 1000));
const p2 = new Promise(resolve => setTimeout(() => resolve('두 번째 데이터'), 500));
const p3 = new Promise(resolve => setTimeout(() => resolve('세 번째 데이터'), 1500));

console.log('모든 데이터 요청 시작...');

Promise.all([p1, p2, p3])
  .then(results => {
    // 모든 Promise가 성공적으로 완료되면 이 부분이 실행됩니다.
    console.log('모든 데이터 로드 완료:', results);
    // 출력: 모든 데이터 로드 완료: ['첫 번째 데이터', '두 번째 데이터', '세 번째 데이터']
  })
  .catch(error => {
    // 하나라도 실패하면 이 부분이 실행됩니다.
    console.error('데이터 로드 중 오류 발생:', error);
  });

// 실패하는 경우 예시
const p4 = new Promise((resolve, reject) => setTimeout(() => resolve('성공'), 1000));
const p5 = new Promise((resolve, reject) => setTimeout(() => reject('실패!'), 500));

Promise.all([p4, p5])
  .then(results => console.log(results))
  .catch(error => console.error('Promise.all 실패:', error)); // 출력: Promise.all 실패: 실패!
```

> [!NOTE]
> `Promise.all()`은 여러 개의 독립적인 비동기 작업을 동시에 시작하고, 모든 작업이 완료된 후에 그 결과를 한 번에 처리하고 싶을 때 유용합니다. 예를 들어, 웹 페이지를 로드할 때 여러 개의 이미지나 데이터를 동시에 가져와야 할 때 사용할 수 있습니다.

### 5-2) `Promise.race()`

`Promise.race()`는 여러 개의 Promise를 배열로 받아서, **가장 먼저 완료된 Promise의 결과(성공 또는 실패)를 반환합니다.** 나머지 Promise들은 무시됩니다. 마치 여러 경주마 중 가장 먼저 결승선에 도착한 말의 결과만 보는 것과 같습니다.

```javascript
const fastPromise = new Promise(resolve => setTimeout(() => resolve('가장 빠른 결과!'), 300));
const slowPromise = new Promise(resolve => setTimeout(() => resolve('느린 결과...'), 1000));
const errorPromise = new Promise((resolve, reject) => setTimeout(() => reject('오류 발생!'), 500));

console.log('경주 시작!');

Promise.race([fastPromise, slowPromise, errorPromise])
  .then(result => {
    // 가장 먼저 성공한 Promise의 결과가 반환됩니다.
    console.log('가장 먼저 완료된 결과:', result);
    // 출력: 가장 먼저 완료된 결과: 가장 빠른 결과!
  })
  .catch(error => {
    // 가장 먼저 실패한 Promise의 오류가 반환됩니다.
    console.error('경주 중 오류 발생:', error);
  });

// 오류가 더 빨리 발생하는 경우
Promise.race([slowPromise, errorPromise])
  .then(result => console.log(result))
  .catch(error => console.error('Promise.race 오류:', error)); // 출력: Promise.race 오류: 오류 발생!
```

> [!NOTE]
> `Promise.race()`는 여러 비동기 작업 중 가장 먼저 완료되는 작업의 결과만 필요할 때 사용합니다. 예를 들어, 여러 서버에 동일한 요청을 보내고 가장 먼저 응답하는 서버의 데이터를 사용하거나, 특정 시간 안에 응답이 오지 않으면 타임아웃 처리를 하고 싶을 때 유용하게 활용할 수 있습니다.

---

- [목차로 돌아가기](../README.md)
- [이전 강의로 이동](08-ES6-Modules.md)
- [다음 강의로 이동](10-ES6-Advanced-Features.md)
