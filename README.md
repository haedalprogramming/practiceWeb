# practiceWeb
웹 프론트엔드를 배워요

## 2025 여름 경북소프트웨어마이스터고 기술교육

**교과목 3 (웹 프론트엔드 과정)** ― 총 8 일, 하루 9 시간 (09:00 – 18:00 / 점심 12:00 – 13:00)

> **학습 목표**
>
> 1. 최신 JavaScript(ES6 +) 문법과 모듈 시스템을 익혀 브라우저·Node 환경 모두에서 활용한다.
> 2. React + Hooks 기반 싱글페이지 애플리케이션(SPA)을 설계·구현한다.
> 3. GitHub Flow(Branch → PR → Review → Merge)를 적용해 팀으로 협업한다.
> 4. Notion·Velog를 활용해 개발 로그와 포트폴리오를 지속적으로 관리한다.
> 5. REST API 연동, 상태 관리, 테스트, CI/CD·배포까지 **전체 프론트엔드 워크플로**를 경험한다.
> 6. Demo Day에서 **실제로 동작하는 웹앱 + 프로젝트 스토리**를 발표한다.

---

## 목차 & 세부 커리큘럼

| Day                                   | 오전 세션 (09:00-12:00)                                    | 오후 세션 (13:00-15:30)                                                           | 심화/실습 (15:30-18:00)                                             | 일일 산출물‧점검                                              |
| ------------------------------------- | ------------------------------------------------------ | ----------------------------------------------------------------------------- | --------------------------------------------------------------- | ------------------------------------------------------ |
| **1. 개발 환경 & GitHub 세팅**              | • [웹 기본 개념](day1/01-Introducing-to-Web.md)<br>• [프론트엔드 로드맵](day1/08-Introducing-Frontend.md)<br>• [개발 환경 설정](day1/09-Setup-Development-Environment.md) | • [Git](day1/04-Git-Fundamentals.md) & [GitHub](day1/05-GitHub.md)<br>• [Markdown](day1/10-Markdown.md) | • [[실습] GitHub 협업](day1/Lab1-GitHub-collaboration.md)         | ✔ GitHub repo/프로젝트 보드 개설<br>✔ Notion 스페이스·Velog 블로그 개설 |
| **2. Modern JavaScript Fundamentals** | • [JS 소개](day2/01-Introducing-JavaScript.md)<br>• [기본 문법](day2/02-ES6-Basic.md), [변수](day2/03-ES6-Variables-and-Scoping.md), [자료형](day2/04-ES6-Data-Types-and-Operators.md), [제어문](day2/05-ES6-Conditional-Statements-and-Loops.md), [함수](day2/06-ES6-Functions.md) | • [배열과 객체](day2/07-ES6-Arrays-and-Objects.md)<br>• [클래스](day2/08-ES6-Classes.md)와 [모듈](day2/09-ES6-Modules.md)<br>• [비동기 프로그래밍](day2/10-ES6-Async-Patterns.md) | • [브라우저/Node.js](day2/12-Browser-JavaScript.md) ([DOM](day2/13-Basic-DOM-Manipulation.md), [Event](day2/14-Basic-Event-Handling.md))<br>• [[실습] 간단한 계산기 만들기](day2/Lab1-Simple-Calculator.md)<br>• [[실습] Todo List](day2/Lab2-Todo-List.md)<br>• [[실습] 성적 처리 프로그램](day2/Lab3-Grade-Processor.md)<br>• [[실습] 간단한 웹 서버 만들기](day2/Lab4-Simple-Web-Server.md) | ✔ Todo CLI 과제 PR<br>✔ PR Merge + 리뷰 1건                 |
| **3. HTML, CSS, React 기초**             | • [HTML 기본 구조](day3/01-What-is-HTML.md)와 [주요 태그](day3/02-Common-HTML-Tags.md)<br>• [CSS 기본 문법](day3/04-Getting-Started-with-CSS.md)과 [선택자](day3/05-CSS-Selectors.md)<br>• [박스 모델](day3/08-CSS-Box-Model.md)과 [Flexbox](day3/09-Layout-with-Flexbox.md) | • [React 소개](day3/13-Introducing-React-and-Setup.md) 및 개발 환경 설정<br>• [JSX](day3/14-Understanding-JSX.md), [컴포넌트](day3/15-Components.md), [Props](day3/16-Props.md), [useState Hook](day3/17-State-and-UseState-Hook.md)<br>• [React DevTools](day3/18-React-DevTools-and-Hot-Reloading.md) | • [[실습] 자기소개 페이지 HTML](day3/Lab1-Simple-Profile-HTML.md)<br>• [[실습] 자기소개 페이지 CSS 스타일링](day3/Lab2-Styling-Profile-CSS.md)<br>• [[실습] Counter, Toggle, Input Form 컴포넌트](day3/Lab3-Counter-Component.md) | ✔ HTML/CSS 포트폴리오 페이지 완성<br>✔ React 기능별 커밋 3건<br>✔ Velog 글: "웹 개발 첫걸음" |
| **4. 라우팅·컴포지션·스타일링**                  | • [React Router v6 소개](day4/01-Introducing-React-Router.md)<br>• [Route, Link, Outlet 사용하기](day4/02-Using-Route-Link-Outlet.md)<br>• [컴포넌트 설계 패턴](day4/03-Component-Design-Patterns.md) (Atomic Design) | • [React 스타일링](day4/04-React-Styling.md)<br>• CSS Module, Tailwind CSS, Styled-Components 특징 및 적용 방법<br>                                               | • [[실습] React로 자기소개 페이지 만들기](day4/Lab1-Profile-Page-React.md)<br>• 원자-분자-유기체 단위 컴포넌트 설계<br>• 스타일링 방식 선택 및 적용                                          | ✔ 다중 페이지 SPA 완성<br>✔ 컴포넌트 계층 구조 설계서<br>✔ 선택한 스타일링 방식 적용          |
| **5. Hooks 심화 & API 연동**              | • [Custom Hooks](day5/01-Custom-Hooks.md), [useEffect 심화](day5/02-useEffect-Deep-Dive.md)<br>• [useRef](day5/03-useRef-Hook.md), [useContext](day5/04-useContext-Hook.md) | • [API 연동](day5/05-API-Integration.md)과 [REST API](day5/06-REST-API.md)<br>• [API 모킹 (MSW)](day5/07-API-Mocking-MSW.md) | • [[실습] `useLocalStorage` Hook](day5/Lab1-useLocalStorage-Hook.md)<br>• [[실습] 테마 전환 기능](day5/Lab2-Theme-Switcher.md)<br>• [[실습] `useRef` 타이머](day5/Lab3-Timer-with-useRef.md)<br>• [[실습] MSW 블로그](day5/Lab4-Blog-with-MSW.md) | ✔ Custom Hook 1개 이상 구현<br>✔ API 연동 기능 구현              |
| **6. 상태 관리·테스트 & 최적화**                | • [TanStack Query](day6/01-TanStack-Query.md)<br>• [상태 관리 라이브러리 비교](day6/03-State-Management-Libs.md)<br>• [Zustand](day6/04-Getting-Started-Zustand.md) | • [에러 처리](day6/05-Error-Handling.md)<br>• [렌더링 이해 및 최적화](day6/06-Understanding-Rendering.md)<br>• [테스트](day6/09-Intro-to-Testing.md) | • [Tailwind CSS](day6/10-Intro-to-Tailwind-CSS.md)<br>• [폼 관리](day6/11-Advanced-Form-Handling.md)<br>• [웹 접근성](day6/12-Web-Accessibility.md)<br>• [[실습] TanStack Query 리팩토링](day6/Lab1-Refactor-to-TanStack-Query.md)<br>• [[실습] Zustand 전역 상태 관리](day6/Lab2-Global-State-with-Zustand.md)<br>• [[실습] RHF 회원가입 폼](day6/Lab3-Signup-Form-with-RHF.md) | ✔ 테스트 통과율 80 %↑<br>✔ Lighthouse PWA Score 80↑          |
| **7. AI 기반 개발 & 배포**                  | • [AI 코딩 어시스턴트 소개](day7/01-Intro-to-AI-Coding-Assistants.md)<br>• [AI 활용 기획/코드 초안 작성](day7/02-AI-for-Planning-and-Drafting.md)<br>• [AI 활용 코드 리뷰/테스트/디버깅](day7/03-AI-for-Review-and-Testing.md) | • [GitHub Actions CI/CD](day7/04-GitHub-Actions-CI-CD.md)<br>• [GitHub Pages 배포](day7/05-GitHub-Pages-Deployment.md)<br>• [환경변수 및 트러블슈팅](day7/06-Environment-Variables-and-Troubleshooting.md) | • [[실습] AI와 함께하는 코딩](day7/Lab1-AI-Coding-Assistant.md)<br>• [[실습] CI/CD 자동화](day7/Lab2-CI-CD-Automation.md) | ✔ LIVE URL · 커밋 태그 v1.0<br>✔ 블로그 글 초안                  |
| **8. Demo Day & 회고**                  | • [개인 포트폴리오 기획](day8/01-Portfolio-Planning.md)<br>• [핵심 기능 개발](day8/02-Core-Feature-Development.md) | • [API 연동 및 상태 관리](day8/03-API-Integration-and-State-Management.md)<br>• [발표 자료 준비](day8/04-Presentation-Preparation.md) | • [최종 테스트 및 배포](day8/05-Final-Check-and-Deployment.md)<br>• [데모 및 동료 리뷰](day8/06-Demo-and-Peer-Review.md)<br>• [과정 전체 회고](day8/07-Course-Retrospective.md) | ✔ 발표 자료 & 수료증<br>✔ 최종 포트폴리오 완성                         |

