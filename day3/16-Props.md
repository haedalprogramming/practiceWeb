# 04. Props

이전 시간에는 UI를 구성하는 독립적인 단위인 컴포넌트에 대해 배웠습니다. 하지만 지금까지 만든 컴포넌트는 항상 정해진 내용만을 보여주었습니다. 이번 시간에는 **Props**를 사용하여 컴포넌트에 동적인 데이터를 전달하고, 재사용성을 극대화하는 방법을 배우겠습니다.

## 1. Props란 무엇인가?

**Props**는 <strong>>properties(속성)</strong>의 줄임말로, **부모 컴포넌트에서 자식 컴포넌트로 데이터를 전달**할 때 사용하는 객체입니다. Props를 통해 컴포넌트는 외부로부터 데이터를 받아 화면에 표시할 내용을 바꿀 수 있습니다.

함수에 인자(argument)를 전달하여 함수의 동작을 다르게 만드는 것처럼, 컴포넌트에 Props를 전달하여 렌더링 결과를 다르게 만들 수 있습니다.

## 2. Props 사용하기

### 1) Props 전달하기 (부모 컴포넌트)

부모 컴포넌트에서 자식 컴포넌트를 사용할 때, HTML의 속성(attribute)을 추가하는 것과 비슷한 방식으로 Props를 전달합니다.

`App` 컴포넌트에서 `Greeting`이라는 자식 컴포넌트에게 `name`이라는 이름의 prop을 전달해 보겠습니다.

```jsx
// src/App.jsx

import Greeting from './components/Greeting';

function App() {
  return (
    <div>
      {/* Greeting 컴포넌트에 name="홍길동" 이라는 prop을 전달 */}
      <Greeting name="홍길동" />
      
      {/* 같은 컴포넌트를 다른 prop 값으로 재사용 */}
      <Greeting name="이순신" />
    </div>
  );
}

export default App;
```

위 코드에서 `<Greeting name="홍길동" />` 부분은 `Greeting` 컴포넌트에게 `{ name: "홍길동" }` 이라는 객체를 Props로 전달하겠다는 의미입니다.

### 2) Props 받아서 사용하기 (자식 컴포넌트)

자식 컴포넌트는 함수형 컴포넌트의 **첫 번째 매개변수**로 부모가 전달한 Props 객체를 받게 됩니다. 관례적으로 이 매개변수의 이름은 `props`라고 짓습니다.

```jsx
// src/components/Greeting.jsx

// 함수의 첫 번째 인자로 props 객체를 받음
function Greeting(props) {
  // props 객체는 { name: "홍길동" } 형태를 가짐
  console.log(props); 

  return <h1>안녕하세요, {props.name}님!</h1>;
}

export default Greeting;
```

`App` 컴포넌트가 `Greeting`을 렌더링할 때, React는 `<Greeting name="홍길동" />`을 보고 `Greeting` 함수를 `{ name: '홍길동' }` 객체를 인자로 넣어 호출합니다. 그 결과 `<h1>안녕하세요, 홍길동님!</h1>` 이라는 JSX가 반환되어 화면에 그려집니다.

> [!TIP]
> **구조 분해 할당 (Destructuring Assignment)**
> 
> 매번 `props.name`, `props.age` 처럼 `props.`를 반복해서 쓰는 것이 번거로울 수 있습니다. JavaScript의 구조 분해 할당 문법을 사용하면 코드를 더 간결하게 만들 수 있습니다.
> 
> ```jsx
> // props 객체에서 name 속성을 바로 추출하여 변수로 사용
> function Greeting({ name }) { 
>   return <h1>안녕하세요, {name}님!</h1>;
> }
> ```

## 3. Props는 읽기 전용 (Read-Only)

매우 중요한 규칙이 있습니다. **컴포넌트는 자신이 받은 Props를 절대 직접 수정해서는 안 됩니다.**

```jsx
function Greeting({ name }) {
  // 💥 잘못된 사용 예: Props를 직접 수정하려고 하면 에러가 발생합니다.
  name = '임꺽정'; // DON'T DO THIS!

  return <h1>안녕하세요, {name}님!</h1>;
}
```

React의 데이터 흐름은 항상 **위에서 아래로(Top-down)**, 즉 부모에서 자식 방향으로 흐릅니다. 자식 컴포넌트가 부모로부터 받은 데이터를 마음대로 바꾸면 데이터 흐름이 꼬여서 애플리케이션을 예측하고 관리하기 매우 어려워집니다. 모든 React 컴포넌트는 자신이 받은 Props에 대해서는 순수 함수처럼 동작해야 합니다.

> [!NOTE]
> **순수 함수(Pure Function)란?**
> 
> 순수 함수는 입력값이 같으면 항상 같은 출력값을 내놓고, 함수 외부의 어떤 상태도 변경하지 않는(Side effect가 없는) 함수를 말합니다. Props를 바꾸지 않는 컴포넌트는 순수 함수와 같습니다.

컴포넌트 내부에서 데이터를 변경하고 싶을 때는 어떻게 해야 할까요? 그럴 때 사용하는 것이 바로 다음 시간에 배울 **State**입니다.

## 4. `props.children`

Props에는 우리가 직접 `name="..."` 처럼 전달하는 것 외에 특별한 prop이 하나 더 있습니다. 바로 `children` 입니다.

컴포넌트 태그를 열고 닫는 태그 사이에 내용을 넣으면, 그 내용이 `props.children`으로 전달됩니다.

`Card`라는 컴포넌트를 만들어 보겠습니다.

```jsx
// src/components/Card.jsx

// children prop을 받아와서 지정된 위치에 렌더링
function Card({ children }) {
  return (
    <div style={{ border: '1px solid gray', padding: '16px', margin: '8px' }}>
      {children}
    </div>
  );
}

export default Card;
```

이제 `App.jsx`에서 `Card` 컴포넌트를 사용해 봅시다.

```jsx
// src/App.jsx

import Card from './components/Card';

function App() {
  return (
    <div>
      <Card>
        {/* 이 안에 있는 모든 내용이 Card 컴포넌트의 children prop으로 전달됩니다. */}
        <h2>첫 번째 카드</h2>
        <p>이것은 Card 컴포넌트의 자식 요소들입니다.</p>
      </Card>

      <Card>
        <img src="/logo.png" alt="logo" />
      </Card>
    </div>
  );
}
```

`props.children`을 사용하면 다른 컴포넌트를 감싸는 형태의 "컨테이너" 컴포넌트를 쉽게 만들 수 있어 매우 유용합니다.

Props는 컴포넌트를 동적이고 재사용 가능하게 만드는 핵심적인 개념입니다. 다음 시간에는 컴포넌트가 스스로 데이터를 관리하고 변경할 수 있게 해주는 **State**에 대해 배우겠습니다.

---

- [목차로 돌아가기](README.md)
- [이전 강의로 이동](15-Components.md)
- [다음 강의로 이동](17-State-and-UseState-Hook.md)
