# Lab2. 성적 처리 프로그램

## 목표

학생들의 성적 데이터를 분석하여 평균 점수, 최고/최저 점수, 합격자 목록 등을 계산하고 출력하는 Node.js 프로그램을 만듭니다. 이 실습을 통해 Node.js의 파일 시스템(`fs` 모듈)을 사용하여 JSON 데이터를 읽고, 배열의 다양한 메서드(`map`, `filter`, `reduce`, `forEach`)를 활용하여 데이터를 효율적으로 처리하는 방법을 익히게 됩니다. 이는 실제 데이터를 다루는 애플리케이션 개발에 필수적인 기술입니다.

## 예상 소요 시간

45분

---

## 튜토리얼 시작하기

### 1단계: 프로젝트 초기 설정 및 데이터 파일 준비

성적 처리 프로그램을 만들기 위해 필요한 파일들을 먼저 준비합니다. 성적 데이터는 JSON 형식으로 파일에 저장되어 있을 것입니다.

1.  `day2` 폴더 안에 `grade_processor.js`라는 이름의 새 파일을 만듭니다. 이 파일에 우리가 작성할 JavaScript 코드가 들어갑니다.
2.  `day2` 폴더 안에 `grades.json`이라는 이름의 새 파일을 만듭니다. 이 파일은 학생들의 이름과 점수를 담고 있는 데이터베이스 역할을 할 것입니다. 다음 JSON 데이터를 복사하여 `grades.json` 파일에 붙여넣습니다.

    ```json
    // day2/grades.json 파일 내용
    [
      { "name": "김철수", "score": 85 },
      { "name": "이영희", "score": 92 },
      { "name": "박민수", "score": 78 },
      { "name": "최수진", "score": 60 },
      { "name": "정하준", "score": 95 },
      { "name": "강지민", "score": 70 },
      { "name": "윤서연", "score": 88 }
    ]
    ```

    이 JSON 데이터는 각 학생의 이름(`name`)과 점수(`score`)를 객체 형태로 표현하고, 이 객체들이 배열 안에 담겨 있는 구조입니다. JavaScript에서 이 데이터를 불러와서 처리할 것입니다.

### 2단계: 성적 데이터 불러오기

프로그램이 시작될 때 `grades.json` 파일에 저장된 학생들의 성적 데이터를 불러와야 합니다. Node.js의 `fs` (File System) 모듈을 사용하여 파일을 읽고, `JSON.parse()`를 사용하여 JavaScript에서 다룰 수 있는 형태로 변환합니다.

`grade_processor.js` 파일을 열고 다음 코드를 작성합니다.

```javascript
// grade_processor.js 파일

// Node.js의 파일 시스템 모듈을 불러옵니다.
// 이 모듈은 파일을 읽거나 쓰는 등의 파일 관련 작업을 수행할 때 사용됩니다.
const fs = require('fs');

// 성적 데이터가 저장된 JSON 파일의 경로를 상수로 정의합니다.
// 이렇게 상수로 정의해두면 파일 경로가 변경될 때 코드 전체를 수정할 필요 없이 이 부분만 바꾸면 됩니다.
const GRADES_FILE = './grades.json';

// 불러온 학생 데이터를 저장할 배열 변수를 선언합니다.
// 초기에는 빈 배열로 시작하며, 파일에서 데이터를 성공적으로 불러오면 이 배열에 채워집니다.
let students = [];

// 파일에서 성적 데이터를 불러오는 역할을 하는 함수를 정의합니다.
function loadGrades() {
  try {
    // GRADES_FILE 경로의 파일을 동기적으로 읽어옵니다.
    // `readFileSync`는 파일 읽기 작업이 완료될 때까지 다음 코드를 실행하지 않고 기다리는 방식입니다.
    // 두 번째 인자 'utf8'은 파일의 내용을 텍스트(문자열)로 해석하라는 인코딩 방식입니다.
    const data = fs.readFileSync(GRADES_FILE, 'utf8');

    // 파일에서 읽어온 `data`는 JSON 형식의 문자열입니다.
    // `JSON.parse()` 메서드를 사용하여 이 JSON 문자열을 JavaScript에서 사용할 수 있는 객체(여기서는 배열)로 변환합니다.
    students = JSON.parse(data);
    console.log('성적 데이터를 성공적으로 불러왔습니다.\n');
  } catch (error) {
    // `try` 블록 안에서 오류(예: 파일이 존재하지 않거나, JSON 형식이 잘못된 경우)가 발생하면
    // `catch` 블록의 코드가 실행됩니다.
    console.error('성적 데이터를 불러오는 중 오류 발생:', error.message);
    console.log('빈 학생 목록으로 시작합니다. grades.json 파일이 올바른지 확인해주세요.');
    students = []; // 오류 발생 시 `students` 배열을 빈 배열로 초기화하여 프로그램이 계속 실행되도록 합니다。
  }
}

// 프로그램이 시작될 때 `loadGrades` 함수를 호출하여 성적 데이터를 메모리로 불러옵니다.
loadGrades();

// --- 여기에 성적 처리 기능들이 추가될 예정입니다. ---
```

