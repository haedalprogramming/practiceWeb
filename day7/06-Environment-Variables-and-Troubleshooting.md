# 06. 환경 변수 관리 및 배포 트러블슈팅

지금까지 우리는 React 애플리케이션을 만들고, GitHub Pages, Netlify, Vercel 등 다양한 플랫폼을 통해 성공적으로 배포하는 방법을 배웠습니다. 하지만 실제 프로젝트를 진행하다 보면 단순히 코드를 빌드하고 배포하는 것 외에 추가적으로 고려해야 할 상황들이 발생합니다.

대표적으로 **API 키**와 같이 외부에 노출되어서는 안 되는 민감한 정보를 안전하게 관리해야 하는 경우, 또는 배포 과정에서 예상치 못한 오류가 발생하여 원인을 찾아 해결해야 하는 경우가 있습니다.

이번 시간에는 이 두 가지 중요한 주제, 즉 **환경 변수 관리**와 **배포 트러블슈팅**에 대해 깊이 있게 알아보겠습니다.

## 4-1) 환경 변수(Environment Variables)란?

**환경 변수**는 애플리케이션이 실행되는 환경(예: 개발자의 로컬 컴퓨터, Netlify/Vercel과 같은 배포 서버 등)에 따라 달라져야 하는 값들을 저장하기 위한 변수입니다.

가장 대표적인 예시가 바로 외부 API를 사용할 때 필요한 **API 키(Key)**입니다. API 키는 보통 유료이거나 사용량에 제한이 있어, 절대로 GitHub과 같은 공개된 장소에 노출되어서는 안 됩니다. 만약 API 키가 포함된 코드를 그대로 GitHub 리포지토리에 `push`한다면, 누구나 그 키를 가져다 무단으로 사용할 수 있는 끔찍한 상황이 발생할 수 있습니다.

이러한 문제를 해결하기 위해, 우리는 민감한 정보들을 코드에서 분리하여 환경 변수로 관리해야 합니다.

### Vite에서 환경 변수 사용하기

Vite로 만든 React 프로젝트에서는 아주 간단한 규칙만으로 환경 변수를 사용할 수 있습니다.

