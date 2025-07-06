# Lab 3. Input Form 컴포넌트 만들기

이전 두 실습을 통해 숫자와 불리언 타입의 State를 다루어 보았습니다. 이번 실습에서는 사용자의 텍스트 입력을 처리하는 **입력 폼(Input Form)** 컴포넌트를 만들어 보겠습니다. React에서 사용자의 입력을 다루는 방식은 매우 중요하며, 이를 **제어 컴포넌트(Controlled Component)** 패턴이라고 부릅니다.

이 실습의 목표는 사용자가 입력창에 텍스트를 입력하면, 그 내용이 실시간으로 화면의 다른 부분에 반영되는 컴포넌트를 만드는 것입니다.

이 실습을 통해 여러분은 다음을 할 수 있게 됩니다:

*   `<input>` 태그의 값을 React State로 관리하는 방법을 배웁니다.
*   사용자 입력에 따라 State를 업데이트하는 `onChange` 이벤트를 처리합니다.
*   React의 핵심 디자인 패턴 중 하나인 **제어 컴포넌트**의 개념을 이해합니다.

---

## Step-by-Step 가이드

### Step 1: `InputForm` 컴포넌트 파일 생성

1.  `src/components` 폴더 안에 `InputForm.jsx` 라는 새 파일을 생성합니다.

### Step 2: 기본 컴포넌트 코드 작성

`InputForm.jsx` 파일에 기본적인 함수형 컴포넌트 코드를 작성합니다. 사용자가 텍스트를 입력할 `<input>` 태그와, 입력된 내용을 보여줄 `<p>` 태그를 포함합니다.

```jsx
// src/components/InputForm.jsx

function InputForm() {
  return (
    <div>
      <h2>간단한 입력 폼</h2>
      <input type="text" />
      <p>입력된 텍스트: </p>
    </div>
  );
}

export default InputForm;
```

### Step 3: `useState`로 텍스트 State 만들기

사용자가 입력하는 텍스트를 저장할 State가 필요합니다. `value`라는 이름의 State 변수를 만들고, 아무것도 입력되지 않은 상태를 표현하기 위해 초기값을 빈 문자열(`''`)로 설정합니다.

```jsx
// src/components/InputForm.jsx

import { useState } from 'react'; // 1. useState를 import 합니다.

function InputForm() {
  // 2. value라는 state 변수를 만들고, 초기값을 빈 문자열로 설정합니다.
  const [value, setValue] = useState('');

  return (
    <div>
      <h2>간단한 입력 폼</h2>
      <input type="text" />
      {/* 3. p 태그에 value state를 표시합니다. */}
      <p>입력된 텍스트: {value}</p>
    </div>
  );
}

export default InputForm;
```

### Step 4: `onChange` 이벤트 핸들러 만들기

사용자가 `<input>`에 무언가를 입력할 때마다 `value` State를 업데이트해야 합니다. 입력 내용이 바뀔 때마다 발생하는 `onChange` 이벤트를 처리할 함수 `handleChange`를 만듭니다.

> [!NOTE]
> **이벤트 객체 (Event Object)**
> `onChange`와 같은 이벤트 핸들러 함수는 **이벤트 객체**를 인자로 받습니다. 이 객체에는 이벤트에 대한 정보가 담겨 있으며, `event.target.value`를 통해 사용자가 입력한 최신 값에 접근할 수 있습니다.

```jsx
// src/components/InputForm.jsx

import { useState } from 'react';

function InputForm() {
  const [value, setValue] = useState('');

  // input의 내용이 변경될 때마다 호출될 함수
  const handleChange = (event) => {
    // event.target.value에 담긴 최신 입력 값을 사용하여 state를 업데이트합니다.
    setValue(event.target.value);
  };

  return (
    <div>
      <h2>간단한 입력 폼</h2>
      <input type="text" />
      <p>입력된 텍스트: {value}</p>
    </div>
  );
}

export default InputForm;
```

### Step 5: `input` 태그에 `value`와 `onChange` 연결하기

