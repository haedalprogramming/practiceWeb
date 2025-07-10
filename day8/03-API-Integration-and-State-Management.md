# 03. API 연동 및 상태 관리 (선택)

핵심 컴포넌트 개발이 완료되었다면, 이제 포트폴리오에 생동감을 불어넣을 차례입니다. 외부 API를 연동하여 동적인 콘텐츠를 보여주거나, 상태 관리 라이브러리를 도입하여 복잡한 데이터 흐름을 효율적으로 제어할 수 있습니다.

> [!NOTE]
> 이 단계는 선택 사항입니다. 하지만 API 연동 및 상태 관리 능력은 프론트엔드 개발자의 중요한 역량이므로, 도전해보는 것을 적극 권장합니다.

## 3-1) 왜 API 연동이 필요한가?

정적인 데이터만으로 구성된 포트폴리오는 시간이 지나면 정보가 낡게 되고, 업데이트하기 위해 매번 코드를 수정하고 다시 배포해야 하는 번거로움이 있습니다. API를 연동하면 이러한 문제를 해결할 수 있습니다.

-   **콘텐츠 자동 업데이트:** Velog, Tistory 등 블로그 API를 연동하여 최신 글 목록을 자동으로 보여줄 수 있습니다.
-   **활동 내역 증명:** GitHub API를 연동하여 자신의 공개 저장소(repository) 목록이나 잔디(contribution graph)를 보여줌으로써 꾸준한 개발 활동을 증명할 수 있습니다.
-   **다양한 정보 제공:** 날씨 API, 뉴스 API 등을 활용하여 포트폴리오에 재미와 유용한 정보를 더할 수 있습니다.

## 3-2) API 데이터 연동하기

Day 5에서 배운 `fetch` API와 `useEffect` Hook을 사용하여 컴포넌트가 마운트될 때 외부 API로부터 데이터를 가져올 수 있습니다.

**예시: GitHub 저장소 목록 가져오기**

GitHub에서 제공하는 REST API를 사용하면 특정 유저의 공개 저장소 목록을 쉽게 가져올 수 있습니다. (`https://api.github.com/users/YOUR_USERNAME/repos`)

```tsx
// src/pages/ProjectsPage.tsx
import { useEffect, useState } from 'react';

interface Repo {
  id: number;
  name: string;
  html_url: string;
  description: string;
}

const ProjectsPage = () => {
  const [repos, setRepos] = useState<Repo[]>([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    const fetchRepos = async () => {
      try {
        const response = await fetch('https://api.github.com/users/YOUR_USERNAME/repos');
        const data = await response.json();
        setRepos(data);
      } catch (error) {
        console.error("Error fetching repos:", error);
      } finally {
        setLoading(false);
      }
    };

    fetchRepos();
  }, []);

  if (loading) return <div>로딩 중...</div>;

  return (
    <div>
      <h2>My GitHub Projects</h2>
      {repos.map(repo => (
        <div key={repo.id}>
          <h3><a href={repo.html_url}>{repo.name}</a></h3>
          <p>{repo.description}</p>
        </div>
      ))}
    </div>
  );
};

export default ProjectsPage;
```

## 3-3) 상태 관리 라이브러리 활용 (심화)

여러 컴포넌트에서 동일한 데이터를 사용하거나, 로딩 및 에러 상태를 보다 체계적으로 관리하고 싶다면 Day 6에서 배운 상태 관리 라이브러리를 도입하는 것을 고려해볼 수 있습니다.

-   **TanStack Query:** 서버 상태(API 데이터) 관리에 특화된 라이브러리입니다. 데이터 캐싱, 재요청, 로딩/에러 상태 처리 등을 매우 간결한 코드로 구현할 수 있어 API 연동이 많은 프로젝트에 적합합니다.
-   **Zustand:** 클라이언트 상태(예: 다크 모드, 모달 상태)를 전역적으로 관리하고 싶을 때 유용합니다. 여러 컴포넌트가 공유해야 하는 상태를 손쉽게 관리할 수 있습니다.

> [!TIP]
> 포트폴리오 프로젝트에 한 가지 이상의 상태 관리 라이브러리를 적용해보는 것은 자신의 기술적 깊이를 보여주는 좋은 방법이 될 수 있습니다.

---

- [목차로 돌아가기](../README.md)
- [이전 강의로 돌아가기](./02-Core-Feature-Development.md)
- [다음 강의로 이동하기](./04-Presentation-Preparation.md)