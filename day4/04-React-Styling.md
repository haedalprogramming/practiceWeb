# 3) 스타일링

## 3-1) React 스타일링

React 애플리케이션에서 UI를 시각적으로 꾸미는 것을 **스타일링**이라고 합니다. React는 컴포넌트 기반의 개발을 지향하기 때문에, 각 컴포넌트의 스타일을 독립적으로 관리하는 다양한 방법들이 존재합니다. 이번 강의에서는 React에서 주로 사용되는 스타일링 방식들을 소개하고, 각각의 특징을 알아보겠습니다.

### 3-1-1) CSS Module

CSS Module은 CSS 클래스 이름이 전역적으로 겹치는 문제를 해결하기 위해 고안된 방법입니다. 일반적인 CSS는 클래스 이름이 전역적으로 적용되어, 다른 컴포넌트에서 같은 클래스 이름을 사용하면 스타일이 겹치거나 예상치 못한 문제가 발생할 수 있습니다. CSS Module은 이러한 문제를 방지하고, 각 컴포넌트의 스타일을 독립적으로 유지할 수 있도록 도와줍니다.

#### 특징

*   **고유한 클래스 이름**: CSS Module을 사용하면 CSS 클래스 이름이 자동으로 고유하게 생성됩니다. 예를 들어, `button`이라는 클래스 이름을 사용해도 실제로는 `Button_button__abc123`과 같이 고유한 이름으로 변환되어 다른 컴포넌트의 `button` 클래스와 충돌하지 않습니다.
*   **지역 스코프**: 스타일이 해당 컴포넌트에만 적용되므로, 다른 컴포넌트에 영향을 주지 않습니다.
*   **간단한 사용법**: `.module.css` 확장자를 사용하여 CSS 파일을 생성하고, 이를 컴포넌트에서 불러와 사용합니다.

#### 사용 예시

1.  `Button.module.css` 파일 생성:

    ```css
    /* Button.module.css */
    .button {
      background-color: blue;
      color: white;
      padding: 10px 20px;
      border-radius: 5px;
    }
    ```

2.  `Button.js` 컴포넌트에서 사용:

    ```jsx
    // Button.js
    import React from 'react';
    import styles from './Button.module.css'; // CSS Module 파일 임포트

    function Button() {
      return (
        <button className={styles.button}>
          클릭하세요
        </button>
      );
    }

    export default Button;
    ```

> [!TIP]
> CSS Module은 클래스 이름 충돌을 방지하고 컴포넌트 기반의 스타일링을 선호하는 경우에 유용합니다.

### 3-1-2) Tailwind CSS

Tailwind CSS는 유틸리티 우선(Utility-first) CSS 프레임워크입니다. 미리 정의된 수많은 유틸리티 클래스들을 HTML 태그에 직접 적용하여 스타일을 입히는 방식입니다. CSS 파일을 직접 작성하는 대신, `text-blue-500`, `p-4`, `flex`, `justify-center`와 같은 클래스들을 조합하여 디자인을 만듭니다.

#### 특징

*   **빠른 개발 속도**: 미리 정의된 클래스들을 조합하여 빠르게 UI를 구축할 수 있습니다.
*   **작은 CSS 파일 크기**: 사용하지 않는 CSS는 빌드 시 제거되므로, 최종 CSS 파일 크기가 작습니다.
*   **일관된 디자인 시스템**: 유틸리티 클래스들이 일관된 디자인 시스템을 기반으로 하므로, 디자인 일관성을 유지하기 쉽습니다.
*   **HTML에 직접 스타일 적용**: CSS 파일을 따로 관리할 필요 없이 HTML 구조 안에서 스타일을 정의합니다.

#### 사용 예시

Tailwind CSS를 설치하고 설정한 후, 다음과 같이 HTML 태그에 클래스를 직접 적용합니다.

```jsx
// MyComponent.js
import React from 'react';

function MyComponent() {
  return (
    <div className="flex justify-center items-center h-screen bg-gray-100">
      <button className="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
        Tailwind 버튼
      </button>
    </div>
  );
}

export default MyComponent;
```

> [!NOTE]
> `flex`, `justify-center`, `items-center`, `h-screen`, `bg-gray-100` 등은 모두 Tailwind CSS에서 제공하는 유틸리티 클래스입니다.

