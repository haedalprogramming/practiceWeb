# Day 6: 상태 관리, 최적화, 테스트

이번 강의(day6)에서는 React 애플리케이션의 품질을 한 단계 끌어올리는 고급 주제들을 다룹니다. 먼저 React의 핵심 동작 원리인 **렌더링** 과정을 이해하고, 이를 바탕으로 복잡한 데이터를 효율적으로 관리하기 위한 **서버 상태 관리(TanStack Query)**와 **클라이언트 상태 관리(Zustand)** 기법을 배웁니다. 또한, 애플리케이션의 안정성을 높이는 **에러 처리**와 사용자 경험을 개선하는 **렌더링 최적화** 방법을 학습합니다. 마지막으로, 코드의 신뢰도를 보장하는 **테스트**의 기본 개념을 익힙니다.

## 학습 목표

*   React의 렌더링 과정을 이해하고, 컴포넌트가 어떤 조건에서 리렌더링(re-rendering)되는지 설명할 수 있습니다.
*   TanStack Query를 사용하여 서버 상태를 효율적으로 관리하고, API 연동 코드를 리팩토링할 수 있습니다.
*   Zustand를 사용하여 여러 컴포넌트가 공유하는 전역 상태를 간결하게 관리할 수 있습니다.
*   Error Boundary를 사용하여 특정 컴포넌트의 에러가 전체 애플리케이션에 영향을 주지 않도록 격리할 수 있습니다.
*   `React.memo`와 `useCallback`을 사용하여 불필요한 렌더링을 방지하고 성능을 최적화할 수 있습니다.
*   React 개발자 도구를 활용하여 컴포넌트 렌더링 성능을 분석하고 병목 지점을 찾을 수 있습니다.
*   Jest와 React Testing Library의 역할을 이해하고 간단한 컴포넌트 테스트를 작성할 수 있습니다.

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

### 5) 실습
- [Lab1. TanStack Query로 API 연동 리팩토링](Lab1-Refactor-to-TanStack-Query.md)
- [Lab2. Zustand로 전역 상태 관리하기](Lab2-Global-State-with-Zustand.md)

---

- [목차로 돌아가기](../README.md)
- [이전 강의로 이동](../day5/README.md)
- [다음 강의로 이동](../day7/README.md)