**코드 상세 설명:**

*   **`const fs = require('fs');`**: Node.js에서 파일 시스템 관련 작업을 수행하기 위해 내장 모듈인 `fs`를 불러오는 코드입니다. `require()` 함수는 Node.js에서 다른 모듈을 가져올 때 사용됩니다.
*   **`const GRADES_FILE = './grades.json';`**: 성적 데이터가 저장된 파일의 이름을 `GRADES_FILE`이라는 상수에 할당했습니다. 이렇게 상수로 정의하면 코드의 가독성이 높아지고, 나중에 파일 이름이 변경될 경우 이 한 줄만 수정하면 되므로 유지보수가 편리합니다.
*   **`let students = [];`**: `grades.json` 파일에서 불러온 학생 데이터를 저장할 빈 배열 `students`를 선언했습니다. `let` 키워드를 사용한 이유는, 파일에서 데이터를 불러와서 이 배열에 다시 할당할 수 있기 때문입니다.
*   **`function loadGrades() { ... }`**: `loadGrades`라는 이름의 함수를 정의했습니다. 이 함수는 `grades.json` 파일의 내용을 읽고, 그 내용을 `students` 배열에 저장하는 역할을 합니다.
    *   **`try { ... } catch (error) { ... }`**: 파일 읽기나 JSON 파싱 과정에서 오류가 발생할 수 있습니다. 예를 들어, `grades.json` 파일이 존재하지 않거나, 파일 내용이 올바른 JSON 형식이 아닐 수 있습니다. `try...catch` 문은 이러한 예상치 못한 오류가 발생했을 때 프로그램이 갑자기 멈추는 것을 방지하고, 오류를 처리할 수 있도록 해줍니다.
    *   **`const data = fs.readFileSync(GRADES_FILE, 'utf8');`**: `fs` 모듈의 `readFileSync` 함수를 사용하여 `GRADES_FILE`에 지정된 파일을 읽어옵니다. `readFileSync`는 파일 읽기 작업이 완료될 때까지 다음 코드를 실행하지 않고 기다리는 **동기(Synchronous)** 방식입니다. 두 번째 인자인 `'utf8'`은 파일의 내용을 텍스트로 읽을 때 사용할 인코딩 방식을 지정합니다. 한글이 포함된 파일을 올바르게 읽기 위해 중요합니다.
    *   **`students = JSON.parse(data);`**: `fs.readFileSync`로 읽어온 `data`는 단순한 문자열 형태입니다. 이 문자열은 JSON 형식으로 되어 있으므로, `JSON.parse()` 메서드를 사용하여 JavaScript에서 실제로 사용할 수 있는 배열이나 객체 형태로 변환합니다. 이렇게 변환된 데이터가 `students` 배열에 할당됩니다.
    *   **`console.log('성적 데이터를 성공적으로 불러왔습니다.\n');`**: 파일 읽기와 파싱이 성공적으로 완료되면 사용자에게 알림 메시지를 출력합니다. `\n`은 줄 바꿈을 의미합니다.
    *   **`console.error('성적 데이터를 불러오는 중 오류 발생:', error.message);`**: `catch` 블록에서는 발생한 오류에 대한 정보를 `console.error`를 통해 출력합니다. `error.message`는 오류의 구체적인 내용을 담고 있습니다.
    *   **`students = [];`**: 오류가 발생하여 데이터를 불러오지 못했을 경우, `students` 배열을 빈 배열로 초기화합니다. 이렇게 하면 프로그램이 오류로 인해 중단되지 않고, 빈 목록으로라도 계속 실행될 수 있습니다.
