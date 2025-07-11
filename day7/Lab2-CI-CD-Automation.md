# Lab 2: CI/CD 및 배포 자동화하기

이번 실습에서는 GitHub Actions를 사용하여 코드가 변경될 때마다 자동으로 웹사이트를 빌드하고 GitHub Pages에 배포하는 CI/CD(Continuous Integration/Continuous Deployment) 파이프라인을 구축하는 전체 과정을 실습합니다.

> [!NOTE]
> 현업에서는 Netlify나 Vercel과 같은 전문적인 배포 플랫폼도 많이 사용합니다. 하지만 이 실습에서는 GitHub에 내장된 기능인 GitHub Actions와 GitHub Pages를 사용하여 CI/CD 파이프라인 구축의 핵심 원리를 직접 경험하고 이해하는 데 집중합니다. 이를 통해 어떤 배포 환경에서도 응용할 수 있는 기본기를 다질 수 있습니다.

## 실습 목표

- GitHub Actions 워크플로우를 설정하여 CI/CD 파이프라인을 구축할 수 있습니다.
- `main` 브랜치에 코드를 Push할 때마다 자동으로 빌드 및 배포가 실행되도록 할 수 있습니다.
- GitHub Pages를 통해 자동화된 워크플로우의 결과를 확인하고, 배포된 웹사이트를 검증할 수 있습니다.

## 1) 실습 준비

가장 먼저, 배포할 React 프로젝트를 준비해야 합니다. 이전 실습에서 만들었던 프로젝트를 사용하거나, 아래 명령어를 통해 새로운 React 프로젝트를 생성할 수 있습니다.

```bash
# "my-ci-cd-app"이라는 이름의 새로운 React 프로젝트 생성
npm create vite@latest my-ci-cd-app -- --template react
```

프로젝트 생성이 완료되면, 해당 프로젝트 디렉토리로 이동하여 필요한 패키지를 설치합니다.

```bash
# 프로젝트 디렉토리로 이동
cd my-ci-cd-app

# 의존성 패키지 설치
npm install
```

이제 이 프로젝트를 GitHub에 올려야 합니다. GitHub에 새로운 저장소(Repository)를 만들고, 아래의 예시 명령어들을 참고하여 프로젝트 코드를 Push해주세요.

```bash
# Git 초기화
git init

# 모든 파일을 Staging 영역에 추가
git add .

# 첫 번째 커밋 생성
git commit -m "Initial commit: Set up React project"

# 브랜치 이름을 main으로 변경
git branch -M main

# 원격 저장소 연결
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPOSITORY_NAME.git

# 원격 저장소로 코드 Push
git push -u origin main
```

> [!IMPORTANT]
> `YOUR_USERNAME`과 `YOUR_REPOSITORY_NAME`은 여러분의 GitHub 사용자 이름과 생성한 저장소 이름으로 반드시 변경해야 합니다.

## 2) `package.json` 설정하기

Vite로 만든 프로젝트를 GitHub Pages에 올바르게 배포하려면, `package.json` 파일에 `homepage` 필드를 추가해야 합니다. 이 설정은 빌드 시 생성되는 HTML 파일이 CSS나 JavaScript 파일들을 어디서 찾아야 할지 알려주는 역할을 합니다.

`package.json` 파일을 열고, 아래와 같이 `homepage` 필드를 추가해주세요.

```json
{
  "name": "my-ci-cd-app",
  "private": true,
  "version": "0.0.0",
  "type": "module",
  "homepage": "https://YOUR_USERNAME.github.io/YOUR_REPOSITORY_NAME",
  "scripts": {
// ... existing code ...
```

> [!NOTE]
> `homepage`를 설정하지 않으면, 배포된 사이트에서 CSS가 적용되지 않거나 페이지가 하얗게 보이는 문제가 발생할 수 있습니다. 이는 파일 경로가 올바르지 않아 브라우저가 리소스를 찾지 못하기 때문입니다.

## 3) GitHub Actions 워크플로우 생성

이제 본격적으로 CI/CD를 자동화할 GitHub Actions 워크플로우 파일을 생성하겠습니다.

1.  프로젝트의 최상위 루트 디렉토리에 `.github`라는 이름의 폴더를 생성합니다.
2.  생성한 `.github` 폴더 안에 `workflows`라는 이름의 폴더를 또 생성합니다.
3.  `workflows` 폴더 안에 `deploy.yml`이라는 이름의 YAML 파일을 생성합니다.

최종적인 파일 구조는 다음과 같아야 합니다.

```
my-ci-cd-app/
├── .github/
│   └── workflows/
│       └── deploy.yml
├── public/
├── src/
├── package.json
└── ... (기타 파일들)
```

이제 `deploy.yml` 파일에 아래의 워크플로우 코드를 작성합니다.

