# Day 6: 상태 관리, 최적화, 테스트, 그리고 심화 주제

이번 강의(day6)에서는 React 애플리케이션의 품질을 한 단계 끌어올리는 고급 주제들을 다룹니다. 먼저 React의 핵심 동작 원리인 **렌더링** 과정을 이해하고, 이를 바탕으로 복잡한 데이터를 효율적으로 관리하기 위한 <strong>서버 상태 관리(TanStack Query)</strong>와 **클라이언트 상태 관리(Zustand)** 기법을 배웁니다. 또한, 애플리케이션의 안정성을 높이는 **에러 처리**, **렌더링 최적화**, **테스트** 방법을 학습합니다. 마지막으로 **Tailwind CSS**, **React Hook Form**, **웹 접근성**과 같은 심화 주제를 통해 실무 역량을 한층 더 강화합니다.

## 학습 목표

*   React의 렌더링 과정을 이해하고, 컴포넌트가 어떤 조건에서 리렌더링(re-rendering)되는지 설명할 수 있습니다.
*   TanStack Query를 사용하여 서버 상태를 효율적으로 관리하고, API 연동 코드를 리팩토링할 수 있습니다.
*   Zustand를 사용하여 여러 컴포넌트가 공유하는 전역 상태를 간결하게 관리할 수 있습니다.
*   Error Boundary를 사용하여 특정 컴포넌트의 에러가 전체 애플리케이션에 영향을 주지 않도록 격리할 수 있습니다.
*   `React.memo`와 `useCallback`을 사용하여 불필요한 렌더링을 방지하고 성능을 최적화할 수 있습니다.
*   React 개발자 도구를 활용하여 컴포넌트 렌더링 성능을 분석하고 병목 지점을 찾을 수 있습니다.
*   Jest와 React Testing Library의 역할을 이해하고 간단한 컴포넌트 테스트를 작성할 수 있습니다.
*   Tailwind CSS를 사용하여 유틸리티 우선 방식으로 UI를 빠르게 개발할 수 있습니다.
*   React Hook Form을 사용하여 복잡한 폼 로직을 효율적으로 관리할 수 있습니다.
*   웹 접근성의 중요성을 이해하고 기본적인 준수 방법을 적용할 수 있습니다.

## 목차

### 1) 서버 상태 관리
- [01. TanStack Query (React Query) 소개](01-TanStack-Query.md)
- [02. TanStack Query 주요 기능 활용](02-TanStack-Query-Features.md)

### 2) 클라이언트 상태 관리
- [03. 상태 관리 라이브러리 비교 (Context vs Zustand vs Redux)](03-State-Management-Libs.md)
- [04. Zustand 시작하기](04-Getting-Started-Zustand.md)

### 3) 애플리케이션 안정성 및 최적화
- [05. React 에러 처리 (Error Boundaries)](05-Error-Handling.md)
- [06. React의 렌더링 이해하기](06-Understanding-Rendering.md)
- [07. 렌더링 최적화 (React.memo, useCallback)](07-Rendering-Optimization.md)
- [08. React 개발자 도구 심화](08-React-DevTools-Advanced.md)

### 4) 테스트
- [09. Jest와 React Testing Library 소개](09-Intro-to-Testing.md)

### 5) 심화 주제
- [10. Tailwind CSS 소개](10-Intro-to-Tailwind-CSS.md)
- [11. React Hook Form을 이용한 폼 관리](11-Advanced-Form-Handling.md)
- [12. 웹 접근성(a11y) 기초](12-Web-Accessibility.md)

### 6) 실습
- [Lab1. TanStack Query로 API 연동 리팩토링](Lab1-Refactor-to-TanStack-Query.md)
- [Lab2. Zustand로 전역 상태 관리하기](Lab2-Global-State-with-Zustand.md)
- [Lab3. React Hook Form으로 회원가입 폼 만들기](Lab3-Signup-Form-with-RHF.md)

---

| [메인으로 돌아가기](../README.md) | [Day 5로 돌아가기](../day5/README.md) | [Day 7으로 이동하기](../day7/README.md) |
| :--- | :--- | :--- |
