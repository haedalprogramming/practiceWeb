# 05. GitHub Pages를 이용한 수동 배포

Netlify나 Vercel과 같은 자동 배포 도구의 편리함을 경험하기에 앞서, 우리는 먼저 가장 기본적인 방법으로 웹사이트를 배포하는 과정을 직접 겪어볼 것입니다. 이 과정을 통해 '빌드'와 '배포'라는 개념이 실제로 무엇을 의미하는지 몸으로 체감할 수 있으며, 이는 앞으로 배울 자동화 과정의 원리를 이해하는 데 튼튼한 기초가 되어줄 것입니다.

이번 시간에는 **GitHub Pages**라는 서비스를 이용하여 우리가 만든 React 애플리케이션을 직접 배포해보겠습니다.

## 2-1) GitHub Pages란?

**GitHub Pages**는 GitHub에서 무료로 제공하는 정적 사이트 호스팅 서비스입니다. GitHub 리포지토리에 있는 HTML, CSS, JavaScript 파일들을 웹사이트 형태로 만들어 공개할 수 있도록 해줍니다.

사용자 또는 조직 페이지(`https://<username>.github.io`), 그리고 프로젝트 페이지(`https://<username>.github.io/<repository-name>`) 두 가지 종류의 사이트를 만들 수 있으며, 우리는 이 중에서 프로젝트 페이지를 활용하여 React 앱을 배포할 것입니다.

## 2-2) 수동 배포를 위한 준비 과정

React 프로젝트를 GitHub Pages에 배포하기 위해서는 몇 가지 준비가 필요합니다.

### 1) gh-pages 패키지 설치

`gh-pages`는 빌드된 결과물(`build` 폴더)을 GitHub 리포지토리의 특정 브랜치(일반적으로 `gh-pages` 브랜치)에 손쉽게 `push` 할 수 있도록 도와주는 개발용 라이브러리입니다.

터미널을 열고 우리 프로젝트의 루트 디렉토리에서 아래 명령어를 실행하여 `gh-pages`를 개발 의존성(`--save-dev`)으로 설치합니다.

```bash
npm install gh-pages --save-dev
```

> [!NOTE]
> <strong>--save-dev</strong> 또는 <strong>-D</strong> 옵션은 해당 패키지가 개발 과정에서만 필요하고, 실제 애플리케이션 코드에는 포함되지 않음을 의미합니다. `gh-pages`는 배포를 도와주는 도구일 뿐, React 앱 자체의 기능과는 무관하므로 개발 의존성으로 설치하는 것이 올바릅니다.

### 2) package.json 파일 수정

`package.json` 파일은 프로젝트의 모든 정보를 담고 있는 중요한 설정 파일입니다. 이 파일에 몇 가지 정보를 추가하고 수정해야 합니다.

#### `homepage` 속성 추가

먼저, `package.json` 파일의 최상단에 `homepage`라는 속성을 추가합니다. 이 속성은 우리 웹사이트가 최종적으로 배포될 URL 주소를 명시하는 역할을 합니다. Create React App은 이 값을 참조하여 빌드 시에 올바른 상대 경로를 생성해 줍니다.

URL 주소의 형식은 `https://<GitHub-사용자이름>.github.io/<리포지토리-이름>` 입니다.

```json
{
  "name": "my-react-app",
  "version": "0.1.0",
  "homepage": "https://my-github-id.github.io/my-react-repo", // 이 부분을 추가합니다.
  "private": true,
  // ... (이하 생략)
}
```

> [!CAUTION]
> `<GitHub-사용자이름>`과 `<리포지토리-이름>` 부분은 반드시 자신의 정보에 맞게 정확하게 수정해야 합니다. 이 주소가 틀리면 CSS나 이미지 파일 등을 제대로 불러오지 못하는 문제가 발생합니다.

#### `scripts`에 배포 명령어 추가

