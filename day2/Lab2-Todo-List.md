# Lab 2: CLI Todo List Manager (명령줄 할 일 관리자)

## 목차
1. [목표](#1-목표)
2. [예상 소요 시간](#2-예상-소요-시간)
3. [튜토리얼 시작하기](#3-튜토리얼-시작하기)
   - [3-1) 1단계: 프로젝트 초기 설정](#3-1-1단계-프로젝트-초기-설정)
   - [3-2) 2단계: 할 일 데이터 불러오기 및 저장하기](#3-2-2단계-할-일-데이터-불러오기-및-저장하기)
   - [3-3) 3단계: 명령줄 인수 처리하기](#3-3-3단계-명령줄-인수-처리하기)
   - [3-4) 4단계: 할 일 추가 (`add`) 기능 구현](#3-4-4단계-할-일-추가-add-기능-구현)
   - [3-5) 5단계: 할 일 목록 보기 (`list`) 기능 구현](#3-5-5단계-할-일-목록-보기-list-기능-구현)
   - [3-6) 6단계: 할 일 완료 (`complete`) 기능 구현](#3-6-6단계-할-일-완료-complete-기능-구현)
   - [3-7) 7단계: 할 일 삭제 (`delete`) 기능 구현](#3-7-7단계-할-일-삭제-delete-기능-구현)
4. [최종 테스트](#4-최종-테스트)
5. [마무리](#5-마무리)

---

## 1) 목표

Node.js 환경에서 명령줄(터미널)을 통해 할 일(Todo) 목록을 관리하는 간단한 애플리케이션을 만듭니다. 할 일 목록은 파일에 저장되어 프로그램이 종료되어도 유지되도록 합니다. 이 실습을 통해 Node.js의 파일 시스템(`fs` 모듈) 사용법과, 배열 및 객체를 활용한 데이터 관리, 그리고 명령줄 인수를 처리하는 방법을 익히게 됩니다.

## 2) 예상 소요 시간

1시간

## 3) 튜토리얼 시작하기

### 3-1) 1단계: 프로젝트 초기 설정

먼저, 이 프로젝트를 위한 새로운 JavaScript 파일과 데이터를 저장할 JSON 파일을 만듭니다.

1.  `day2` 폴더 안에 `todo.js`라는 이름의 새 파일을 만듭니다.
2.  `day2` 폴더 안에 `todos.json`이라는 이름의 새 파일을 만듭니다. 이 파일은 처음에는 비어있거나, `[]` (빈 배열)을 내용으로 가질 수 있습니다.

    ```json
    // day2/todos.json 파일 내용
    []
    ```

### 3-2) 2단계: 할 일 데이터 불러오기 및 저장하기

할 일 목록은 `todos.json` 파일에 저장될 것입니다. 프로그램이 시작될 때 이 파일을 읽어와서 할 일 목록을 불러오고, 할 일 목록이 변경될 때마다 다시 파일에 저장해야 합니다.

`todo.js` 파일을 열고 다음 코드를 작성합니다.

```javascript
// todo.js 파일

// Node.js의 파일 시스템 모듈을 불러옵니다.
// fs 모듈은 파일을 읽거나 쓰는 등의 파일 관련 작업을 할 때 사용됩니다.
const fs = require('fs');

// 할 일 데이터가 저장될 파일 경로를 정의합니다.
const TODO_FILE = './todos.json';

// 할 일 목록을 저장할 배열 변수를 선언합니다.
let todos = [];

// 1. 파일에서 할 일 목록을 불러오는 함수
function loadTodos() {
  try {
    // TODO_FILE 경로의 파일을 동기적으로 읽어옵니다.
    // 'utf8'은 파일의 내용을 텍스트로 해석하라는 의미입니다.
    const data = fs.readFileSync(TODO_FILE, 'utf8');
    // 읽어온 JSON 문자열을 JavaScript 객체(배열)로 변환합니다.
    todos = JSON.parse(data);
    console.log('할 일 목록을 불러왔습니다.');
  } catch (error) {
    // 파일이 없거나 JSON 형식이 잘못된 경우 오류가 발생할 수 있습니다.
    // 이 경우, 빈 배열로 시작하여 새로운 할 일 목록을 만듭니다.
    console.log('할 일 파일이 없거나 읽을 수 없습니다. 새 목록을 시작합니다.');
    todos = [];
  }
}

// 2. 할 일 목록을 파일에 저장하는 함수
function saveTodos() {
  try {
    // JavaScript 객체(배열)를 JSON 문자열로 변환합니다.
    // null, 2는 JSON.stringify의 추가 옵션으로, 가독성 좋게 들여쓰기하여 저장합니다.
    const data = JSON.stringify(todos, null, 2);
    // TODO_FILE 경로에 JSON 문자열을 동기적으로 씁니다.
    fs.writeFileSync(TODO_FILE, data, 'utf8');
    console.log('할 일 목록을 저장했습니다.');
  } catch (error) {
    console.error('할 일 목록 저장 중 오류 발생:', error.message);
  }
}

// 프로그램 시작 시 할 일 목록을 불러옵니다.
loadTodos();

// --- 여기에 할 일 관리 기능들이 추가될 예정입니다. ---

// 프로그램 종료 시 할 일 목록을 저장합니다.
// process.on('exit', ...)는 Node.js 프로그램이 종료되기 직전에 실행될 코드를 등록합니다.
process.on('exit', () => {
  saveTodos();
});
```

**코드 설명:**

*   `require('fs')`: Node.js에서 파일을 다루기 위한 `fs` (File System) 모듈을 가져옵니다.
*   `TODO_FILE`: 할 일 데이터가 저장될 파일의 이름을 상수로 정의했습니다.
*   `loadTodos()`:
    *   `fs.readFileSync(TODO_FILE, 'utf8')`: `todos.json` 파일을 읽어옵니다. `readFileSync`는 파일 읽기가 완료될 때까지 다음 코드를 실행하지 않고 기다리는 **동기(Synchronous)** 방식입니다.
    *   `JSON.parse(data)`: 파일에서 읽어온 내용은 문자열 형태이므로, JavaScript에서 사용할 수 있는 배열/객체 형태로 변환(`parse`)합니다.
    *   `try...catch`: 파일이 없거나 내용이 잘못된 경우 오류가 발생할 수 있으므로, `try...catch` 문으로 오류를 처리합니다. 오류가 나면 `todos`를 빈 배열로 초기화하여 프로그램이 계속 실행되도록 합니다.
*   `saveTodos()`:
    *   `JSON.stringify(todos, null, 2)`: JavaScript 배열/객체를 파일에 저장할 수 있는 JSON 문자열 형태로 변환(`stringify`)합니다. `null, 2`는 JSON 문자열을 예쁘게 들여쓰기하여 사람이 읽기 쉽게 만들어줍니다.
    *   `fs.writeFileSync(TODO_FILE, data, 'utf8')`: 변환된 JSON 문자열을 `todos.json` 파일에 씁니다. `writeFileSync`도 동기 방식입니다.
*   `process.on('exit', ...)`: 프로그램이 종료될 때 `saveTodos()` 함수가 자동으로 호출되도록 설정합니다. 이렇게 하면 프로그램이 어떤 방식으로든 종료될 때마다 변경된 할 일 목록이 파일에 저장됩니다.

### 3-3) 3단계: 명령줄 인수 처리하기

사용자가 `node todo.js add "할 일"`과 같이 명령줄에 입력한 내용을 JavaScript 코드에서 읽어와야 합니다. Node.js에서는 `process.argv`라는 특별한 배열을 통해 이 정보를 얻을 수 있습니다.

`todo.js` 파일의 맨 아래에 다음 코드를 추가합니다.

```javascript
// todo.js 파일 (이전 코드에 이어서)

// process.argv는 명령줄 인수를 담고 있는 배열입니다.
// 예: node todo.js add "점심 식사하기"
// process.argv는 ['node', '/path/to/todo.js', 'add', '점심 식사하기'] 와 같습니다.
const command = process.argv[2]; // 세 번째 요소가 우리가 원하는 명령어입니다. (add, list, complete, delete)
const arg = process.argv[3];     // 네 번째 요소가 명령어에 대한 인자입니다. (할 일 내용, 할 일 번호)

// 어떤 명령어가 입력되었는지에 따라 다른 함수를 호출합니다.
switch (command) {
  case 'add':
    addTodo(arg); // addTodo 함수는 아직 만들지 않았습니다.
    break;
  case 'list':
    listTodos(); // listTodos 함수는 아직 만들지 않았습니다.
    break;
  case 'complete':
    completeTodo(parseInt(arg)); // completeTodo 함수는 아직 만들지 않았습니다.
    break;
  case 'delete':
    deleteTodo(parseInt(arg)); // deleteTodo 함수는 아직 만들지 않았습니다.
    break;
  default:
    // 유효하지 않은 명령어가 입력되었을 때 안내 메시지를 출력합니다.
    console.log(`
      사용법:
      node todo.js add <할 일 내용>       - 새로운 할 일을 추가합니다.
      node todo.js list                  - 할 일 목록을 보여줍니다.
      node todo.js complete <할 일 번호> - 특정 할 일을 완료 상태로 변경합니다.
      node todo.js delete <할 일 번호>   - 특정 할 일을 목록에서 삭제합니다.
    `);
    break;
}
```

**코드 설명:**

*   `process.argv`: Node.js 프로그램이 실행될 때 전달되는 명령줄 인수들을 배열 형태로 가지고 있습니다.
    *   `process.argv[0]`은 항상 `node` 실행 파일의 경로입니다.
    *   `process.argv[1]`은 현재 실행 중인 JavaScript 파일(`todo.js`)의 경로입니다.
    *   `process.argv[2]`부터가 우리가 입력한 명령어(`add`, `list` 등)와 그에 대한 인자들입니다.
*   `command`와 `arg` 변수에 필요한 명령줄 인수를 할당합니다.
*   `switch (command)`: `command` 변수의 값에 따라 다른 `case` 블록의 코드를 실행합니다.
*   `parseInt(arg)`: `complete`나 `delete` 명령어의 인자는 숫자로 주어지므로, `parseInt()` 함수를 사용하여 문자열을 정수로 변환합니다.

### 3-4) 4단계: 할 일 추가 (`add`) 기능 구현

이제 `addTodo` 함수를 구현하여 새로운 할 일을 목록에 추가하고 저장하도록 합니다.

`todo.js` 파일의 `loadTodos()` 함수 아래, `process.on('exit', ...)` 위에 다음 함수를 추가합니다.

```javascript
// todo.js 파일 (이전 코드에 이어서)

// 새로운 할 일을 추가하는 함수
function addTodo(content) {
  if (!content) { // 할 일 내용이 비어있는지 확인합니다.
    console.log('할 일 내용을 입력해주세요.');
    return; // 함수를 여기서 종료합니다.
  }

  // 새로운 할 일 객체를 만듭니다.
  // id: 현재 할 일 목록의 마지막 id에 1을 더하거나, 목록이 비어있으면 1로 시작합니다.
  // content: 사용자가 입력한 할 일 내용입니다.
  // completed: 처음에는 완료되지 않은 상태이므로 false로 설정합니다.
  const newTodo = {
    id: todos.length > 0 ? Math.max(...todos.map(todo => todo.id)) + 1 : 1,
    content: content,
    completed: false
  };

  // 새로운 할 일 객체를 할 일 목록 배열에 추가합니다.
  todos.push(newTodo);
  console.log(`할 일 추가: "${content}" (ID: ${newTodo.id})`);
  saveTodos(); // 변경된 목록을 파일에 저장합니다。
}
```

**코드 설명:**

*   `if (!content)`: 사용자가 할 일 내용을 입력했는지 확인합니다. 내용이 없으면 안내 메시지를 출력하고 함수를 종료합니다.
*   `id: todos.length > 0 ? Math.max(...todos.map(todo => todo.id)) + 1 : 1`:
    *   이것은 **삼항 연산자**라고 불리는 간결한 `if-else` 문입니다.
    *   `todos.length > 0` (할 일 목록에 이미 할 일이 있다면)
        *   `Math.max(...todos.map(todo => todo.id)) + 1`: 기존 할 일들의 `id` 중에서 가장 큰 값에 1을 더하여 새로운 `id`를 만듭니다.
            *   `todos.map(todo => todo.id)`: `todos` 배열의 각 할 일 객체에서 `id`만 뽑아내어 새로운 `id` 배열을 만듭니다.
            *   `...`: **스프레드 연산자**입니다. `id` 배열의 요소들을 `Math.max()` 함수에 개별적인 인수로 전달합니다.
            *   `Math.max()`: 전달된 숫자들 중에서 가장 큰 값을 찾아줍니다.
    *   `: 1` (할 일 목록이 비어있다면)
        *   새로운 할 일의 `id`를 1로 설정합니다.
*   `todos.push(newTodo)`: 새로 만든 `newTodo` 객체를 `todos` 배열의 맨 뒤에 추가합니다.
*   `saveTodos()`: 할 일 목록이 변경되었으므로, `saveTodos()` 함수를 호출하여 변경된 내용을 `todos.json` 파일에 저장합니다.

**테스트:**

터미널에서 다음 명령어를 실행하여 할 일을 추가해봅니다.

```bash
node todo.js add "점심 식사하기"
node todo.js add "저녁 운동하기"
node todo.js add "책 읽기 30분"
```

각 명령어를 실행할 때마다 `할 일 목록을 저장했습니다.` 메시지가 출력되는지 확인하고, `todos.json` 파일을 열어보면 추가된 할 일들이 JSON 형태로 저장되어 있을 것입니다.

### 3-5) 5단계: 할 일 목록 보기 (`list`) 기능 구현

`listTodos` 함수를 구현하여 현재 할 일 목록을 보기 좋게 출력하도록 합니다.

`addTodo()` 함수 아래에 다음 함수를 추가합니다.

```javascript
// todo.js 파일 (이전 코드에 이어서)

// 할 일 목록을 출력하는 함수
function listTodos() {
  console.log('\n--- 할 일 목록 ---'); // 줄 바꿈 문자와 함께 제목 출력

  if (todos.length === 0) { // 할 일 목록이 비어있는지 확인합니다.
    console.log('할 일이 없습니다.');
  } else {
    // forEach 메서드를 사용하여 todos 배열의 각 할 일에 대해 반복합니다.
    todos.forEach(todo => {
      // 완료 여부에 따라 체크박스 문자열을 결정합니다.
      const status = todo.completed ? '[x]' : '[ ]';
      // 템플릿 리터럴을 사용하여 할 일 정보를 보기 좋게 출력합니다.
      console.log(`${todo.id}. ${status} ${todo.content}`);
    });
  }
  console.log('-------------------\n'); // 줄 바꿈 문자와 함께 구분선 출력
}
```

**코드 설명:**

*   `if (todos.length === 0)`: `todos` 배열의 `length` 속성을 사용하여 배열이 비어있는지 확인합니다.
*   `todos.forEach(todo => { ... })`: `forEach` 메서드는 배열의 각 요소에 대해 주어진 함수를 한 번씩 실행합니다. 여기서는 각 `todo` 객체를 인자로 받아서 처리합니다.
*   `const status = todo.completed ? '[x]' : '[ ]';`: **삼항 연산자**를 사용하여 `todo.completed` 값이 `true`이면 `[x]`를, `false`이면 `[ ]`를 `status` 변수에 할당합니다.
*   `console.log(`${todo.id}. ${status} ${todo.content}`);`: **템플릿 리터럴**을 사용하여 할 일의 `id`, `status`, `content`를 조합하여 보기 좋은 형태로 출력합니다.

**테스트:**

터미널에서 다음 명령어를 실행하여 할 일 목록을 확인해봅니다.

```bash
node todo.js list
```

이전에 추가했던 할 일들이 목록으로 출력되는지 확인합니다.

### 3-6) 6단계: 할 일 완료 (`complete`) 기능 구현

`completeTodo` 함수를 구현하여 특정 할 일을 완료 상태로 변경하도록 합니다.

`listTodos()` 함수 아래에 다음 함수를 추가합니다.

```javascript
// todo.js 파일 (이전 코드에 이어서)

// 특정 할 일을 완료 상태로 변경하는 함수
function completeTodo(id) {
  // find 메서드를 사용하여 주어진 id와 일치하는 할 일 객체를 찾습니다.
  const todoToComplete = todos.find(todo => todo.id === id);

  if (todoToComplete) { // 할 일을 찾았다면
    todoToComplete.completed = true; // completed 속성을 true로 변경합니다.
    console.log(`할 일 완료: "${todoToComplete.content}" (ID: ${id})`);
    saveTodos(); // 변경된 목록을 파일에 저장합니다。
  } else { // 할 일을 찾지 못했다면
    console.log(`ID ${id}번 할 일을 찾을 수 없습니다.`);
  }
}
```

**코드 설명:**

*   `todos.find(todo => todo.id === id)`: `find` 메서드는 배열의 요소들을 순회하면서 주어진 조건(`todo.id === id`)을 만족하는 첫 번째 요소를 반환합니다. 조건에 맞는 요소를 찾지 못하면 `undefined`를 반환합니다.
*   `if (todoToComplete)`: `find` 메서드가 `undefined`가 아닌 유효한 할 일 객체를 반환했는지 확인합니다.
*   `todoToComplete.completed = true;`: 찾은 할 일 객체의 `completed` 속성을 `true`로 변경합니다.
*   `saveTodos()`: 변경된 목록을 파일에 저장합니다.

**테스트:**

터미널에서 다음 명령어를 실행하여 할 일을 완료 처리해봅니다.

```bash
node todo.js complete 1
node todo.js list
```

`node todo.js list`를 실행했을 때 1번 할 일 옆에 `[x]`가 표시되는지 확인합니다.

### 3-7) 7단계: 할 일 삭제 (`delete`) 기능 구현

`deleteTodo` 함수를 구현하여 특정 할 일을 목록에서 삭제하도록 합니다.

`completeTodo()` 함수 아래에 다음 함수를 추가합니다.

```javascript
// todo.js 파일 (이전 코드에 이어서)

// 특정 할 일을 목록에서 삭제하는 함수
function deleteTodo(id) {
  // filter 메서드를 사용하여 주어진 id와 일치하지 않는 할 일들만 모아 새로운 배열을 만듭니다.
  // 이렇게 하면 특정 id를 가진 할 일은 새 배열에서 제외되어 삭제되는 효과를 냅니다.
  const initialLength = todos.length; // 삭제 전 배열의 길이를 저장합니다.
  todos = todos.filter(todo => todo.id !== id);

  if (todos.length < initialLength) { // 배열의 길이가 줄어들었다면 삭제가 성공한 것입니다.
    console.log(`할 일 삭제: ID ${id}번 할 일이 삭제되었습니다.`);
    saveTodos(); // 변경된 목록을 파일에 저장합니다.
  } else { // 배열의 길이가 그대로라면 해당 ID의 할 일을 찾지 못한 것입니다.
    console.log(`ID ${id}번 할 일을 찾을 수 없습니다.`);
  }
}
```

**코드 설명:**

*   `todos = todos.filter(todo => todo.id !== id);`: `filter` 메서드는 배열의 요소들을 순회하면서 주어진 조건(`todo.id !== id`)을 만족하는 요소들만 모아 **새로운 배열**을 반환합니다. 이 새로운 배열을 다시 `todos` 변수에 할당함으로써, 특정 `id`를 가진 할 일만 제외하고 나머지 할 일들로 `todos` 배열을 업데이트합니다.
*   `initialLength`와 `todos.length`를 비교하여 삭제가 실제로 이루어졌는지 확인하고 메시지를 출력합니다.
*   `saveTodos()`: 변경된 목록을 파일에 저장합니다.

**테스트:**

터미널에서 다음 명령어를 실행하여 할 일을 삭제해봅니다.

```bash
node todo.js delete 2
node todo.js list
```

`node todo.js list`를 실행했을 때 2번 할 일이 목록에서 사라졌는지 확인합니다.

## 4) 최종 테스트

이제 모든 기능이 구현되었습니다. 다양한 명령어를 조합하여 할 일 관리자 프로그램이 잘 작동하는지 확인해봅니다.

```bash
node todo.js add "새로운 할 일 1"
node todo.js add "새로운 할 일 2"
node todo.js list
node todo.js complete 1
node todo.js list
node todo.js delete 2
node todo.js list
node todo.js add "또 다른 할 일"
node todo.js list
node todo.js complete 3 # 새로 추가된 할 일의 ID가 3인지 확인
node todo.js list
```

`todos.json` 파일을 직접 열어보면서 데이터가 올바르게 저장되고 변경되는지도 확인해보세요.

## 5) 마무리

이 실습을 통해 여러분은 Node.js 환경에서 파일 시스템을 다루고, 명령줄 인수를 처리하며, 배열과 객체 메서드를 활용하여 데이터를 관리하는 방법을 익혔습니다. 이는 실제 애플리케이션을 개발하는 데 매우 중요한 기초가 됩니다.

---

- [목차로 돌아가기](../README.md)
- [이전 강의로 이동](Lab1-Simple-Calculator.md)
- [다음 강의로 이동](Lab3-Grade-Processor.md)