*   **`loadGrades();`**: `grade_processor.js` 파일이 Node.js로 실행될 때, 가장 먼저 `loadGrades` 함수를 호출하여 `grades.json` 파일의 데이터를 메모리로 불러오도록 합니다.

**테스트:**

터미널(명령 프롬프트 또는 PowerShell)을 열고 `day2` 폴더로 이동한 다음, 다음 명령어를 실행합니다.

```bash
node grade_processor.js
```

명령어를 실행했을 때 `성적 데이터를 성공적으로 불러왔습니다.`라는 메시지가 출력되는지 확인합니다. 만약 `grades.json` 파일이 없거나 내용이 잘못되었다면, 오류 메시지가 출력될 것입니다. 이 경우 `grades.json` 파일이 올바르게 생성되었는지 다시 확인해주세요.

### 3단계: 전체 학생 목록 및 점수 출력

이제 `grades.json` 파일에서 성공적으로 불러온 학생들의 이름과 점수를 화면에 보기 좋게 출력하는 기능을 추가합니다. 이 기능은 `forEach` 배열 메서드를 사용하여 `students` 배열의 모든 요소를 순회하며 정보를 출력합니다.

`grade_processor.js` 파일의 `loadGrades()` 함수 호출 아래에 다음 코드를 추가합니다.

```javascript
// grade_processor.js 파일 (이전 코드에 이어서)

// 모든 학생의 이름과 점수를 화면에 출력하는 함수를 정의합니다.
function displayAllStudents() {
  console.log('--- 전체 학생 목록 ---');

  // 만약 students 배열이 비어있다면 (즉, 불러온 학생 데이터가 없다면)
  if (students.length === 0) {
    console.log('등록된 학생이 없습니다.');
    return; // 함수를 여기서 종료하여 더 이상 진행하지 않습니다.
  }

  // students 배열의 각 요소(학생 객체)에 대해 한 번씩 주어진 함수를 실행합니다。
  // `student`는 현재 반복 중인 학생 객체를 나타냅니다.
  students.forEach(student => {
    // 템플릿 리터럴(백틱 ` `)을 사용하여 출력할 문자열을 만듭니다.
    // `${student.name}`은 학생 객체의 `name` 속성 값을, `${student.score}`는 `score` 속성 값을 문자열 안에 삽입합니다.
    console.log(`${student.name}: ${student.score}점`);
  });
  console.log('----------------------\n'); // 목록의 끝을 나타내는 구분선과 줄 바꿈을 출력합니다。
}

// `displayAllStudents` 함수를 호출하여 모든 학생의 목록을 화면에 표시합니다.
displayAllStudents();
```

**코드 상세 설명:**

*   **`function displayAllStudents() { ... }`**: `displayAllStudents`라는 이름의 함수를 정의했습니다. 이 함수는 현재 `students` 배열에 저장된 모든 학생의 정보를 출력하는 역할을 합니다.
*   **`if (students.length === 0) { ... }`**: `students` 배열의 `length` 속성을 사용하여 배열에 요소가 하나도 없는지(즉, 학생 데이터가 없는지) 확인합니다. 만약 비어있다면, `등록된 학생이 없습니다.`라는 메시지를 출력하고 `return` 키워드를 사용하여 함수를 즉시 종료합니다. 이렇게 하면 불필요한 반복을 피하고 깔끔하게 처리할 수 있습니다.
*   **`students.forEach(student => { ... });`**: `forEach`는 배열이 가지고 있는 매우 유용한 메서드 중 하나입니다. 이 메서드는 배열의 각 요소에 대해 한 번씩 주어진 함수(여기서는 화살표 함수 `student => { ... }`)를 실행합니다. `student`는 현재 반복되고 있는 배열의 요소, 즉 학생 객체를 나타냅니다.
*   **`console.log(`${student.name}: ${student.score}점`);`**: `forEach` 내부에서 실행되는 코드입니다. **템플릿 리터럴**을 사용하여 학생의 `name` 속성과 `score` 속성 값을 문자열 안에 삽입하여 출력합니다. 예를 들어, 첫 번째 학생인 김철수의 경우 `김철수: 85점`과 같이 출력될 것입니다.

