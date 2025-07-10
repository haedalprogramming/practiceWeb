# Day 5: React 심화 - Hooks와 비동기 통신

> [!NOTE]
> 5일차 학습에 오신 것을 환영합니다! 오늘은 React의 강력한 기능인 **Hooks**에 대해 더 깊이 알아보고, 실제 웹 애플리케이션의 핵심인 **서버와 통신하는 방법**을 배웁니다. `useEffect`, `useRef`, `useContext`와 같은 필수적인 Hook들의 작동 원리를 이해하고, 이를 활용해 비동기 API를 호출하고 상태를 관리하는 방법을 익힙니다.

---

## 📚 학습 목표

- **Custom Hooks**를 만들어 재사용 가능한 로직을 캡슐화할 수 있습니다.
- `useEffect` Hook의 의존성 배열을 이해하고, 컴포넌트의 생명주기와 관련된 부수 효과(side effects)를 제어할 수 있습니다.
- `useRef`를 사용하여 DOM 요소에 직접 접근하거나 리렌더링을 유발하지 않는 값을 저장할 수 있습니다.
- `useContext`를 사용하여 Props drilling 없이 컴포넌트 트리 전역에 데이터를 공유할 수 있습니다.
- `fetch` API를 사용하여 **REST API**와 비동기적으로 통신하고, 응답 데이터를 상태에 저장할 수 있습니다.
- **MSW(Mock Service Worker)**를 이용해 백엔드 API를 모킹(mocking)하여 프론트엔드 개발을 독립적으로 수행할 수 있습니다.

---

## 📜 강의 목록

### 1. React Hooks 심화
- [**1) Custom Hooks**](./01-Custom-Hooks.md): 반복되는 로직을 나만의 Hook으로 만들기
- [**2) `useEffect` 심층 탐구**](./02-useEffect-Deep-Dive.md): 의존성 배열과 클린업 함수 완전 정복
- [**3) `useRef` Hook**](./03-useRef-Hook.md): 렌더링과 무관한 값 저장 및 DOM 직접 제어
- [**4) `useContext` Hook**](./04-useContext-Hook.md): Props drilling 없이 상태 전역으로 공유하기

### 2. API 연동과 비동기 처리
- [**5) API 연동의 모든 것**](./05-API-Integration.md): 클라이언트-서버 데이터 통신의 기본
- [**6) REST API란?**](./06-REST-API.md): 웹 통신의 표준 아키텍처 이해하기
- [**7) API 모킹과 MSW**](./07-API-Mocking-MSW.md): 백엔드 없이 프론트엔드 개발하기

### 3. 실습 과제
- [**실습 1) `useLocalStorage` Custom Hook 만들기**](./Lab1-useLocalStorage-Hook.md): 새로고침해도 데이터가 유지되는 나만의 Hook 구현
- [**실습 2) `useContext`로 테마 전환 기능 만들기**](./Lab2-Theme-Switcher.md): 전역 상태 관리를 통한 다크/라이트 모드 구현
- [**실습 3) `useRef`로 정확한 타이머 만들기**](./Lab3-Timer-with-useRef.md): 리렌더링 없는 값 저장을 활용한 타이머 제어
- [**실습 4) MSW로 블로그 앱 구현하기**](./Lab4-Blog-with-MSW.md): 가짜 API 서버와 `fetch`를 이용한 종합 비동기 통신 실습

---

| [메인으로 돌아가기](../../README.md) | [Day 4로 돌아가기](../day4/README.md) | [Day 6으로 이동하기](../day6/README.md) |
| :--- | :--- | :--- |