1.  **`.env` 파일 생성:** 프로젝트의 루트 디렉토리(가장 상위 폴더)에 `.env`라는 이름의 파일을 생성합니다.

    > [!IMPORTANT]
    > `.env` 파일은 민감한 정보를 담고 있으므로, 반드시 `.gitignore` 파일에 `.env*` 나 `/`.env` 한 줄을 추가하여 Git이 이 파일을 추적하지 않도록 설정해야 합니다. Vite는 프로젝트 생성 시 기본적으로 이 설정을 `.gitignore` 파일에 포함하고 있습니다.

2.  **환경 변수 정의:** `.env` 파일 안에 `VITE_`라는 접두사로 시작하는 변수를 만듭니다. 이 접두사는 Vite의 규칙이며, 클라이언트 측 코드에서 접근할 수 있도록 하려면 반드시 이 접두사를 사용해야 합니다.

    **`.env` 파일 내용 예시:**
    ```
    VITE_API_KEY=여기에_여러분의_API_키를_입력하세요
    VITE_API_BASE_URL=https://api.example.com
    ```

3.  **코드에서 사용하기:** 이제 애플리케이션 코드 내에서 `import.meta.env.VITE_변수명` 형태로 해당 값을 불러와 사용할 수 있습니다.

    ```jsx
    const apiKey = import.meta.env.VITE_API_KEY;
    const baseUrl = import.meta.env.VITE_API_BASE_URL;

    fetch(`${baseUrl}/data?apiKey=${apiKey}`)
      .then(response => response.json())
      .then(data => console.log(data));
    ```

    이렇게 하면, 로컬 개발 환경에서는 `.env` 파일에 저장된 값을 사용하여 API를 테스트할 수 있습니다.

### 배포 플랫폼에 환경 변수 설정하기

그렇다면 Netlify나 Vercel과 같은 배포 서버는 `.env` 파일이 없는데 어떻게 이 값들을 알 수 있을까요?

정답은 **각 플랫폼의 대시보드에서 직접 환경 변수를 설정**해주는 것입니다.

-   **Netlify:** `Site settings` > `Build & deploy` > `Environment` > `Environment variables` 메뉴에서 `Key`와 `Value`를 입력하여 환경 변수를 추가할 수 있습니다.
-   **Vercel:** `Project Settings` > `Environment Variables` 메뉴에서 `Name`과 `Value`를 입력하여 환경 변수를 추가할 수 있습니다.

이렇게 플랫폼에 환경 변수를 등록해두면, 배포가 진행될 때(즉, `npm run build`가 실행될 때) 우리가 코드에 작성한 `import.meta.env.VITE_API_KEY` 부분이 플랫폼에 설정된 실제 API 키 값으로 교체되어 빌드가 완료됩니다.

이 방식을 통해 우리는 민감한 정보를 안전하게 보호하면서, 로컬 환경과 실제 배포 환경에서 각기 다른 설정값을 사용하도록 만들 수 있습니다.

## 4-2) 배포 관련 흔한 오류와 해결 방법 (Troubleshooting)

배포는 항상 순조롭게 진행되면 좋겠지만, 때로는 예상치 못한 문제들을 마주치게 됩니다. 당황하지 않고 침착하게 원인을 찾아 해결하는 것이 중요합니다.

### 1) "Page Not Found" (404 에러)

배포된 사이트에 접속했는데 "Page Not Found" 또는 "404" 오류 페이지만 덩그러니 보이는 경우입니다. 가장 흔하게 발생하는 문제이며, 원인은 다양합니다.

-   **GitHub Pages:** Vite 프로젝트의 경우, `package.json`의 `homepage` 속성 대신 `vite.config.js` 파일에서 `base` 옵션을 설정해야 합니다. `base` 값은 `/<repo-name>/` 형식이어야 합니다.

    **`vite.config.js` 설정 예시:**
    ```js
    import { defineConfig } from 'vite'
    import react from '@vitejs/plugin-react'

    // https://vitejs.dev/config/
    export default defineConfig({
      plugins: [react()],
      base: '/your-repo-name/' // 👈 여기에 리포지토리 이름을 추가합니다.
    })
    ```
    이 `base` 설정이 잘못되거나 누락되면 CSS나 JS 파일을 제대로 불러오지 못해 404 오류나 빈 화면이 발생할 수 있습니다.
-   **React Router 사용 시:** React Router와 같은 클라이언트 사이드 라우팅 라이브러리를 사용하는 경우, 사용자가 `/about`과 같은 특정 경로로 직접 접속하려고 할 때 서버가 해당 경로의 파일을 찾지 못해 404 에러를 반환할 수 있습니다.
    -   **해결 방법:** 모든 경로 요청에 대해 일단 `index.html` 파일을 먼저 보여주도록 서버 설정을 변경해야 합니다.
        -   **Netlify:** 루트 디렉토리에 `_redirects` 라는 이름의 파일을 만들고, 파일 안에 `/* /index.html 200` 한 줄을 추가하면 됩니다.
        -   **Vercel:** Vercel은 이 문제를 자동으로 처리해주므로 별도의 설정이 필요 없습니다.

### 2) 빈 화면만 보이는 경우

페이지는 열리지만 아무 내용도 보이지 않는, 말 그대로 '백지' 상태인 경우입니다. 이때는 브라우저의 개발자 도구(F12)를 열어 콘솔(Console) 탭을 확인하는 것이 가장 중요합니다.

-   **콘솔 에러 확인:** 콘솔 탭에 빨간색으로 표시되는 에러 메시지가 있는지 확인합니다.
    -   `Failed to load resource`: 이미지, CSS, JS 파일 등의 경로가 잘못되어 불러오지 못하는 경우입니다. GitHub Pages 배포의 경우 `vite.config.js`의 `base` 설정이 올바른지, 혹은 다른 파일들의 경로가 올바른지 확인해야 합니다.
    -   `Uncaught TypeError`, `ReferenceError`: 코드 자체에 문법적인 오류가 있을 가능성이 높습니다. 로컬 환경에서는 잘 동작했지만, 빌드 과정에서 문제가 발생했을 수 있습니다.
-   **네트워크 탭 확인:** 개발자 도구의 네트워크(Network) 탭에서 특정 파일을 불러오는 데 실패(상태 코드가 4xx 또는 5xx)했는지 확인할 수 있습니다.

### 3) 배포 실패 (Build Failed)

Netlify나 Vercel 대시보드에서 배포 상태가 'Failed' 또는 'Error'로 표시되는 경우입니다.

-   **배포 로그(Deploy Log) 확인:** 배포 실패의 원인은 대부분 배포 로그에 기록되어 있습니다. Netlify나 Vercel의 배포 상세 페이지로 이동하여 전체 로그를 꼼꼼히 읽어보세요.
-   **흔한 원인들:**
    -   `Command not found: npm`: Node.js 버전이 호환되지 않거나, 빌드 환경 설정이 잘못된 경우입니다. 플랫폼의 Node.js 버전을 프로젝트와 맞추어주어야 합니다.
    -   `Cannot find module '...'`: `npm install` 과정에서 특정 패키지가 제대로 설치되지 않은 경우입니다. `package.json` 파일에 해당 패키지가 올바르게 명시되어 있는지, 오타는 없는지 확인합니다.
    -   `npm test` 실패: CI/CD 파이프라인에 테스트 단계가 포함된 경우, 테스트 코드가 실패하면 빌드도 중단됩니다.
    -   `Out of Memory`: 프로젝트의 규모가 너무 커서 빌드 과정에서 플랫폼이 제공하는 메모리 한도를 초과하는 경우입니다. 코드 스플리팅(Code Splitting) 등을 통해 빌드 크기를 줄이는 최적화 작업이 필요할 수 있습니다.

> [!TIP]
> 배포 문제를 해결하는 가장 좋은 방법은 **로그를 읽는 습관**을 들이는 것입니다. 에러 메시지는 문제의 원인을 알려주는 가장 중요한 단서입니다. 두려워하지 말고, 에러 메시지를 천천히 읽고 이해하려 노력하거나, 메시지 전체를 복사하여 검색해보는 것이 실력 향상에 큰 도움이 됩니다.

---

- [목차로 돌아가기](../README.md)
- [이전 강의로 이동](./05-GitHub-Pages-Deployment.md)
- [다음 강의로 이동](./Lab1-AI-Coding-Assistant.md)