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
| **2. Modern JavaScript Fundamentals** | • [JS 소개](day2/01-Introducing-JavaScript.md)<br>• [기본 문법](day2/02-ES6-Basic.md), [변수](day2/03-ES6-Variables-and-Scoping.md), [자료형](day2/04-ES6-Data-Types-and-Operators.md), [제어문](day2/05-ES6-Conditional-Statements-and-Loops.md), [함수](day2/06-ES6-Functions.md) | • [배열과 객체](day2/07-ES6-Arrays-and-Objects.md)<br>• [클래스](day2/08-ES6-Classes.md)와 [모듈](day2/09-ES6-Modules.md)<br>• [비동기 프로그래밍](day2/10-ES6-Async-Patterns.md) | • [브라우저/Node.js](day2/12-Browser-JavaScript.md) ([DOM](day2/13-Basic-DOM-Manipulation.md), [Event](day2/14-Basic-Event-Handling.md))<br>• [[실습] Todo List](day2/Lab1-Todo-List.md)<br>• [[실습] 성적 처리 프로그램](day2/Lab2-Grade-Processor.md)<br>• [[실습] 간단한 웹 서버 만들기](day2/Lab3-Simple-Web-Server.md) | ✔ Todo CLI 과제 PR<br>✔ PR Merge + 리뷰 1건                 |
| **3. React 기초**                       | • Vite + React 프로젝트 생성<br>• JSX·컴포넌트·Props·useState    | • React DevTools, Hot Reloading                                               | • 과제: Counter, Toggle, Input Form 3종 컴포넌트                       | ✔ 기능별 커밋 3건<br>✔ Velog 글: “React 첫걸음”                  |
| **4. 라우팅·컴포지션·스타일링**                  | • React Router v6: Route, Link, Outlet                 | • 컴포넌트 설계 패턴(원자·분자·유기체)                                                       | • 스타일 옵션: CSS Module · Tailwind CSS · Styled-Components 중 1택 적용 | ✔ 팀 프로젝트 폴더 구조 & 디자인 시안<br>✔ Figma ↔ 코드 매핑 계획          |
| **5. API 연동 & 커스텀 Hook**              | • fetch·axios 패턴, REST 규약                              | • useEffect로 데이터 패칭, Error boundary                                           | • Custom Hook으로 로딩·캐시 관리                                        | ✔ 백엔드 Mock Server 연결<br>✔ API 응답 화면 2개 구현              |
| **6. 상태 관리·테스트 & 최적화**                | • Context API vs Redux Toolkit vs Zustand 개요           | • React Query 활용 데이터 캐싱                                                       | • Jest‧RTL 유닛·통합 테스트 작성,<br>• Lighthouse 성능·접근성 체크              | ✔ 테스트 통과율 80 %↑<br>✔ Lighthouse PWA Score 80↑          |
| **7. 배포 & 포트폴리오 정리**                  | • GitHub Actions로 CI 파이프라인 구축                          | • Netlify / Vercel 자동 배포 & 환경변수                                               | • Notion Case Study·Velog 후기 작성                                 | ✔ LIVE URL · 커밋 태그 v1.0<br>✔ 블로그 글 초안                  |
| **8. Demo Day & 회고**                  | • 발표 구조(문제-해결-배운 점)·데모 리허설                             | • 팀별 발표 & Q \&A (15 min/team)                                                 | • 360° 회고 & 개선 사항 공유                                            | ✔ 발표 자료 & 수료증<br>✔ 최종 포트폴리오 완성                         |

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