다음으로, `package.json` 파일의 `scripts` 객체에 배포 과정을 자동화할 두 개의 명령어를 추가합니다.

-   `predeploy`: `deploy` 스크립트가 실행되기 **전(pre)**에 자동으로 실행될 명령어입니다. 우리는 여기에 `npm run build`를 지정하여, 배포 직전에 항상 최신 코드를 기반으로 빌드를 수행하도록 합니다.
-   `deploy`: 실제 배포를 담당할 명령어입니다. `gh-pages` 패키지를 사용하여 `build` 폴더의 내용을 `gh-pages`라는 이름의 브랜치로 `push`하도록 설정합니다.

```json
// ... (생략)
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject",
    "predeploy": "npm run build",
    "deploy": "gh-pages -d build"
  },
// ... (이하 생략)
```

`scripts` 객체에 위 두 줄을 추가하면, 우리는 이제 터미널에서 `npm run deploy`라는 간단한 명령어 하나만으로 빌드와 배포를 한 번에 처리할 수 있게 됩니다.

## 2-3) 배포 실행하기

모든 준비가 끝났습니다. 이제 터미널에 아래 명령어를 입력하여 배포를 실행해봅시다.

```bash
npm run deploy
```

이 명령어를 실행하면 다음과 같은 일들이 순서대로 일어납니다.

1.  `predeploy` 스크립트가 먼저 실행됩니다: `npm run build`
2.  React 프로젝트가 프로덕션 모드로 빌드되고, 그 결과물이 `build` 폴더에 생성됩니다.
3.  `deploy` 스크립트가 실행됩니다: `gh-pages -d build`
4.  `gh-pages` 라이브러리가 `build` 폴더의 내용을 GitHub 리포지토리의 `gh-pages`라는 브랜치로 `push`합니다. (만약 `gh-pages` 브랜치가 없다면 자동으로 생성합니다.)
5.  터미널에 `Published`라는 메시지가 나타나면 배포가 성공적으로 완료된 것입니다.

## 2-4) GitHub Pages 설정 확인

마지막으로, GitHub 리포지토리에서 Pages 기능이 올바르게 활성화되었는지 확인해야 합니다.

1.  배포한 GitHub 리포지토리 페이지로 이동합니다.
2.  상단의 `Settings` 탭을 클릭합니다.
3.  왼쪽 메뉴에서 `Pages`를 클릭합니다.
4.  `Build and deployment` 섹션에서 `Source`가 `Deploy from a branch`로 설정되어 있는지 확인합니다.
5.  `Branch`가 방금 우리가 배포한 `gh-pages` 브랜치로, 폴더는 `/(root)`로 설정되어 있는지 확인합니다. 대부분의 경우 이 설정은 자동으로 완료됩니다.

설정이 올바르다면, 상단에 "Your site is live at `https://<username>.github.io/<repository-name>`" 과 같은 메시지와 함께 배포된 사이트 주소가 나타납니다. 이 주소로 접속하여 당신의 웹사이트가 성공적으로 배포되었는지 확인해보세요!

> [!TIP]
> 배포 후 사이트에 즉시 반영되지 않을 수 있습니다. GitHub Pages가 변경사항을 처리하고 웹에 게시하는 데 몇 분 정도 시간이 걸릴 수 있으니, 조금 기다렸다가 다시 확인해보세요.

이제 우리는 코드를 수정하고, `git push`로 원격 리포지토리에 반영한 뒤, `npm run deploy` 명령어를 실행하는 과정을 통해 웹사이트를 수동으로 업데이트할 수 있게 되었습니다. 이 과정이 바로 '배포'입니다. 다음 시간에는 이 수동 배포 과정을 어떻게 자동화할 수 있는지 알아보겠습니다.

---

- [목차로 돌아가기](../README.md)
- [이전 강의로 이동](./04-GitHub-Actions-CI-CD.md)
- [다음 강의로 이동](./06-Environment-Variables-and-Troubleshooting.md) 