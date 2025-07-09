# 실습 3: React Hook Form으로 회원가입 폼 만들기

React에서 `useState`로 폼을 관리하면 입력 필드가 많아지고 유효성 검사가 복잡해질수록 코드가 길어지고 성능 문제가 발생하기 쉽습니다. <strong>React Hook Form (RHF)</strong>은 이러한 문제를 해결해주는 강력한 라이브러리로, 최소한의 리렌더링으로 높은 성능을 보장하며 간결한 코드로 복잡한 폼을 구현할 수 있게 해줍니다.

이번 실습에서는 RHF를 사용하여 일반적인 **회원가입 폼**을 만들어 보겠습니다. 다양한 유효성 검사 규칙을 적용하고 에러 메시지를 처리하는 과정을 통해 RHF의 핵심 기능을 완벽하게 익혀봅시다.

---

## 1) 학습 목표

- React Hook Form을 설치하고 `useForm` 훅을 사용하여 폼을 초기화할 수 있습니다.
- `register` 함수를 사용하여 여러 입력 필드를 폼에 등록하는 방법을 이해합니다.
- `required`, `minLength`, `pattern` 등 내장된 유효성 검사 규칙을 적용할 수 있습니다.
- `validate` 함수를 사용하여 '비밀번호 확인'과 같은 커스텀 유효성 검사를 구현할 수 있습니다.
- `formState: { errors }` 객체를 활용하여 사용자에게 직관적인 에러 메시지를 보여주는 방법을 익힙니다.
- `handleSubmit`을 이용해 유효성 검사를 통과한 데이터만 안전하게 제출하는 로직을 구현할 수 있습니다.

---

## 2) 실습 요구사항

1.  **`react-hook-form` 라이브러리 설치**.
2.  **`SignupForm` 컴포넌트 생성**.
3.  **폼 필드 및 유효성 검사 규칙**:
    -   **이름 (name)**: 필수, 최소 2글자 이상.
    -   **이메일 (email)**: 필수, 유효한 이메일 형식 (정규 표현식 사용).
    -   **비밀번호 (password)**: 필수, 최소 8글자 이상.
    -   **비밀번호 확인 (confirmPassword)**: 필수, '비밀번호' 필드의 값과 일치해야 함.
4.  각 입력 필드 아래에 유효성 검사를 통과하지 못했을 경우 **에러 메시지를 표시**.
5.  폼 제출 버튼을 클릭했을 때, 모든 유효성 검사를 통과하면 입력된 데이터를 `console.log`로 출력.

---

## 3) 코드 구현

### 3-1) React Hook Form 설치

터미널에서 아래 명령어를 실행하여 라이브러리를 설치합니다.

```bash
npm install react-hook-form
```

### 3-2) `SignupForm` 컴포넌트 구현

`src/components` 폴더에 `SignupForm.js` 파일을 생성하고 아래와 같이 코드를 작성합니다.

