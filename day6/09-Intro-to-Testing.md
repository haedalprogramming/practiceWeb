# 9) Jest와 React Testing Library 소개

우리가 만든 React 애플리케이션이 복잡해질수록, 새로운 기능을 추가하거나 기존 코드를 수정할 때 예상치 못한 오류가 발생할 위험이 커집니다. 버튼 하나를 고쳤더니 전혀 다른 기능이 먹통이 되는 경험을 하고 싶지는 않겠죠?

이러한 문제를 방지하고 애플리케이션의 안정성을 높이기 위해 우리는 **테스트 코드**를 작성합니다. 이번 시간에는 React 테스트의 기본 개념과 핵심 도구인 Jest와 React Testing Library에 대해 알아보겠습니다.

---

## 1) 테스트는 왜 중요할까?

소프트웨어 **테스트**는 **코드가 의도한 대로 정확하게 동작하는지 검증하는 과정**입니다. 테스트 코드를 작성하면 다음과 같은 장점이 있습니다.

-   **버그 조기 발견**: 개발 과정에서 실수를 빠르게 찾아내고 수정할 수 있습니다.
-   **안정성 확보**: 새로운 기능이 추가되어도 기존 기능이 잘 동작한다는 것을 보장하여, 애플리케이션을 안정적으로 유지할 수 있습니다.
-   **자신감 있는 리팩토링**: 코드를 개선하거나 구조를 변경(리팩토링)할 때, 테스트가 있다면 기능이 망가지는 것을 두려워하지 않고 과감하게 시도할 수 있습니다.
-   **살아있는 문서**: 잘 작성된 테스트 코드는 그 자체로 해당 코드가 어떻게 동작해야 하는지를 보여주는 명확한 문서 역할을 합니다.

---

## 2) Jest와 React Testing Library의 역할

React 테스트 환경에서는 주로 두 가지 도구가 함께 사용됩니다.

### 2-1) Jest

**Jest**는 Facebook에서 만든 **JavaScript 테스트 프레임워크**입니다. 프레임워크란 특정 작업을 수행하는 데 필요한 여러 도구를 모아놓은 종합세트라고 생각할 수 있습니다. Jest는 다음과 같은 역할을 합니다.

-   **테스트 러너(Test Runner)**: 테스트 파일을 찾아서 실행하고, 결과를 요약해서 보여줍니다.
-   **단언(Assertion) 라이브러리**: `expect(A).toBe(B)` 와 같은 문법을 사용하여, 코드의 실제 결과(A)가 우리가 기대하는 결과(B)와 같은지 검증하는 기능을 제공합니다.
-   **모킹(Mocking)**: 테스트하려는 코드와 연결된 다른 부분을 실제처럼 흉내 내는 가짜(Mock) 객체를 만드는 기능을 제공합니다.

### 2-2) React Testing Library (RTL)

**React Testing Library**는 **React 컴포넌트를 테스트하기 위한 라이브러리**입니다. Jest가 테스트를 실행하고 검증하는 전반적인 환경을 제공한다면, RTL은 그 환경 안에서 **사용자 관점**으로 컴포넌트를 테스트할 수 있도록 도와주는 유용한 도구들을 제공합니다.

> [!IMPORTANT]
> **React Testing Library의 핵심 철학**
> "The more your tests resemble the way your software is used, the more confidence they can give you."
> **"테스트는 소프트웨어가 사용되는 방식과 유사할수록 더 큰 신뢰를 줍니다."**
>
> RTL은 컴포넌트의 내부 구현(state가 어떻게 변하는지, 어떤 함수가 호출되는지 등)을 테스트하는 것을 지양합니다. 대신 **실제 사용자가 화면을 보고 상호작용하는 방식**으로 테스트하기를 권장합니다. 예를 들어, "버튼을 클릭했더니, 화면에 '성공!'이라는 메시지가 나타났다"와 같이 테스트하는 것이죠.

---

## 3) 첫 번째 React 테스트 작성하기

`Create React App`으로 프로젝트를 만들면 Jest와 React Testing Library가 이미 기본적으로 설치 및 설정되어 있어 바로 테스트를 작성할 수 있습니다.

### 3-1) 테스트할 컴포넌트

먼저 간단한 `Greeting` 컴포넌트를 만들어 보겠습니다.

```jsx
// src/components/Greeting.js
function Greeting({ name }) {
  return <p>안녕하세요, {name}!</p>;
}

export default Greeting;
```