**테스트:**

`node grade_processor.js`를 다시 실행하여 모든 학생의 이름과 점수가 다음과 같이 출력되는지 확인합니다.

```
성적 데이터를 성공적으로 불러왔습니다.

--- 전체 학생 목록 ---
김철수: 85점
이영희: 92점
박민수: 78점
최수진: 60점
정하준: 95점
강지민: 70점
윤서연: 88점
----------------------
```

### 4단계: 평균 점수 계산 및 출력

이제 `students` 배열에 있는 모든 학생의 점수를 합산하여 평균 점수를 계산하고 출력하는 기능을 추가합니다. 이 작업에는 `reduce` 배열 메서드가 매우 유용하게 사용됩니다.

`grade_processor.js` 파일의 `displayAllStudents()` 함수 호출 아래에 다음 코드를 추가합니다.

```javascript
// grade_processor.js 파일 (이전 코드에 이어서)

// 모든 학생의 평균 점수를 계산하고 출력하는 함수를 정의합니다.
function calculateAverageScore() {
  // 만약 students 배열이 비어있다면 평균을 계산할 수 없으므로 메시지를 출력하고 함수를 종료합니다.
  if (students.length === 0) {
    console.log('평균을 계산할 학생이 없습니다.\n');
    return;
  }

  // `reduce` 메서드를 사용하여 모든 학생의 점수를 합산합니다.
  // `reduce`는 배열의 각 요소를 순회하며 하나의 결과 값을 만들어냅니다.
  // 첫 번째 인자는 콜백 함수이고, 두 번째 인자는 `accumulator`의 초기값입니다.
  // 콜백 함수:
  //   `accumulator`: 이전 콜백 함수의 반환 값입니다. 여기서는 현재까지의 점수 합계를 저장합니다.
  //   `student`: 현재 처리 중인 배열의 요소, 즉 학생 객체입니다.
  const totalScore = students.reduce((accumulator, student) => {
    return accumulator + student.score; // 현재 학생의 점수를 `accumulator`에 더하여 반환합니다.
  }, 0); // `accumulator`의 초기값을 0으로 설정합니다. (합계가 0부터 시작하도록)

  // 총점을 학생 수로 나누어 평균을 계산합니다.
  const average = totalScore / students.length;

  // 템플릿 리터럴을 사용하여 평균 점수를 출력합니다.
  // `average.toFixed(2)`는 평균 점수를 소수점 둘째 자리까지 반올림하여 문자열로 변환합니다.
  console.log(`--- 평균 점수 ---
전체 학생 평균: ${average.toFixed(2)}점
-------------------\n`);
}

// `calculateAverageScore` 함수를 호출하여 평균 점수를 계산하고 화면에 표시합니다.
calculateAverageScore();
```

**코드 상세 설명:**

*   **`function calculateAverageScore() { ... }`**: `calculateAverageScore`라는 이름의 함수를 정의했습니다. 이 함수는 학생들의 평균 점수를 계산하고 출력하는 역할을 합니다.
*   **`if (students.length === 0) { ... }`**: 마찬가지로 학생 데이터가 없는 경우를 처리합니다. 평균을 계산할 수 없으므로 적절한 메시지를 출력하고 함수를 종료합니다.
*   **`const totalScore = students.reduce((accumulator, student) => { ... }, 0);`**: `reduce`는 배열의 모든 요소를 순회하며 하나의 단일 값을 만들어낼 때 사용되는 강력한 배열 메서드입니다. 여기서는 모든 학생의 점수를 합산하는 데 사용됩니다.
    *   첫 번째 인자는 **콜백 함수**입니다. 이 콜백 함수는 두 개의 중요한 매개변수를 받습니다: `accumulator`와 `student`.
        *   `accumulator`: `reduce` 메서드가 반복될 때마다 이전 콜백 함수의 반환 값을 저장하는 변수입니다. 여기서는 현재까지의 점수 합계를 누적합니다.
        *   `student`: 현재 `students` 배열에서 처리 중인 학생 객체입니다.
    *   두 번째 인자는 `accumulator`의 **초기값**입니다. 여기서는 `0`으로 설정하여 점수 합계가 0부터 시작하도록 했습니다.
    *   콜백 함수 내부에서는 `accumulator`에 현재 학생의 `score`를 더한 값을 반환합니다. 이 반환 값이 다음 반복의 `accumulator`가 됩니다.
