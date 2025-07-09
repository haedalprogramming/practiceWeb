# 11) React Hook Form을 이용한 고급 폼 관리

## 1) React에서 폼(Form)이란?

웹 애플리케이션에서 <strong>폼(Form)</strong>은 사용자가 정보를 입력하고 서버로 전송할 수 있게 해주는 핵심 요소입니다. 회원가입, 로그인, 게시글 작성, 검색 등 사용자와의 상호작용 대부분이 폼을 통해 이루어집니다.

React에서 폼을 다룰 때는 각 입력 필드(input, textarea 등)의 값을 추적하고 관리해야 합니다. 일반적으로 `useState` Hook을 사용하여 각 입력 값에 대한 상태(state)를 만들어 관리하는데, 이를 **제어 컴포넌트(Controlled Component)** 방식이라고 부릅니다.

```jsx
function MyForm() {
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');

  const handleNameChange = (event) => {
    setName(event.target.value);
  };

  const handleEmailChange = (event) => {
    setEmail(event.target.value);
  };

  const handleSubmit = (event) => {
    event.preventDefault();
    alert(`이름: ${name}, 이메일: ${email}`);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input type="text" value={name} onChange={handleNameChange} />
      <input type="email" value={email} onChange={handleEmailChange} />
      <button type="submit">제출</button>
    </form>
  );
}
```

## 2) 제어 컴포넌트 방식의 문제점

제어 컴포넌트 방식은 React의 데이터 흐름을 따르기 때문에 직관적이지만, 폼이 복잡해질수록 여러 문제점을 드러냅니다.

-   **장황한 코드 (Boilerplate)**: 입력 필드가 많아질수록 `useState`와 `onChange` 핸들러 함수도 그만큼 늘어나 코드가 길고 반복적으로 변합니다.
-   **성능 저하**: 사용자가 입력 필드에 글자를 하나씩 입력할 때마다 해당 컴포넌트의 상태가 변경되고, 그럴 때마다 <strong>리렌더링(Re-rendering)</strong>이 발생합니다. 폼이 크고 복잡하다면 잦은 리렌더링은 애플리케이션의 성능을 저하시킬 수 있습니다.
-   **복잡한 상태 관리**: 유효성 검사(validation) 로직, 에러 메시지 표시, 폼 제출 상태(활성화/비활성화) 등을 직접 구현하려면 상태 관리가 매우 복잡해집니다.

> [!WARNING]
> <strong>리렌더링(Re-rendering)</strong>이란 컴포넌트의 상태(state)나 속성(props)이 변경되었을 때, React가 해당 컴포넌트의 UI를 다시 그리는 과정을 말합니다. 불필요한 리렌더링이 자주 발생하면 앱의 반응성이 떨어질 수 있습니다.

## 3) React Hook Form 소개

<strong>React Hook Form</strong>은 이러한 문제점들을 해결하기 위해 등장한, 폼 상태 관리 라이브러리입니다. Hook을 기반으로 동작하며, 최소한의 코드로 성능이 뛰어난 폼을 쉽게 만들 수 있도록 도와줍니다.

React Hook Form의 핵심 철학은 다음과 같습니다.

-   **성능 향상**: 불필요한 리렌더링을 최소화하여 성능을 극대화합니다. (기본적으로 <strong>비제어 컴포넌트(Uncontrolled Component)</strong> 방식을 사용)
-   **코드 양 감소**: `useState`와 `onChange` 핸들러 없이 폼을 만들 수 있어 코드 양이 획기적으로 줄어듭니다.
-   **개발자 경험 향상**: 간단하고 직관적인 API를 제공하여 배우기 쉽고 사용하기 편리합니다.

> [!NOTE]
> <strong>비제어 컴포넌트(Uncontrolled Component)</strong>는 기존 HTML 폼 요소처럼 DOM 자체가 폼 데이터(상태)를 관리하는 방식입니다. React Hook Form은 `ref`를 통해 DOM에 직접 접근하여 입력 값을 가져오기 때문에, 입력할 때마다 상태가 변하지 않아 리렌더링이 발생하지 않습니다.

## 4) React Hook Form 기본 사용법

React Hook Form을 사용하려면 먼저 라이브러리를 설치해야 합니다.

```bash
npm install react-hook-form
```

이제 기본적인 사용법을 알아보겠습니다. 가장 중요한 것은 `useForm`이라는 Hook입니다.