### 3-2) 테스트 코드 작성

일반적으로 테스트 파일은 `컴포넌트명.test.js` 또는 `컴포넌트명.spec.js` 라는 이름으로 만듭니다.

```jsx
// src/components/Greeting.test.js
import { render, screen } from '@testing-library/react';
import Greeting from './Greeting';

// 1. describe: 여러 관련 테스트를 하나의 그룹으로 묶어줍니다.
describe('Greeting 컴포넌트', () => {
  // 2. test (또는 it): 개별 테스트 케이스를 정의합니다.
  test('name prop이 주어졌을 때, 인사말을 올바르게 렌더링한다', () => {
    // 3. Arrange(준비): 테스트할 컴포넌트를 렌더링합니다.
    render(<Greeting name="철수" />);

    // 4. Act(실행): 여기서는 렌더링 자체가 실행 단계입니다.
    // 사용자의 클릭과 같은 상호작용이 있다면 이 단계에서 수행합니다.

    // 5. Assert(단언): 화면에 기대하는 결과가 있는지 확인합니다.
    const greetingElement = screen.getByText(/안녕하세요, 철수!/);
    expect(greetingElement).toBeInTheDocument();
  });
});
```

-   `render()`: 테스트를 위해 컴포넌트를 가상 화면에 렌더링합니다.
-   `screen`: 렌더링된 가상 화면에 접근하기 위한 객체입니다.
-   `getByText()`: `screen` 객체의 쿼리(Query) 중 하나로, 텍스트 내용을 기반으로 요소를 찾습니다.
-   `expect(...).toBeInTheDocument()`: Jest의 단언문으로, `greetingElement`가 화면(document)에 존재하는지 확인합니다.

### 3-3) 테스트 실행하기

터미널에서 다음 명령어를 실행하면 프로젝트 내의 모든 테스트 파일이 실행됩니다.

```bash
npm test
```

Jest는 테스트를 실행하고, 통과했는지 실패했는지를 알려줄 것입니다.

---

## 4) 사용자 상호작용 테스트하기

이번에는 사용자의 행동(클릭 등)을 테스트해 보겠습니다. RTL은 `@testing-library/user-event` 라이브러리를 통해 사용자의 상호작용을 실제와 가깝게 시뮬레이션할 수 있도록 지원합니다.

```jsx
// src/components/Counter.js
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>증가</button>
    </div>
  );
}
```

```jsx
// src/components/Counter.test.js
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import Counter from './Counter';

test('사용자가 증가 버튼을 클릭하면 카운트가 1 증가한다', async () => {
  // 준비
  render(<Counter />);

  // 실행
  const button = screen.getByRole('button', { name: '증가' });
  await userEvent.click(button);

  // 단언
  const countElement = screen.getByText(/Count: 1/);
  expect(countElement).toBeInTheDocument();
});
```

-   `userEvent.click(element)`: 사용자가 특정 요소를 클릭하는 행동을 시뮬레이션합니다. `user-event`의 동작은 비동기로 처리될 수 있으므로 `async/await`을 사용하는 것이 좋습니다.
-   `getByRole('button', ...)`: 요소의 역할(role)을 기반으로 찾는 쿼리입니다. '증가'라는 이름을 가진 'button' 역할을 하는 요소를 찾습니다. 이렇게 찾는 것이 시각 장애인 사용자가 사용하는 스크린 리더의 동작 방식과 유사하여 접근성을 고려한 좋은 테스트 방식입니다.

---

## 5) 정리

-   **테스트**는 애플리케이션의 안정성을 높이고 자신감 있는 개발을 가능하게 하는 필수적인 과정입니다.
-   **Jest**는 테스트 실행 환경과 검증 도구를 제공하는 테스트 프레임워크입니다.
-   **React Testing Library**는 사용자의 관점에서 컴포넌트를 테스트하도록 돕는 라이브러리입니다.
-   좋은 테스트는 **내부 구현이 아닌, 사용자에게 보이는 결과와 동작**을 중심으로 작성되어야 합니다.

테스팅은 처음에는 어색하고 번거롭게 느껴질 수 있지만, 장기적으로는 훨씬 더 견고하고 유지보수하기 좋은 애플리케이션을 만드는 데 큰 도움이 될 것입니다.

---

- [목차로 돌아가기](./README.md)
- [이전 강의로 이동](./08-React-DevTools-Advanced.md)
- [다음 강의로 이동](./10-Intro-to-Tailwind-CSS.md)