```yml
# 워크플로우의 이름
name: Deploy to GitHub Pages

# 어떤 이벤트에서 이 워크플로우를 실행할지 정의
on:
  # main 브랜치에 push 이벤트가 발생했을 때 실행
  push:
    branches:
      - main
  # Actions 탭에서 수동으로 실행할 수도 있도록 설정
  workflow_dispatch:

# 이 워크플로우에서 실행될 작업(job)들을 정의
jobs:
  # build-and-deploy 라는 이름의 작업 정의
  build-and-deploy:
    # 이 작업이 실행될 가상 환경을 지정 (최신 Ubuntu)
    runs-on: ubuntu-latest
    # 이 작업이 저장소에 콘텐츠를 쓸 수 있도록 권한을 설정
    permissions:
      contents: read
      pages: write
      id-token: write
    # 작업에서 실행될 단계(step)들을 정의
    steps:
      # 1. 저장소의 코드를 가상 환경으로 가져옴 (Checkout)
      - name: Checkout
        uses: actions/checkout@v4

      # 2. Node.js 환경 설정
      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm' # npm 의존성을 캐싱하여 빌드 속도 향상

      # 3. 의존성 패키지 설치
      - name: Install dependencies
        run: npm install

      # 4. 프로젝트 빌드
      - name: Build
        run: npm run build

      # 5. GitHub Pages에 업로드할 아티팩트를 준비
      - name: Setup Pages
        uses: actions/configure-pages@v4

      # 6. 빌드 결과물(아티팩트)을 GitHub Pages에 업로드
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          # "dist" 디렉토리를 업로드 (Vite의 빌드 결과물 폴더)
          path: './dist'
          
      # 7. GitHub Pages에 배포
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

> [!NOTE]
> YAML 파일은 들여쓰기가 매우 중요합니다. 스페이스 2칸을 사용하여 위 코드와 동일한 구조로 작성해야 정상적으로 동작합니다.

## 4) GitHub 저장소 설정 변경

워크플로우가 GitHub Pages에 배포할 수 있도록, 저장소의 설정을 변경해야 합니다.

1.  GitHub 저장소 페이지에서 **\Settings\** 탭으로 이동합니다.
2.  왼쪽 메뉴에서 **\Pages\**를 클릭합니다.
3.  **\Build and deployment\** 섹션에서 **\Source\**를 **\GitHub Actions\**로 변경합니다.

![GitHub Pages Source 설정](https://github.com/eun-seok/eun-seok/assets/71386194/686a3449-3174-4b5b-9d48-35c91f486e9e)

## 5) 워크플로우 실행 및 배포 확인

모든 설정이 끝났습니다! 이제 작성한 워크플로우 파일을 GitHub에 Push하여 자동 배포를 실행해봅시다.

```bash
# 변경된 모든 파일을 Staging 영역에 추가
git add .

# 워크플로우 파일 추가 커밋
git commit -m "feat: Add GitHub Actions workflow for deployment"

# 원격 저장소로 코드 Push
git push
```

Push가 완료되면, GitHub 저장소의 **\Actions\** 탭으로 이동하세요. `Deploy to GitHub Pages`라는 이름의 워크플로우가 자동으로 실행되는 것을 볼 수 있습니다.

![Actions 탭에서 실행 중인 워크플로우](../images/actions.png)

워크플로우를 클릭하면 각 단계가 실행되는 과정을 실시간으로 확인할 수 있습니다. 모든 단계 옆에 녹색 체크 표시가 나타나면 성공적으로 완료된 것입니다.

이제 다시 **\Settings\** > **\Pages\**로 이동하거나, Actions 로그 마지막에 출력된 URL로 접속하여 여러분의 웹사이트가 성공적으로 배포되었는지 확인해보세요!

> [!TIP]
> 배포가 완료된 후 실제 사이트에 반영되기까지 몇 분 정도 시간이 걸릴 수 있습니다. 만약 워크플로우 실행 중 오류가 발생했다면, Actions 탭의 로그를 자세히 살펴보세요. 오류의 원인과 관련된 유용한 정보를 얻을 수 있습니다.

## 6) 자동 배포 경험하기

CI/CD의 가장 큰 장점은 '자동화'입니다. 이제 코드를 간단히 수정하고 다시 Push하여 배포가 자동으로 일어나는 마법 같은 경험을 해보겠습니다.

`src/App.jsx` 파일을 열고, `<h1>` 태그의 내용을 원하는 문구로 변경해보세요.

```jsx
// src/App.jsx

// ... existing code ...
function App() {
  const [count, setCount] = useState(0)

  return (
    <>
      <div>
// ... existing code ...
      </div>
      <h1>Vite + React + CI/CD = ❤️</h1>
      <div className="card">
// ... existing code ...
```

코드를 수정한 후, 다시 commit하고 push합니다.

```bash
git add src/App.jsx
git commit -m "docs: Update text in App component"
git push
```

다시 **\Actions\** 탭을 확인해보세요. 새로운 Push 이벤트를 감지하여 워크플로우가 또다시 자동으로 실행됩니다. 잠시 후 배포가 완료되면, 이전에 확인했던 여러분의 GitHub Pages URL에 접속하여 내용이 변경되었는지 확인해보세요.

---

축하합니다! 여러분은 이제 코드를 작성하고 Push만 하면 자동으로 웹사이트가 배포되는 멋진 CI/CD 파이프라인을 갖게 되었습니다. 이 자동화된 워크플로우는 앞으로 여러분의 개발 생산성을 크게 향상시켜 줄 것입니다.

---

- [목차로 돌아가기](../README.md)
- [이전 강의로 이동](./Lab1-AI-Coding-Assistant.md)
- [다음 강의로 이동](./Lab2-Polish-and-Retrospective.md)