> [!TIP]
> Tailwind CSS는 매우 강력하고 인기 있는 도구입니다. Day 6의 '심화 주제'에서 설치부터 실제 프로젝트에 적용하는 방법까지 더 자세히 다룰 예정입니다.


### 3-1-3) Styled-Components

Styled-Components는 CSS-in-JS 라이브러리 중 하나입니다. JavaScript 코드 안에서 CSS를 작성하여 React 컴포넌트에 직접 스타일을 적용하는 방식입니다. 이를 통해 컴포넌트와 스타일을 하나의 파일에서 관리할 수 있습니다.

#### 특징

*   **컴포넌트 기반 스타일링**: 각 React 컴포넌트에 종속적인 스타일을 정의할 수 있습니다.
*   **CSS의 모든 기능 사용**: 일반 CSS 문법을 그대로 사용할 수 있으며, JavaScript의 기능을 활용하여 동적인 스타일링도 가능합니다.
*   **자동으로 고유한 클래스 이름 생성**: CSS Module과 유사하게 자동으로 고유한 클래스 이름을 생성하여 스타일 충돌을 방지합니다.
*   **Props를 이용한 동적 스타일링**: 컴포넌트의 `props` 값을 사용하여 조건부 스타일링을 쉽게 구현할 수 있습니다.

#### 사용 예시

1.  Styled-Components 설치:

    ```bash
    npm install styled-components
    ```

2.  `StyledButton.js` 컴포넌트 생성:

    ```jsx
    // StyledButton.js
    import React from 'react';
    import styled from 'styled-components'; // styled 임포트

    // styled.button을 사용하여 <button> 태그에 스타일을 적용한 새로운 컴포넌트 생성
    const StyledButton = styled.button`
      background-color: ${props => props.primary ? 'palevioletred' : 'white'};
      color: ${props => props.primary ? 'white' : 'palevioletred'};
      font-size: 1em;
      margin: 1em;
      padding: 0.25em 1em;
      border: 2px solid palevioletred;
      border-radius: 3px;
    `;

    function Button({ primary, children }) {
      return (
        <StyledButton primary={primary}>
          {children}
        </StyledButton>
      );
    }

    export default Button;
    ```

3.  `App.js`에서 사용:

    ```jsx
    // App.js
    import React from 'react';
    import Button from './StyledButton';

    function App() {
      return (
        <div>
          <Button>일반 버튼</Button>
          <Button primary>프라이머리 버튼</Button>
        </div>
      );
    }

    export default App;
    ```

> [!IMPORTANT]
> Styled-Components는 JavaScript 코드 내에서 CSS를 작성하므로, JavaScript의 변수나 함수를 사용하여 동적인 스타일을 적용할 수 있다는 강력한 장점이 있습니다.

### 3-1-4) 어떤 스타일링 방식을 선택해야 할까?

각 스타일링 방식은 장단점이 명확하므로, 프로젝트의 특성과 팀의 선호도에 따라 적절한 방식을 선택하는 것이 중요합니다.

*   **CSS Module**: CSS 클래스 이름 충돌을 피하고 싶지만, 기존 CSS 문법에 익숙한 경우에 좋습니다.
*   **Tailwind CSS**: 빠른 프로토타이핑이나 디자인 시스템이 명확한 경우, 그리고 CSS를 직접 작성하는 시간을 줄이고 싶은 경우에 매우 효율적입니다.
*   **Styled-Components**: 컴포넌트와 스타일을 함께 관리하고 싶고, JavaScript의 기능을 활용하여 동적인 스타일링을 많이 해야 하는 경우에 적합합니다.

> [!TIP]
> 하나의 프로젝트에서 여러 스타일링 방식을 혼합하여 사용하는 경우도 있습니다. 예를 들어, 전역적인 스타일은 일반 CSS로 관리하고, 컴포넌트별 스타일은 CSS Module이나 Styled-Components를 사용하는 방식입니다.

---

- [목차로 돌아가기](../README.md)
- [이전 강의로 이동](03-Component-Design-Patterns.md)
- [다음 강의로 이동](Lab1-Profile-Page-React.md)