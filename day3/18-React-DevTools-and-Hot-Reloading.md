# 06. React 개발 도구 및 Hot Reloading

지금까지 React의 핵심 개념인 JSX, 컴포넌트, Props, State에 대해 배웠습니다. 이번 시간에는 이러한 개념들을 활용하여 개발할 때 생산성을 크게 높여주는 필수 도구인 **React Developer Tools**와 **Hot Reloading** 기능에 대해 알아보겠습니다.

## 1. React Developer Tools

**React Developer Tools**는 React 애플리케이션을 디버깅하고 검사하기 위해 Meta(페이스북)에서 공식적으로 제공하는 **브라우저 확장 프로그램**입니다.

> [!NOTE]
> <strong>디버깅(Debugging)</strong>이란?
> 
> 코드에 존재하는 오류(버그, Bug)를 찾고 수정하는 과정을 말합니다. React Developer Tools는 이 과정을 훨씬 쉽고 직관적으로 만들어 줍니다.

이 도구를 사용하면 웹 페이지의 어떤 부분이 어떤 컴포넌트로 이루어져 있는지, 각 컴포넌트가 어떤 Props와 State를 가지고 있는지 등을 한눈에 파악할 수 있습니다.

### 1) 설치하기

React Developer Tools는 각 브라우저의 확장 프로그램 스토어에서 설치할 수 있습니다.

*   [Chrome용 설치](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi)
*   [Firefox용 설치](https://addons.mozilla.org/ko/firefox/addon/react-devtools/)
*   [Edge용 설치](https://microsoftedge.microsoft.com/addons/detail/react-developer-tools/gpphkfbcpidddadnkolkpfckpihlkkil)

설치가 완료되면 브라우저 오른쪽 상단에 React 로고 모양의 아이콘이 생깁니다.

### 2) 사용 방법

1.  `npm run dev`로 실행 중인 React 애플리케이션 페이지에서 키보드의 `F12` 키를 눌러 브라우저 개발자 도구를 엽니다.
2.  개발자 도구 탭 목록에서 **`Components`** 와 **`Profiler`** 라는 새로운 탭이 생긴 것을 볼 수 있습니다. (만약 보이지 않는다면 `>>` 아이콘을 클릭하여 찾아보세요.)

#### Components 탭

`Components` 탭은 React 애플리케이션을 디버깅할 때 가장 많이 사용하게 될 핵심 기능입니다.

*   **① 컴포넌트 트리**: 현재 페이지를 구성하는 컴포넌트들의 계층 구조를 보여줍니다. HTML의 DOM 트리와 비슷하지만, 우리가 만든 컴포넌트 단위로 표시됩니다. 여기서 특정 컴포넌트를 클릭하면 해당 컴포넌트의 상세 정보를 볼 수 있습니다.
*   **② Props & State**: 왼쪽에서 선택한 컴포넌트가 가지고 있는 `props`와 `state`의 목록과 현재 값을 보여줍니다. **이곳에서 직접 값을 수정해볼 수도 있습니다!** 값을 바꾸면 화면에 즉시 반영되기 때문에, 다양한 상황을 빠르게 테스트해볼 수 있습니다.

#### Profiler 탭

`Profiler` 탭은 애플리케이션의 렌더링 성능을 분석하고 최적화할 때 사용하는 고급 도구입니다. 지금 당장 사용하지는 않겠지만, 이런 도구가 있다는 것을 알아두면 나중에 복잡한 애플리케이션을 만들 때 도움이 될 것입니다.

## 2. Hot Reloading (Fast Refresh)

**Hot Reloading**은 코드를 수정하고 저장할 때마다, 브라우저 전체를 새로고침하지 않고 **변경된 부분만 실시간으로 화면에 반영**해주는 놀라운 기능입니다. Vite에서는 이 기능을 **Fast Refresh**라고 부르며, 기본적으로 활성화되어 있습니다.

예를 들어, 카운터 앱의 숫자를 5까지 올린 상태에서, 버튼의 텍스트를 '증가'에서 '카운트 올리기'로 수정하고 저장했다고 가정해봅시다.

*   **Hot Reloading이 없다면?**: 페이지 전체가 새로고침되면서 카운터 숫자가 초기값인 0으로 돌아갑니다. 다시 5까지 버튼을 눌러야 테스트를 이어갈 수 있습니다.
*   **Hot Reloading이 있다면?**: 페이지는 새로고침되지 않고, 버튼의 텍스트만 '카운트 올리기'로 싹 바뀝니다. 카운터 숫자는 여전히 5를 유지하고 있습니다.

> [!IMPORTANT]
> **Hot Reloading의 장점**
> *   **개발 속도 향상**: 저장과 동시에 변경 사항을 즉시 확인할 수 있어 개발 흐름이 끊기지 않습니다.
> *   **State 유지**: 컴포넌트의 현재 상태(State)가 그대로 유지된 채로 UI만 업데이트됩니다. 이는 복잡한 상호작용을 테스트할 때 매우 큰 편리함을 제공합니다.

우리가 `npm run dev` 명령어로 개발 서버를 실행하면, Vite가 파일의 변경을 감지하고 Hot Reloading을 자동으로 처리해줍니다. 우리는 그저 코드를 작성하고 저장하기만 하면 됩니다.

React Developer Tools와 Hot Reloading은 현대 React 개발 환경의 필수 요소입니다. 이 도구들을 적극적으로 활용하여 더 빠르고 효율적으로 개발하는 습관을 들이는 것이 좋습니다.

이제 React의 기본적인 이론 학습을 마쳤습니다. 다음 시간부터는 배운 내용을 바탕으로 간단한 실습 예제들을 만들어 보겠습니다.

---

- [목차로 돌아가기](README.md)
- [이전 강의로 이동](17-State-and-UseState-Hook.md)
- [다음 강의로 이동](Lab3-Counter-Component.md)