**`src/components/SignupForm.js`**
```javascript
import React from 'react';
import { useForm } from 'react-hook-form';

function SignupForm() {
  const { 
    register, 
    handleSubmit, 
    watch,
    formState: { errors } 
  } = useForm({ mode: 'onBlur' });

  // handleSubmit이 유효성 검사를 통과시킨 데이터(data)를 인자로 받아 실행됩니다.
  const onSubmit = (data) => {
    console.log('제출된 데이터:', data);
    alert(JSON.stringify(data, null, 2));
  };

  // 'password' 필드의 현재 값을 감시합니다.
  const password = watch('password');

  return (
    <form onSubmit={handleSubmit(onSubmit)} noValidate>
      <h2>회원가입</h2>

      <div className="form-group">
        <label htmlFor="name">이름</label>
        <input 
          id="name"
          type="text"
          {...register('name', { 
            required: '이름은 필수 입력 항목입니다.',
            minLength: { value: 2, message: '이름은 2글자 이상이어야 합니다.' }
          })} 
        />
        {errors.name && <p className="error-message">{errors.name.message}</p>}
      </div>

      <div className="form-group">
        <label htmlFor="email">이메일</label>
        <input 
          id="email"
          type="email"
          {...register('email', { 
            required: '이메일은 필수 입력 항목입니다.',
            pattern: {
              value: /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/,
              message: '유효한 이메일 주소를 입력해주세요.'
            }
          })} 
        />
        {errors.email && <p className="error-message">{errors.email.message}</p>}
      </div>
      
      <div className="form-group">
        <label htmlFor="password">비밀번호</label>
        <input 
          id="password"
          type="password"
          {...register('password', { 
            required: '비밀번호는 필수 입력 항목입니다.',
            minLength: { value: 8, message: '비밀번호는 8자 이상이어야 합니다.' }
          })} 
        />
        {errors.password && <p className="error-message">{errors.password.message}</p>}
      </div>

      <div className="form-group">
        <label htmlFor="confirmPassword">비밀번호 확인</label>
        <input 
          id="confirmPassword"
          type="password"
          {...register('confirmPassword', { 
            required: '비밀번호 확인은 필수 입력 항목입니다.',
            validate: value => 
              value === password || '비밀번호가 일치하지 않습니다.'
          })} 
        />
        {errors.confirmPassword && <p className="error-message">{errors.confirmPassword.message}</p>}
      </div>
      
      <button type="submit">가입하기</button>
    </form>
  );
}

export default SignupForm;
```
> [!IMPORTANT]
> - `useForm({ mode: 'onBlur' })`: `mode` 옵션을 `'onBlur'`로 설정하면, 사용자가 입력 필드를 벗어났을 때(focus out) 유효성 검사가 실행됩니다. 기본값은 `'onSubmit'`입니다.
> - `watch('password')`: `watch` 함수는 특정 필드의 값을 실시간으로 구독합니다. '비밀번호 확인' 필드의 유효성을 검사하려면 현재 '비밀번호' 필드의 값이 필요하기 때문에 `watch`를 사용했습니다.
> - `validate: value => value === password || '...'`: `validate` 옵션을 사용해 커스텀 유효성 검사 로직을 작성했습니다. 입력값(`value`)이 감시하고 있는 `password` 값과 다를 경우, 지정된 에러 메시지를 반환합니다.
> - `noValidate`: `<form>` 태그에 `noValidate` 속성을 추가하면 브라우저의 기본 HTML5 유효성 검사 기능이 비활성화되어, RHF의 유효성 검사 로직만 사용하게 됩니다.

### 3-3) 스타일링 및 App.js에 추가

간단한 CSS를 추가하여 폼의 가독성을 높이고, `App.js`에서 `SignupForm` 컴포넌트를 렌더링합니다.

**`App.css`에 아래 내용 추가**
```css
form {
  max-width: 400px;
  margin: 40px auto;
  padding: 30px;
  border: 1px solid #ddd;
  border-radius: 8px;
  box-shadow: 0 2px 5px rgba(0,0,0,0.1);
}

.form-group {
  margin-bottom: 20px;
}

label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
}

input[type="text"],
input[type="email"],
input[type="password"] {
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box; /* padding이 너비에 포함되도록 */
}

.error-message {
  color: #e53e3e;
  font-size: 0.875rem;
  margin-top: 5px;
}

button[type="submit"] {
  width: 100%;
  padding: 12px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1rem;
}

button[type="submit"]:hover {
  background-color: #0056b3;
}
```

**`App.js`**
```javascript
import React from 'react';
import SignupForm from './components/SignupForm';
import './App.css';

function App() {
  return (
    <div className="App">
      <SignupForm />
    </div>
  );
}

export default App;
```

---

## 4) 결론

이번 실습을 통해 React Hook Form을 사용하여 복잡한 회원가입 폼을 얼마나 효율적으로 만들 수 있는지 확인했습니다. `useState`를 사용했을 때보다 훨씬 적은 코드로, 더 나은 성능과 강력한 유효성 검사 기능을 구현했습니다.

이제 여러분은 React Hook Form을 활용하여 사용자 친화적이고 안정적인 폼을 자신 있게 만들 수 있게 되었습니다.

---

[메인으로 돌아가기](../README.md)
[이전 실습으로 돌아가기](./Lab2-Global-State-with-Zustand.md)
[다음 강의로 이동하기](../README.md) 