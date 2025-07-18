# 05. 깃허브 (GitHub) 시작하기

안녕하세요! 웹 개발의 세계에 오신 것을 환영합니다. 오늘은 여러분이 개발자로서 가장 많이 사용하게 될 도구 중 하나인 '깃허브(GitHub)'에 대해 알아볼 거예요. 깃허브는 여러분이 만든 코드를 저장하고, 다른 사람들과 함께 작업하며, 프로젝트를 관리하는 데 도움을 주는 아주 유용한 서비스입니다.

> [!NOTE]
> 깃허브는 단순히 코드를 저장하는 곳이 아니라, 전 세계 개발자들이 함께 협력하고 아이디어를 공유하는 거대한 커뮤니티 공간이기도 합니다. 마치 여러분이 친구들과 함께 그림을 그리거나, 글을 쓰는 것처럼 코드를 함께 만들고 발전시킬 수 있는 곳이라고 생각하면 됩니다.

## 목차

1. [깃허브 계정 생성 및 로그인](#1-깃허브-계정-생성-및-로그인)
2. [레포지토리 (Repository) 생성하기](#2-레포지토리-repository-생성하기)
3. [파일 업로드 및 변경하기 (Commit)](#3-파일-업로드-및-변경하기-commit)
   - [3-1) 웹에서 파일 추가하기](#3-1-웹에서-파일-추가하기)
   - [3-2) 웹에서 파일 수정하기](#3-2-웹에서-파일-수정하기)
4. [브랜치 (Branch) 사용하기](#4-브랜치-branch-사용하기)
   - [4-1) 새로운 브랜치 생성하기](#4-1-새로운-브랜치-생성하기)
   - [4-2) 브랜치에서 파일 변경하기](#4-2-브랜치에서-파일-변경하기)
5. [풀 리퀘스트 (Pull Request) 보내기](#5-풀-리퀘스트-pull-request-보내기)
6. [풀 리퀘스트 (Pull Request) 병합하기 (Merge)](#6-풀-리퀘스트-pull-request-병합하기-merge)
7. [이슈 (Issues) 사용하기](#7-이슈-issues-사용하기)
8. [깃허브 페이지 (GitHub Pages)로 웹사이트 배포하기](#8-깃허브-페이지-github-pages로-웹사이트-배포하기)
9. [깃허브 데스크톱 (GitHub Desktop) 사용하기](#9-깃허브-데스크톱-github-desktop-사용하기)

---

## 1) 깃허브 계정 생성 및 로그인

깃허브를 사용하려면 먼저 계정을 만들어야 합니다. 웹 브라우저를 열고 [github.com](https://github.com)에 접속하여 회원가입을 진행해주세요. 이메일 주소, 사용자 이름, 비밀번호를 입력하고 안내에 따라 계정을 생성하면 됩니다.

## 2) 레포지토리 (Repository) 생성하기

레포지토리(Repository)는 여러분의 프로젝트 코드를 저장하는 공간입니다. 마치 여러분의 방에 있는 서랍장이나 파일 폴더와 같다고 생각하면 됩니다. 이제 첫 번째 레포지토리를 만들어 볼까요?

1.  깃허브 웹사이트에 로그인한 후, 오른쪽 상단의 `+` 버튼을 클릭하고 `New repository`를 선택합니다.
2.  **Repository name**: 프로젝트의 이름을 입력합니다. (예: `my-first-web-project`)
3.  **Description (Optional)**: 프로젝트에 대한 간단한 설명을 작성합니다.
4.  **Public/Private**: `Public`은 누구나 이 레포지토리를 볼 수 있도록 공개하는 것이고, `Private`은 여러분만 볼 수 있도록 비공개하는 것입니다. 처음에는 `Public`으로 설정해도 좋습니다.
5.  **Add a README file**: 이 옵션을 체크하면 프로젝트에 대한 설명을 담을 수 있는 `README.md` 파일이 자동으로 생성됩니다. 꼭 체크해주세요!
6.  **Create repository** 버튼을 클릭합니다.

축하합니다! 첫 번째 레포지토리가 성공적으로 생성되었습니다.

## 3) 파일 업로드 및 변경하기 (Commit)

이제 레포지토리에 파일을 추가하거나 기존 파일을 변경하는 방법을 알아볼까요? 깃허브에서는 파일의 변경 사항을 '커밋(Commit)'이라는 단위로 기록합니다. 커밋은 마치 여러분이 일기를 쓰는 것처럼, 어떤 내용을 언제 변경했는지 기록하는 것이라고 생각하면 됩니다.

### 3-1) 웹에서 파일 추가하기

1.  생성한 레포지토리 페이지로 이동합니다.
2.  `Add file` 버튼을 클릭하고 `Create new file`을 선택합니다.
3.  **Name your file...**: 파일 이름을 입력합니다. (예: `index.html`)
4.  파일 내용 입력란에 간단한 HTML 코드를 작성합니다.
    ```html
    <!DOCTYPE html>
    <html>
    <head>
        <title>My First Web Page</title>
    </head>
    <body>
        <h1>Hello, GitHub!</h1>
    </body>
    </html>
    ```
5.  **Commit new file** 섹션에서 커밋 메시지를 작성합니다. 커밋 메시지는 "무엇을 변경했는지"를 간략하게 설명하는 글입니다. (예: `Add initial index.html`)
6.  `Commit new file` 버튼을 클릭합니다.

이제 `index.html` 파일이 레포지토리에 추가되었습니다.

### 3-2) 웹에서 파일 수정하기

1.  수정하고 싶은 파일을 클릭합니다. (예: `index.html`)
2.  파일 내용 위에 있는 연필 모양 아이콘 (Edit this file)을 클릭합니다.
3.  내용을 수정한 후, 아래의 **Commit changes** 섹션에서 커밋 메시지를 작성하고 `Commit changes` 버튼을 클릭합니다.

> [!IMPORTANT]
> 커밋 메시지는 나중에 여러분이나 다른 사람들이 어떤 변경이 있었는지 쉽게 파악할 수 있도록 명확하고 간결하게 작성하는 것이 중요합니다.

## 4) 브랜치 (Branch) 사용하기

브랜치(Branch)는 프로젝트의 여러 버전을 동시에 관리할 수 있게 해주는 기능입니다. 마치 나무의 가지처럼, 메인 코드(main 브랜치)에서 새로운 가지를 뻗어 독립적으로 작업을 진행하고, 작업이 완료되면 다시 메인 가지에 합치는 방식입니다.

> [!NOTE]
> 왜 브랜치를 사용할까요? 여러 명이 함께 작업할 때, 각자 다른 기능을 개발하거나 버그를 수정할 때 서로의 코드에 영향을 주지 않기 위해서입니다. 또한, 새로운 기능을 추가하다가 문제가 생겨도 메인 코드에는 영향을 주지 않으므로 안전하게 실험할 수 있습니다.

### 4-1) 새로운 브랜치 생성하기

1.  레포지토리 페이지에서 파일 목록 위에 있는 `main`이라고 쓰여진 드롭다운 메뉴를 클릭합니다.
2.  새로운 브랜치 이름을 입력하는 칸에 이름을 입력합니다. (예: `feature/add-about-page`)
3.  `Create branch: feature/add-about-page from main` 버튼을 클릭합니다.

이제 `feature/add-about-page`라는 새로운 브랜치가 생성되었고, 현재 작업 중인 브랜치가 이 브랜치로 변경됩니다.

### 4-2) 브랜치에서 파일 변경하기

새로 만든 브랜치에서 `index.html` 파일을 수정하거나 새로운 파일을 추가해 보세요. 이 변경 사항은 `feature/add-about-page` 브랜치에만 적용되고, `main` 브랜치에는 영향을 주지 않습니다.

## 5) 풀 리퀘스트 (Pull Request) 보내기

풀 리퀘스트(Pull Request, PR)는 여러분이 브랜치에서 작업한 내용을 메인 브랜치에 합치고 싶을 때 사용하는 기능입니다. "제가 이런 기능을 만들었는데, 메인 코드에 합쳐도 될까요?"라고 제안하는 것과 같습니다.

1.  새로운 브랜치에서 변경 사항을 커밋한 후, 레포지토리 페이지로 돌아오면 `Compare & pull request` 버튼이 나타납니다. 이 버튼을 클릭합니다.
2.  **Open a pull request** 페이지에서 풀 리퀘스트의 제목과 설명을 작성합니다. (예: `Add about page feature`)
3.  `Create pull request` 버튼을 클릭합니다.

이제 풀 리퀘스트가 생성되었습니다. 다른 사람들이 여러분의 코드를 검토하고 의견을 줄 수 있습니다. 만약 여러분 혼자 작업한다면, 스스로 검토하고 합칠 수 있습니다.

## 6) 풀 리퀘스트 (Pull Request) 병합하기 (Merge)

풀 리퀘스트가 검토되고 문제가 없다고 판단되면, 메인 브랜치에 합칠 수 있습니다.

1.  생성된 풀 리퀘스트 페이지로 이동합니다.
2.  `Merge pull request` 버튼을 클릭합니다.
3.  `Confirm merge` 버튼을 클릭합니다.

이제 여러분의 변경 사항이 `main` 브랜치에 성공적으로 합쳐졌습니다. 작업이 완료된 브랜치는 삭제해도 좋습니다.

## 7) 이슈 (Issues) 사용하기

이슈(Issues)는 프로젝트에서 해야 할 일, 발견된 버그, 새로운 기능 제안 등을 기록하고 추적하는 데 사용됩니다. 마치 프로젝트의 할 일 목록이나 문제점 보고서와 같습니다.

1.  레포지토리 페이지 상단의 `Issues` 탭을 클릭합니다.
2.  `New issue` 버튼을 클릭합니다.
3.  이슈의 제목과 설명을 작성합니다. (예: `Fix: Navigation bar not responsive on mobile`)
4.  `Submit new issue` 버튼을 클릭합니다.

생성된 이슈는 다른 사람들과 함께 논의하고, 누가 이 이슈를 해결할지 할당하고, 진행 상황을 추적하는 데 사용될 수 있습니다.

## 8) 깃허브 페이지 (GitHub Pages)로 웹사이트 배포하기

깃허브 페이지는 여러분의 HTML, CSS, JavaScript 파일을 웹사이트로 만들어 전 세계에 공개할 수 있게 해주는 깃허브의 무료 기능입니다.

1.  레포지토리 페이지에서 `Settings` 탭을 클릭합니다.
2.  왼쪽 메뉴에서 `Pages`를 클릭합니다.
3.  **Branch** 섹션에서 `main` 브랜치를 선택하고 `/(root)` 폴더를 선택한 후 `Save` 버튼을 클릭합니다.
4.  잠시 기다리면, 여러분의 웹사이트 주소가 나타납니다. (예: `https://[사용자이름].github.io/[레포지토리이름]/`)

이제 여러분이 만든 웹페이지를 전 세계 어디서든 접속할 수 있게 되었습니다!

> [!TIP]
> 깃허브 페이지는 정적인 웹사이트(HTML, CSS, JavaScript로만 이루어진 웹사이트)를 호스팅하는 데 매우 유용합니다. 복잡한 서버 기능이 필요한 웹사이트는 다른 호스팅 서비스를 이용해야 합니다.

## 9) 깃허브 데스크톱 (GitHub Desktop) 사용하기

깃허브 웹사이트에서 직접 작업하는 것도 좋지만, '깃허브 데스크톱'이라는 프로그램을 사용하면 여러분의 컴퓨터에서 더 편리하게 깃허브 작업을 할 수 있습니다. 깃허브 데스크톱은 깃허브의 복잡한 명령어를 몰라도 쉽게 코드를 관리할 수 있도록 도와주는 도구입니다.

1.  [desktop.github.com](https://desktop.github.com/)에서 깃허브 데스크톱을 다운로드하여 설치합니다.
2.  설치 후 깃허브 계정으로 로그인합니다.
3.  `File` > `Clone Repository...`를 선택하여 여러분의 레포지토리를 컴퓨터로 가져옵니다.
4.  이제 컴퓨터의 파일 탐색기에서 레포지토리 폴더를 열고 파일을 수정하거나 추가할 수 있습니다.
5.  변경 사항을 저장한 후 깃허브 데스크톱으로 돌아오면, 변경된 파일 목록이 나타납니다. 커밋 메시지를 작성하고 `Commit to main` (또는 현재 브랜치) 버튼을 클릭합니다.
6.  `Push origin` 버튼을 클릭하여 변경 사항을 깃허브 웹사이트에 업로드합니다.

> [!NOTE]
> 깃허브 데스크톱은 깃(Git)이라는 버전 관리 시스템을 더 쉽게 사용할 수 있도록 도와주는 도구입니다. 깃에 대한 자세한 내용은 다음 강의에서 더 깊이 다룰 예정입니다.

---

- [목차로 돌아가기](../README.md)
- [이전 강의로 이동](04-Git-Fundamentals.md)
- [다음 강의로 이동](06-Development-Workflow.md)