```jsx
import { useForm } from 'react-hook-form';

function SimpleForm() {
  const { register, handleSubmit, formState: { errors } } = useForm();

  // 폼 제출 시 실행될 함수 (유효성 검사 통과 시)
  const onSubmit = (data) => {
    console.log(data); // { email: '입력값', password: '입력값' }
    alert(JSON.stringify(data));
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <div>
        <label>Email</label>
        <input
          type="email"
          {...register("email", { required: "이메일은 필수 입력 항목입니다." })}
        />
        {errors.email && <p>{errors.email.message}</p>}
      </div>

      <div>
        <label>Password</label>
        <input
          type="password"
          {...register("password", { required: "비밀번호는 필수 입력 항목입니다." })}
        />
        {errors.password && <p>{errors.password.message}</p>}
      </div>

      <button type="submit">로그인</button>
    </form>
  );
}
```

### 4-1) `useForm` Hook

`useForm`은 React Hook Form의 핵심 Hook으로, 폼 관리에 필요한 다양한 메서드와 상태를 반환합니다.

-   **`register`**: 입력 필드를 React Hook Form에 등록하는 함수입니다. 이 함수를 사용하면 `onChange`, `onBlur`, `value`, `ref` 등을 수동으로 설정할 필요가 없습니다. `register` 함수의 첫 번째 인자는 해당 입력 필드의 `name`이 되며, 두 번째 인자로는 유효성 검사 규칙 객체를 전달할 수 있습니다.
-   **`handleSubmit`**: 폼 제출 이벤트를 처리하는 함수입니다. 이 함수는 먼저 내부적으로 연결된 모든 유효성 검사를 실행하고, 모든 검사를 통과했을 때만 우리가 전달한 `onSubmit` 콜백 함수를 실행합니다.
-   **`formState: { errors }`**: 폼의 상태, 특히 유효성 검사 에러 정보를 담고 있는 객체입니다. `errors` 객체에는 유효성 검사에 실패한 필드의 정보와 에러 메시지가 담겨 있습니다.

## 5) 유효성 검사 (Validation)

React Hook Form은 매우 간단하고 선언적인 방식으로 유효성 검사를 지원합니다. `register` 함수의 두 번째 인자에 규칙을 명시하기만 하면 됩니다.

```jsx
<input
  {...register("username", {
    required: "사용자 이름은 필수입니다.",
    minLength: {
      value: 3,
      message: "이름은 3글자 이상이어야 합니다."
    },
    pattern: {
      value: /^[A-Za-z]+$/i,
      message: "영어 알파벳만 입력 가능합니다."
    }
  })}
/>
{errors.username && <p>{errors.username.message}</p>}
```

주요 유효성 검사 규칙은 다음과 같습니다.

-   **`required`**: 필수 입력 항목인지 여부를 검사합니다.
-   **`minLength`**: 최소 길이를 검사합니다.
-   **`maxLength`**: 최대 길이를 검사합니다.
-   **`min`**, **`max`**: 최솟값, 최댓값을 검사합니다. (주로 `type="number"`일 때 사용)
-   **`pattern`**: 정규 표현식(Regular Expression)을 사용하여 복잡한 패턴을 검사합니다.
-   **`validate`**: 커스텀 유효성 검사 함수를 직접 만들어 적용할 수 있습니다.

> [!TIP]
> `errors` 객체와 조건부 렌더링(`&&`)을 함께 사용하면, 특정 필드에 에러가 있을 때만 에러 메시지를 간편하게 화면에 표시할 수 있습니다.

## 6) 결론

React Hook Form은 React에서 폼을 다루는 작업을 매우 효율적이고 간단하게 만들어주는 강력한 라이브러리입니다. 제어 컴포넌트 방식의 단점인 성능 문제와 장황한 코드를 해결하고, 직관적인 API로 유효성 검사까지 쉽게 구현할 수 있게 해줍니다.

복잡한 폼을 만들 계획이 있다면, React Hook Form을 사용하는 것은 이제 선택이 아닌 필수에 가깝습니다. 더 나은 성능, 더 적은 코드, 그리고 즐거운 개발 경험을 위해 React Hook Form을 적극적으로 활용해 보세요.

---

- [목차로 돌아가기](./README.md)
- [이전 강의로 이동](./10-Intro-to-Tailwind-CSS.md)
- [다음 강의로 이동](./12-Web-Accessibility.md) 