*   **`const average = totalScore / students.length;`**: `reduce`를 통해 얻은 `totalScore`를 전체 학생 수(`students.length`)로 나누어 평균을 계산합니다.
*   **`average.toFixed(2)`**: `toFixed()`는 숫자를 지정된 소수점 자릿수까지 반올림하여 문자열로 반환하는 숫자 메서드입니다. 여기서는 평균 점수를 소수점 둘째 자리까지 보여주기 위해 사용했습니다.

**테스트:**

`node grade_processor.js`를 다시 실행하여 평균 점수가 다음과 같이 출력되는지 확인합니다.

```
성적 데이터를 성공적으로 불러왔습니다.

--- 전체 학생 목록 ---
김철수: 85점
이영희: 92점
박민수: 78점
최수진: 60점
정하준: 95점
강지민: 70점
윤서연: 88점
----------------------

--- 평균 점수 ---
전체 학생 평균: 81.14점
-------------------
```

### 5단계: 최고/최저 점수 학생 출력

이번에는 `reduce()` 메서드를 다시 활용하여 학생들 중에서 가장 높은 점수를 받은 학생과 가장 낮은 점수를 받은 학생을 찾아 출력하는 기능을 추가합니다. `reduce`는 단순히 숫자를 합산하는 것 외에도, 배열의 요소들을 비교하여 특정 기준에 맞는 하나의 요소를 찾아낼 때도 매우 강력합니다.

`grade_processor.js` 파일의 `calculateAverageScore()` 함수 호출 아래에 다음 코드를 추가합니다.

```javascript
// grade_processor.js 파일 (이전 코드에 이어서)

// 최고 점수와 최저 점수를 받은 학생을 찾아 출력하는 함수를 정의합니다.
function findMinMaxScoreStudents() {
  // 만약 students 배열이 비어있다면 최고/최저 점수를 찾을 수 없으므로 메시지를 출력하고 함수를 종료합니다.
  if (students.length === 0) {
    console.log('최고/최저 점수를 찾을 학생이 없습니다.\n');
    return;
  }

  // `reduce`를 사용하여 최고 점수 학생을 찾습니다.
  // `maxStudent`: 현재까지 발견된 최고 점수 학생 객체 (accumulator)
  // `currentStudent`: 현재 처리 중인 학생 객체
  // 초기값으로 `students[0]` (배열의 첫 번째 학생)을 설정하여 첫 학생부터 비교를 시작합니다.
  const topStudent = students.reduce((maxStudent, currentStudent) => {
    // 현재 학생의 점수(`currentStudent.score`)가 현재까지의 최고 점수 학생의 점수(`maxStudent.score`)보다 높다면,
    // `currentStudent`를 새로운 `maxStudent`로 반환합니다.
    // 그렇지 않다면, 기존의 `maxStudent`를 그대로 반환합니다.
    return currentStudent.score > maxStudent.score ? currentStudent : maxStudent;
  }, students[0]);

  // `reduce`를 사용하여 최저 점수 학생을 찾습니다.
  // `minStudent`: 현재까지 발견된 최저 점수 학생 객체 (accumulator)
  // `currentStudent`: 현재 처리 중인 학생 객체
  // 초기값으로 `students[0]` (배열의 첫 번째 학생)을 설정합니다.
  const lowStudent = students.reduce((minStudent, currentStudent) => {
    // 현재 학생의 점수(`currentStudent.score`)가 현재까지의 최저 점수 학생의 점수(`minStudent.score`)보다 낮다면,
    // `currentStudent`를 새로운 `minStudent`로 반환합니다.
    // 그렇지 않다면, 기존의 `minStudent`를 그대로 반환합니다.
    return currentStudent.score < minStudent.score ? currentStudent : minStudent;
  }, students[0]);

  // 템플릿 리터럴을 사용하여 최고/최저 점수 학생의 정보를 출력합니다.
  console.log(`--- 최고/최저 점수 학생 ---
