# 02. 핵심 기능 개발

탄탄한 기획이 완료되었다면, 이제 본격적으로 코드를 작성하여 아이디어를 현실로 만들 차례입니다. 이 단계에서는 Vite를 사용하여 React 프로젝트를 생성하고, 기획 단계에서 설계한 와이어프레임을 바탕으로 핵심 컴포넌트들을 개발합니다.

## 2-1) 프로젝트 생성 및 초기 설정

먼저, 여러분의 포트폴리오 프로젝트를 담을 새로운 Vite + React 프로젝트를 생성합니다.

```bash
npm create vite@latest my-portfolio -- --template react-ts
cd my-portfolio
npm install
```

프로젝트가 생성되면, 사용할 스타일링 라이브러리(예: Styled-components, Tailwind CSS)를 설치하고 초기 설정을 진행합니다. Day 4, Day 6에서 배운 내용을 참고하여 설정을 완료하세요.

> [!CAUTION]
> 초기 설정 과정(ESLint, Prettier, 스타일링 라이브러리 등)은 다소 번거로울 수 있지만, 프로젝트의 일관성과 안정성을 위해 매우 중요합니다. 빠뜨리지 말고 꼼꼼하게 설정해주세요.

## 2-2) 폴더 구조 설계

프로젝트의 규모가 커지더라도 유지보수하기 쉽도록, 일관된 규칙에 따라 폴더 구조를 설계하는 것이 좋습니다. Day 4에서 배운 **Atomic Design 패턴**을 적용하여 컴포넌트를 관리하는 것을 추천합니다.

```
src
├── apis
├── assets
├── components
│   ├── atoms       # 버튼, 인풋 등 가장 작은 단위
│   ├── molecules   # 검색창 등 원자들의 조합
│   ├── organisms   # 헤더, 푸터 등 분자들의 조합
│   └── templates   # 페이지 레이아웃
├── constants
├── hooks
├── pages         # 실제 화면을 구성하는 페이지
├── store
├── styles
└── utils
```

## 2-3) 핵심 컴포넌트 개발

이제 기획 단계에서 정의한 핵심 기능들을 컴포넌트 단위로 개발하기 시작합니다. 와이어프레임을 참고하여 `atoms`부터 `organisms` 순서로, 작은 단위부터 조합하여 큰 단위를 만들어나가는 **상향식(Bottom-up)** 접근 방식을 추천합니다.

### 1) Atoms & Molecules 개발

-   `Button`, `Input`, `Icon`, `ProfileImage` 등 재사용 가능한 가장 작은 UI 요소들을 먼저 개발합니다.
-   이어서 `SearchBar` (Input + Button), `NavLinks` (Link들의 집합) 등 원자들을 조합하여 분자 컴포넌트를 만듭니다.

### 2) Organisms 개발

-   분자 컴포넌트들을 조합하여 `Header`, `Footer`, `ProjectCard`, `SkillList` 와 같은 더 복잡한 유기체 컴포넌트를 개발합니다.
-   이 단계에서는 `useState`를 사용하여 컴포넌트의 상태(예: 모달의 열림/닫힘 상태)를 관리하거나, `props`를 통해 데이터를 전달받아 UI를 렌더링하는 로직이 포함됩니다.

**예시: `ProjectCard` 컴포넌트**

```tsx
// src/components/organisms/ProjectCard.tsx

interface ProjectCardProps {
  imageUrl: string;
  title: string;
  description: string;
  projectUrl: string;
  techStack: string[];
}

const ProjectCard = ({ 
  imageUrl, 
  title, 
  description, 
  projectUrl, 
  techStack 
}: ProjectCardProps) => {
  return (
    // 카드 UI 구조
    <div>
      <img src={imageUrl} alt={`${title} 이미지`} />
      <h3>{title}</h3>
      <p>{description}</p>
      <div>
        {techStack.map(tech => <span key={tech}>{tech}</span>)}
      </div>
      <a href={projectUrl} target="_blank" rel="noopener noreferrer">프로젝트 보기</a>
    </div>
  );
};

export default ProjectCard;
```

---

- [목차로 돌아가기](../README.md)
- [이전 강의로 돌아가기](./01-Portfolio-Planning.md)
- [다음 강의로 이동하기](./03-API-Integration-and-State-Management.md)