이제 `<input>` 태그가 우리가 만든 State와 상호작용하도록 연결해야 합니다. 이것이 바로 **제어 컴포넌트** 패턴의 핵심입니다.

1.  `<input>`의 `value` 속성에는 `value` State를 연결합니다.
2.  `<input>`의 `onChange` 속성에는 `handleChange` 함수를 연결합니다.

```jsx
// src/components/InputForm.jsx

// ... (import, 함수 선언 부분은 이전과 동일)

function InputForm() {
  const [value, setValue] = useState('');

  const handleChange = (event) => {
    setValue(event.target.value);
  };

  return (
    <div>
      <h2>간단한 입력 폼</h2>
      <input 
        type="text" 
        value={value}       // input의 값을 React state와 동기화
        onChange={handleChange} // input의 내용이 바뀔 때마다 handleChange 함수 호출
      />
      <p>입력된 텍스트: {value}</p>
    </div>
  );
}

export default InputForm;
```

> [!IMPORTANT]
> **제어 컴포넌트 (Controlled Component)**
> 위와 같이 `<input>`의 값을 React State로 제어하는 방식을 "제어 컴포넌트"라고 부릅니다. 사용자가 키보드를 입력하면 `onChange`가 발생하여 State를 바꾸고, 바뀐 State가 다시 `input`의 `value`로 전달되어 화면이 업데이트됩니다. 이렇게 하면 데이터의 흐름을 명확하게 파악하고 관리할 수 있습니다.

### Step 6: `App.jsx`에서 `InputForm` 컴포넌트 사용하기

마지막으로, `App.jsx`에 우리가 만든 `InputForm` 컴포넌트를 추가합니다.

```jsx
// src/App.jsx

import Counter from './components/Counter';
import Toggle from './components/Toggle';
import InputForm from './components/InputForm'; // 1. InputForm 컴포넌트를 가져옵니다.

function App() {
  return (
    <div>
      <h1>My React App</h1>
      <Counter />
      <hr />
      <Toggle />
      <hr />
      <InputForm /> {/* 2. InputForm 컴포넌트를 렌더링합니다. */}
    </div>
  );
}

export default App;
```

### Step 7: 결과 확인

브라우저 화면에서 입력창에 아무 텍스트나 입력해보세요. 입력하는 내용이 아래 문단에 실시간으로 똑같이 나타나는 것을 볼 수 있습니다.

React Developer Tools를 열어 `InputForm` 컴포넌트를 선택하고, 키보드를 입력할 때마다 `State` 값이 어떻게 변하는지 관찰해보세요.

---

## 🚀 도전 과제 (Optional)

1.  **초기화 버튼**: 입력된 내용을 한 번에 지우는 '초기화' 버튼을 추가해보세요. (힌트: 버튼의 `onClick` 핸들러에서 `setValue('')`를 호출하면 됩니다.)

## ✅ 최종 코드

<details>
<summary><b>src/components/InputForm.jsx</b></summary>

```jsx
import { useState } from 'react';

function InputForm() {
  const [value, setValue] = useState('');

  const handleChange = (event) => {
    setValue(event.target.value);
  };

  return (
    <div>
      <h2>간단한 입력 폼</h2>
      <input 
        type="text" 
        value={value}
        onChange={handleChange}
      />
      <p>입력된 텍스트: {value}</p>
    </div>
  );
}

export default InputForm;
```

</details>

<details>
<summary><b>src/App.jsx</b></summary>

```jsx
import Counter from './components/Counter';
import Toggle from './components/Toggle';
import InputForm from './components/InputForm';

function App() {
  return (
    <div>
      <h1>My React App</h1>
      <Counter />
      <hr />
      <Toggle />
      <hr />
      <InputForm />
    </div>
  );
}

export default App;
```

</details>

---

- [목차로 돌아가기](README.md)
- [이전 강의로 이동](Lab4-Toggle-Component.md)
- [다음 강의로 이동](../day4/README.md)
