# 08. ES6+ 비동기 처리

## 1) 동기(Synchronous)와 비동기(Asynchronous)
- **동기**: 컴퓨터가 한 줄씩 순서대로 코드를 실행합니다. 앞 작업이 끝나야 다음 작업이 시작됩니다.
- **비동기**: 시간이 걸리는 작업(예: 네트워크 요청)을 다른 작업과 **함께** 시작하고, 완료되면 결과를 알려줍니다.

```javascript
console.log('A');
setTimeout(() => console.log('B'), 1000); // 1초 후에 실행되는 비동기 함수
console.log('C');
// 실행 결과: A → C → B
```

## 2) 콜백 함수(Callback)
- 비동기 작업이 끝난 후 **호출될 함수**를 미리 전달하는 방식

```javascript
function fetchData(callback) {
  setTimeout(() => {
    callback('서버 데이터');
  }, 1000);
}

fetchData((data) => {
  console.log(data); // '서버 데이터'
});
```

> [!TIP]
> 콜백이 여러 번 중첩되면 코드가 복잡해지는 **콜백 헬(callback hell)**이 발생할 수 있습니다.

## 3) Promise
- **Promise**는 비동기 작업의 성공/실패를 다루기 위한 객체입니다.

```javascript
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    const success = true;
    if (success) {
      resolve('완료!');
    } else {
      reject('실패!');
    }
  }, 1000);
});

promise
  .then(result => console.log(result))  // 성공 시 실행
  .catch(error => console.error(error)); // 실패 시 실행
```

> [!TIP]
> 항상 `.catch()`를 붙여 에러를 처리하세요.

## 4) async/await
- **async 함수**: 비동기 코드를 작성할 때 가독성을 높여줍니다.
- **await**: Promise가 처리될 때까지 기다립니다.

```javascript
async function getData() {
  try {
    const result = await promise; // promise가 완료될 때까지 대기
    console.log(result);            // '완료!'
  } catch (error) {
    console.error(error);           // '실패!'
  }
}

getData();
```

## 5) Promise.all과 Promise.race
- `Promise.all([...])`: 모든 Promise가 완료될 때까지 기다립니다.
- `Promise.race([...])`: 가장 먼저 완료된 Promise 결과를 사용합니다.

```javascript
const p1 = new Promise(res => setTimeout(() => res('첫 번째'), 500));
const p2 = new Promise(res => setTimeout(() => res('두 번째'), 1000));

// 모두 완료 후 실행
Promise.all([p1, p2]).then(results => console.log(results)); // ['첫 번째', '두 번째']

// 가장 먼저 완료된 결과
Promise.race([p1, p2]).then(result => console.log(result)); // '첫 번째'
```

---