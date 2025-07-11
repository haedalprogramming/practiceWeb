# 05. 최종 테스트 및 배포

이제 여러분의 포트폴리오 프로젝트를 세상에 공개할 시간입니다. 배포는 개발의 끝이 아니라, 새로운 시작입니다. 이 단계에서는 사용자가 겪을 수 있는 문제를 최소화하기 위해 마지막으로 프로젝트를 점검하고, Day 7에서 배운 GitHub Actions를 활용하여 배포를 자동화하는 과정을 거칩니다.

## 5-1) 배포 전 최종 점검 리스트

비행기가 이륙 전 꼼꼼한 점검을 거치듯, 우리의 웹사이트도 배포 전에 최종 점검이 필요합니다. 아래 리스트를 하나씩 확인하며 프로젝트의 완성도를 높여보세요.

-   **[ ] 코드 품질 확인:**
    -   `npm run lint` 명령어를 실행하여 ESLint가 지적하는 오류나 경고가 없는지 확인하고 수정합니다.
    -   `console.log`, `debugger` 등 개발 중에 사용했던 불필요한 코드가 남아있지 않은지 확인하고 제거합니다.

-   **[ ] 기능 및 UI 테스트:**
    -   모든 버튼, 링크, 상호작용 요소들이 의도대로 동작하는지 마지막으로 테스트합니다.
    -   이미지가 깨지거나, 텍스트가 레이아웃을 벗어나는 등 UI가 깨지는 부분이 없는지 확인합니다.
    -   (선택) Chrome, Edge, Safari 등 여러 브라우저에서 화면이 동일하게 보이는지 확인합니다. (크로스 브라우징 테스트)

-   **[ ] SEO 및 웹 접근성 기본:**
    -   `index.html` 파일의 `<title>` 태그를 자신의 포트폴리오에 맞게 수정했는지 확인합니다.
    -   사이트를 대표하는 `favicon.ico` 파일을 `public` 폴더에 추가했는지 확인합니다.
    -   모든 이미지에 의미 있는 `alt` 속성이 포함되었는지 확인합니다.

-   **[ ] `README.md` 파일 작성:**
    -   프로젝트의 루트 디렉토리에 있는 `README.md` 파일을 업데이트합니다. 프로젝트에 대한 간략한 설명, 사용된 기술 스택, 실행 방법, 그리고 배포된 라이브 URL을 포함해야 합니다.

## 5-2) GitHub Actions를 이용한 CI/CD 배포

Day 7에서 배운 내용을 바탕으로, `main` 브랜치에 코드를 `push`하면 자동으로 테스트, 빌드, 배포가 진행되는 CI/CD 파이프라인을 구축합니다.

1.  **Vite 설정 파일 수정:**
    `vite.config.ts` 파일에 `base` 옵션을 추가하여 GitHub Pages 배포에 맞게 경로를 설정합니다.

    ```ts
    // vite.config.ts
    import { defineConfig } from 'vite'
    import react from '@vitejs/plugin-react'

    export default defineConfig({
      plugins: [react()],
      base: '/[YOUR_REPOSITORY_NAME]/', // 👈 이 부분을 추가하세요!
    })
    ```

2.  **GitHub Actions 워크플로우 파일 작성:**
    `.github/workflows/deploy.yml` 파일을 생성하고, 아래 내용을 붙여넣습니다. 이 워크플로우는 `main` 브랜치에 코드가 푸시될 때마다 자동으로 실행됩니다.

    ```yaml
    name: Deploy to GitHub Pages

    on:
      push:
        branches:
          - main # main 브랜치에 푸시될 때 실행

    jobs:
      build-and-deploy:
        runs-on: ubuntu-latest
        steps:
          - name: Checkout
            uses: actions/checkout@v3

          - name: Set up Node.js
            uses: actions/setup-node@v3
            with:
              node-version: 18

          - name: Install dependencies
            run: npm install

          - name: Build
            run: npm run build

          - name: Deploy
            uses: peaceiris/actions-gh-pages@v3
            with:
              github_token: ${{ secrets.GITHUB_TOKEN }}
              publish_dir: ./dist # 빌드 결과물이 있는 디렉토리
    ```

3.  **배포 실행 및 확인:**
    -   수정한 코드를 모두 `git add`, `git commit` 한 후 `main` 브랜치로 `git push` 합니다.
    -   GitHub 저장소의 **Actions** 탭으로 이동하여 워크플로우가 성공적으로 실행되는지 확인합니다.
    -   **Settings → Pages** 탭으로 이동하여 생성된 라이브 URL을 확인하고, 사이트가 정상적으로 보이는지 최종 확인합니다.

---

- [목차로 돌아가기](../README.md)
- [이전 강의로 돌아가기](./04-Presentation-Preparation.md)
- [다음 강의로 이동하기](./06-Demo-and-Peer-Review.md)