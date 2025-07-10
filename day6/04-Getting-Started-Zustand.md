# 4. Zustand 시작하기

이전 시간에는 여러 상태 관리 라이브러리를 비교하고, 왜 우리 과정에서 **Zustand**를 선택했는지 알아보았습니다. 이제 직접 Zustand를 설치하고 사용해보면서, 얼마나 쉽고 강력하게 상태를 관리할 수 있는지 체험해 보겠습니다.

이번 시간에는 간단한 카운터(Counter) 예제를 통해 Zustand의 핵심 사용법을 익혀보겠습니다.

---

### 목차

1.  [Zustand 설치](#1-zustand-설치)
2.  [스토어(Store) 만들기](#2-스토어store-만들기)
3.  [컴포넌트에서 스토어 사용하기](#3-컴포넌트에서-스토어-사용하기)
4.  [전체 예제 코드](#4-전체-예제-코드)
5.  [Zustand의 핵심 정리](#5-zustand의-핵심-정리)

---

## 1) Zustand 설치

먼저, React 프로젝트에 Zustand 라이브러리를 추가해야 합니다. 터미널을 열고 프로젝트 폴더로 이동한 뒤, 다음 명령어를 실행하여 Zustand를 설치합니다.

```bash
npm install zustand
```

> [!TIP]
> **npm(Node Package Manager)** 은 JavaScript로 만들어진 다양한 라이브러리(패키지)를 쉽게 설치하고 관리할 수 있게 도와주는 도구입니다. `npm install` 명령어는 필요한 라이브러리를 다운로드하여 우리 프로젝트에 추가하는 역할을 합니다.

## 2) 스토어(Store) 만들기

Zustand의 가장 기본적인 개념은 **스토어(Store)** 입니다. 스토어는 애플리케이션의 상태(state)와 상태를 변경하는 함수(action)들을 담고 있는 중앙 저장소입니다.

Zustand에서는 `create` 함수를 사용하여 스토어를 만듭니다. 스토어는 **커스텀 훅(Custom Hook)** 형태로 만들어지므로, 어떤 컴포넌트에서든 쉽게 불러와 사용할 수 있습니다.

카운터 예제를 위한 스토어를 만들어 보겠습니다. 보통 스토어는 `src/store.js` 또는 `src/stores/counterStore.js` 와 같이 별도의 파일로 관리하는 것이 좋습니다.

```javascript
// src/store.js
import { create } from 'zustand';

const useCounterStore = create((set) => ({
  // 1. 상태 (state)
  count: 0,
  
  // 2. 상태를 변경하는 함수 (action)
  increase: () => set((state) => ({ count: state.count + 1 })),
  decrease: () => set((state) => ({ count: state.count - 1 })),
  reset: () => set({ count: 0 }),
}));

export default useCounterStore;
```

> [!IMPORTANT]
> - **`create(initializer)`**: `create` 함수는 스토어의 초기 상태와 액션을 정의하는 함수를 인자로 받습니다.
> - **`set` 함수**: `set` 함수는 Zustand로부터 전달받는 특별한 함수로, 상태를 업데이트하는 데 사용됩니다. `set` 함수를 호출하여 스토어의 상태를 변경할 수 있습니다.
>   - `set((state) => ({ ... }))` 처럼 현재 상태(`state`)를 받아와서 새로운 상태 객체를 반환하는 형태로 사용하면, 여러 비동기 작업에서도 안전하게 상태를 업데이트할 수 있습니다.
>   - `set({ count: 0 })` 처럼 객체를 바로 전달하여 상태를 덮어쓸 수도 있습니다.
> - **커스텀 훅**: `useCounterStore`라는 이름의 커스텀 훅이 만들어졌습니다. 이제 이 훅을 컴포넌트에서 사용하여 `count` 상태와 `increase`, `decrease` 같은 함수에 접근할 수 있습니다.

## 3) 컴포넌트에서 스토어 사용하기

이제 방금 만든 `useCounterStore` 훅을 React 컴포넌트에서 사용해 보겠습니다.

```jsx
// src/Counter.jsx
import React from 'react';
import useCounterStore from './store';

function Counter() {
  const count = useCounterStore((state) => state.count);
  const increase = useCounterStore((state) => state.increase);
  const decrease = useCounterStore((state) => state.decrease);
  const reset = useCounterStore((state) => state.reset);

  return (
    <div>
      <h1>카운터</h1>
      <p>현재 값: {count}</p>
      <button onClick={increase}>증가</button>
      <button onClick={decrease}>감소</button>
      <button onClick={reset}>초기화</button>
    </div>
  );
}

export default Counter;
```

> [!NOTE]
> `useCounterStore(selector)` 형태로 스토어를 사용합니다. 여기서 **셀렉터(selector)** 는 스토어의 전체 상태(`state`) 중에서 어떤 값을 가져올지 선택하는 함수입니다.
> - `(state) => state.count`는 스토어에서 `count` 값만 가져오겠다는 의미입니다.
> - `(state) => state.increase`는 스토어에서 `increase` 함수만 가져오겠다는 의미입니다.

### <strong>성능 최적화</strong>

Zustand의 가장 큰 장점 중 하나는 **자동으로 성능 최적화가 된다는 점**입니다.

위 예제에서 `count` 상태만 사용하는 컴포넌트는 `count`가 변경될 때만 리렌더링됩니다. 만약 스토어에 다른 상태 (예: `user`)가 추가되고 그 상태가 변경되더라도, `count`를 구독하는 컴포넌트는 영향을 받지 않습니다.

이렇게 필요한 상태만 정확히 구독하는 방식 덕분에, Context API에서 발생할 수 있는 불필요한 리렌더링 문제를 자연스럽게 피할 수 있습니다.

만약 여러 상태와 액션을 한 번에 가져오고 싶다면 다음과 같이 작성할 수도 있습니다.

```jsx
// 여러 값을 한 번에 가져오기
const { count, increase, decrease } = useCounterStore((state) => ({
  count: state.count,
  increase: state.increase,
  decrease: state.decrease,
}));
```

## 4) 전체 예제 코드

다음은 위에서 설명한 `store.js`와 `Counter.jsx`를 합쳐 실행 가능한 전체 코드입니다.

```jsx
// App.jsx (혹은 main.jsx)

import React from 'react';
import { create } from 'zustand';

// 1. 스토어 생성 (원래는 별도 파일로 분리)
const useCounterStore = create((set) => ({
  count: 0,
  increase: () => set((state) => ({ count: state.count + 1 })),
  decrease: () => set((state) => ({ count: state.count - 1 })),
  reset: () => set({ count: 0 }),
}));

// 2. 카운터 컴포넌트
function Counter() {
  const count = useCounterStore((state) => state.count);
  const increase = useCounterStore((state) => state.increase);
  const decrease = useCounterStore((state) => state.decrease);
  const reset = useCounterStore((state) => state.reset);

  return (
    <div>
      <h1>Zustand 카운터</h1>
      <p>현재 값: <strong>{count}</strong></p>
      <button onClick={increase}>증가</button>
      <button onClick={decrease}>감소</button>
      <button onClick={reset}>초기화</button>
    </div>
  );
}

// 3. 앱 컴포넌트
function App() {
  return (
    <div>
      <Counter />
    </div>
  );
}

export default App;
```

## 5) Zustand의 핵심 정리

-   **Provider가 필요 없습니다**: Context API와 달리, 애플리케이션을 `<Provider>`로 감싸줄 필요가 없습니다. 그냥 훅을 가져와 사용하면 됩니다.
-   **적은 코드 (Less Boilerplate)**: Redux 등에 비해 훨씬 적은 양의 코드로 동일한 기능을 구현할 수 있습니다.
-   **성능 최적화**: 필요한 상태만 골라서 구독(subscribe)하므로, 불필요한 리렌더링이 발생하지 않습니다.

이제 여러분은 Zustand를 사용하여 React 애플리케이션의 상태를 관리할 준비가 되었습니다! 다음 실습에서는 이 지식을 활용하여 더 흥미로운 기능을 만들어 보겠습니다.

---

- [목차로 돌아가기](./README.md)
- [이전 강의로 이동](./03-State-Management-Libs.md)
- [다음 강의로 이동](./05-Error-Handling.md)
