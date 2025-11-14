---
title: "React 앱 GitHub Pages 배포 가이드(CRA + gh-pages)"
layout: page
order: 1
---

## 개요
Create React App(CRA)로 만든 프론트엔드를 **GitHub Pages**에 올리는 가장 간단한 방법을 정리했다. 핵심은 `gh-pages` 패키지로 `build/`를 `gh-pages` 브랜치에 푸시하고, 리포지토리 **Settings → Pages**에서 배포 원본을 `gh-pages`로 잡는 것이다. 

---

## 1) 사전 준비
- 로컬에 CRA 프로젝트가 있는 상태에서 진행한다. (혹은 `npx create-react-app my-app`으로 새 프로젝트 생성)  
- GitHub에 원격 리포지토리를 만들어 `main` 브랜치로 푸시해 둔다.

---

## 2) gh-pages 설치
프로젝트 루트에서:

```bash
npm i -D gh-pages
````

`gh-pages`는 정적 파일을 **`gh-pages` 브랜치**로 올려 Pages가 서빙하도록 해주는 CLI다.

---

## 3) package.json 설정

CRA 공식 가이드의 순서를 그대로 따른다.
① **homepage** 추가 → ② **deploy 스크립트** 추가 → ③ `npm run deploy` 실행.

> 프로젝트 사이트(일반 리포) 주소 예시: `https://<user>.github.io/<repo>`

```json
{
  "homepage": "https://<user>.github.io/<repo>",
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",

    "predeploy": "npm run build",
    "deploy": "gh-pages -d build"
  }
}
```

> 위와 동일한 절차 예제: gitname/react-gh-pages 튜토리얼(같은 설정을 단계별로 설명).

---

## 4) 배포 실행

```bash
npm run deploy
```

* `predeploy`가 먼저 빌드를 수행하고, `gh-pages -d build`가 **빌드 결과물을 `gh-pages` 브랜치로 푸시**한다.

---

## 5) GitHub Pages 설정

리포지토리 **Settings → Pages**로 이동해 **Publishing source**를 `gh-pages` 브랜치(폴더 `/ (root)`)로 선택한다. 저장 후 표시되는 **Live URL**이 최종 접속 주소다.

---

## 6) SPA 라우팅(새로고침 404 방지)

GitHub Pages는 서버 리라이트가 없어 **SPA에서 경로 새로고침 시 404**가 난다. 해결은 두 가지:

* 가장 간단: **`HashRouter`** 사용
  URL의 해시(`#`) 뒤를 클라이언트 라우팅으로 쓰므로 서버에 경로가 전달되지 않는다.

* 대안: **404 → index.html 리다이렉트(“spa-github-pages” 트릭)**
  `404.html`과 `index.html`에 스크립트를 추가해 어느 경로로 들어와도 앱 엔트리로 돌린다(프로젝트 페이지면 `segmentCount=1`).

---

## 7) 자주 겪는 이슈

* 배포했는데 **README 화면만 보일 때** → Pages의 **Source가 `gh-pages`/root인지**, 브랜치에 빌드 산출물이 있는지 확인.
* 프로젝트 루트가 아닌 **엉뚱한 폴더에서 `npm run deploy`** 실행 → 현재 폴더의 `package.json` 기준으로 동작하므로 반드시 **프로젝트 루트**에서 실행.
* 정적 자산 경로 깨짐 → **`homepage`**가 올바른지 재확인(CRA는 빌드 시 상대 경로에 반영).

---

## (선택) Vite로 배포한다면

Vite는 **Pages → Source: GitHub Actions**로 설정 후, 공식 문서의 워크플로 템플릿으로 `dist/`를 업로드하는 방식을 권장한다. 프로젝트 사이트인 경우 `vite.config`에 `base: '/<repo>/'` 설정을 잊지 말자.

---

## 결론

* CRA라면 **`homepage` + `gh-pages` + `npm run deploy`** 3단계가 가장 간단하다.
* 라우팅이 있다면 **HashRouter**(간단) 또는 **404 리다이렉트** 트릭으로 새로고침 404를 해결하자.

```

---
```