---

### 평가 방법 (총 100 점)

| 항목                  | 비율   | 평가지표                         |
| ------------------- | ---- | ---------------------------- |
| 최종 웹앱 결과물           | 40 점 | 기능 완성도, UI/UX, 반응형, 성능       |
| 코드 품질 & GitHub 협업   | 20 점 | 커밋 메시지, PR 리뷰, Branch 전략     |
| 테스트·접근성·성능 지표       | 15 점 | 테스트 커버리지, Lighthouse·WCAG 준수 |
| 포트폴리오(Notion·Velog) | 15 점 | 과정 기록, 가독성, 시각적 구성           |
| 발표 & 커뮤니케이션         | 10 점 | 데모 안정성, 논리 전개, 질의응답          |

---

### 운영 포인트

* **Daily Stand-up (09:00) / Retrospective (17:45)**: GitHub Issue 상태 업데이트
* **Mentor Clinic**: 매일 14:30-15:30, 코드 리뷰 · 버그 헬프
* **Checkpoint Quiz**: Day 2·4·6 오전, Kahoot! 10문항
* **Boilerplate & 리소스 킷**: ESLint/Prettier 설정, Vite React TS 템플릿, Tailwind config, 테스트 스니펫

---

본 커리큘럼은 \*\*“설계-구현-검증-배포”\*\*까지 프론트엔드 전 과정을 8일 안에 체험하도록 설계했습니다.