최고 점수: ${topStudent.name} (${topStudent.score}점)
최저 점수: ${lowStudent.name} (${lowStudent.score}점)
---------------------------\n`);
}

// `findMinMaxScoreStudents` 함수를 호출하여 최고/최저 점수 학생을 찾아 화면에 표시합니다.
findMinMaxScoreStudents();
```

**코드 상세 설명:**

*   **`function findMinMaxScoreStudents() { ... }`**: `findMinMaxScoreStudents`라는 함수를 정의했습니다. 이 함수는 학생 목록에서 최고 점수와 최저 점수를 받은 학생을 찾아 출력하는 역할을 합니다.
*   **`if (students.length === 0) { ... }`**: 학생 데이터가 없는 경우를 처리합니다.
*   **`const topStudent = students.reduce((maxStudent, currentStudent) => { ... }, students[0]);`**: 최고 점수 학생을 찾기 위해 `reduce` 메서드를 사용했습니다.
    *   `maxStudent`: `reduce` 콜백 함수의 `accumulator`입니다. 이 변수에는 현재까지 가장 높은 점수를 가진 학생 객체가 저장됩니다.
    *   `currentStudent`: 현재 `students` 배열에서 처리 중인 학생 객체입니다.
    *   `students[0]`을 `accumulator`의 초기값으로 설정했습니다. 이렇게 하면 첫 번째 학생부터 비교를 시작할 수 있습니다.
    *   **`return currentStudent.score > maxStudent.score ? currentStudent : maxStudent;`**: 이 부분은 **삼항 연산자**입니다. `currentStudent`의 점수가 `maxStudent`의 점수보다 높으면 `currentStudent`를 반환하고, 그렇지 않으면 `maxStudent`를 반환합니다. 이 반환 값이 다음 반복의 `maxStudent`가 됩니다.
*   **`const lowStudent = students.reduce((minStudent, currentStudent) => { ... }, students[0]);`**: 최저 점수 학생을 찾는 로직도 최고 점수 학생을 찾는 로직과 동일하게 `reduce`를 사용합니다. 단지 비교 연산자(`>`)가 `<`로 바뀌어 더 낮은 점수를 가진 학생을 찾아냅니다.

**테스트:**

`node grade_processor.js`를 다시 실행하여 최고/최저 점수 학생이 다음과 같이 올바르게 출력되는지 확인합니다.

```
성적 데이터를 성공적으로 불러왔습니다.

--- 전체 학생 목록 ---
김철수: 85점
이영희: 92점
박민수: 78점
최수진: 60점
정하준: 95점
강지민: 70점
윤서연: 88점
----------------------

--- 평균 점수 ---
전체 학생 평균: 81.14점
-------------------

--- 최고/최저 점수 학생 ---
최고 점수: 정하준 (95점)
최저 점수: 최수진 (60점)
---------------------------
```

### 6단계: 합격자 목록 출력

마지막으로, 특정 기준(여기서는 70점 이상)을 만족하는 학생들만 골라내어 합격자 목록을 출력하는 기능을 추가합니다. 이 작업에는 `filter` 배열 메서드가 매우 적합합니다.

`grade_processor.js` 파일의 `findMinMaxScoreStudents()` 함수 호출 아래에 다음 코드를 추가합니다.

```javascript
// grade_processor.js 파일 (이전 코드에 이어서)

// 70점 이상인 합격자 학생 목록을 출력하는 함수를 정의합니다.
function displayPassStudents() {
  console.log('--- 합격자 목록 (70점 이상) ---');

  // 만약 students 배열이 비어있다면 합격자를 찾을 수 없으므로 메시지를 출력하고 함수를 종료합니다.
  if (students.length === 0) {
    console.log('등록된 학생이 없습니다.');
    return;
  }

  // `filter` 메서드를 사용하여 students 배열에서 특정 조건을 만족하는 학생들만 골라냅니다.
  // `filter`는 콜백 함수가 `true`를 반환하는 요소들로 구성된 새로운 배열을 반환합니다.
  // `student`는 현재 처리 중인 학생 객체입니다.
  const passStudents = students.filter(student => student.score >= 70);

  // 합격자 목록이 비어있는지 확인합니다.
  if (passStudents.length === 0) {
    console.log('합격자가 없습니다.');
  } else {
    // `forEach` 메서드를 사용하여 필터링된 `passStudents` 배열의 각 합격자에 대해 반복합니다.
    passStudents.forEach(student => {
      console.log(`${student.name}: ${student.score}점`);
    });
  }
  console.log('----------------------------\n');
}

// `displayPassStudents` 함수를 호출하여 합격자 목록을 화면에 표시합니다.
displayPassStudents();
```

**코드 상세 설명:**

*   **`function displayPassStudents() { ... }`**: `displayPassStudents`라는 함수를 정의했습니다. 이 함수는 70점 이상인 학생들만 필터링하여 출력하는 역할을 합니다.
*   **`if (students.length === 0) { ... }`**: 학생 데이터가 없는 경우를 처리합니다.
*   **`const passStudents = students.filter(student => student.score >= 70);`**: `filter`는 배열의 각 요소를 순회하면서 주어진 조건(콜백 함수가 `true`를 반환하는지)을 만족하는 요소들만 모아 **새로운 배열**을 만들어 반환하는 메서드입니다. 여기서는 `student.score >= 70`이라는 조건을 사용하여 점수가 70점 이상인 학생들만 `passStudents` 배열에 포함시킵니다.
*   **`if (passStudents.length === 0) { ... }`**: `filter` 결과, 합격자가 한 명도 없을 경우를 처리합니다.
*   **`passStudents.forEach(student => { ... });`**: `filter`를 통해 얻은 `passStudents` 배열의 각 합격자에 대해 `forEach`를 사용하여 이름과 점수를 출력합니다.

**테스트:**

`node grade_processor.js`를 다시 실행하여 합격자 목록이 다음과 같이 올바르게 출력되는지 확인합니다.

```
성적 데이터를 성공적으로 불러왔습니다.

--- 전체 학생 목록 ---
김철수: 85점
이영희: 92점
박민수: 78점
정하준: 95점
강지민: 70점
윤서연: 88점
----------------------

--- 평균 점수 ---
전체 학생 평균: 81.14점
-------------------

--- 최고/최저 점수 학생 ---
최고 점수: 정하준 (95점)
최저 점수: 최수진 (60점)
---------------------------

--- 합격자 목록 (70점 이상) ---
김철수: 85점
이영희: 92점
박민수: 78점
정하준: 95점
강지민: 70점
윤서연: 88점
----------------------------
```

### 7단계: 최종 확인

이제 모든 기능이 구현되었습니다. 터미널에서 `node grade_processor.js`를 실행하여 모든 정보가 올바르게 출력되는지 확인합니다. 각 섹션의 제목과 내용이 명확하게 구분되어 있는지, 그리고 계산된 값들이 정확한지 다시 한번 확인해보세요.

## 마무리

이 실습을 통해 여러분은 Node.js 환경에서 JSON 파일을 읽고, 배열의 `forEach`, `reduce`, `filter`와 같은 강력한 메서드들을 활용하여 데이터를 분석하고 처리하는 방법을 익혔습니다. 이는 실제 데이터를 다루는 다양한 애플리케이션 개발에 필수적인 기술입니다. 특히 `reduce`와 `filter`는 데이터를 가공하고 원하는 정보를 추출하는 데 매우 유용하게 사용되므로, 이 개념들을 잘 이해하고 활용할 수 있도록 연습하는 것이 중요합니다.

---

- [목차로 돌아가기](../README.md)
- [이전 강의로 이동](Lab1-Todo-List.md)
- [다음 강의로 이동](Lab3-Simple-Web-Server.md)
