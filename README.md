# my-react-router

react-router 라이브러리 없이 라우팅 구현해보기

## 프로젝트 개발 환경 설정 및 실행

- vite를 이용하여 프로젝트 생성

```bash
yarn create vite my-react-router --template react-ts
```

- vscode에서 yarn berry 설정

```bash
yarn set version berry
yarn
yarn dlx @yarnpkg/sdks vscode

Cmd + Shift + P
=> Select TypeScript version
=> Use Workspace Version
```

- 개발 환경에서 실행

```bash
yarn dev
```

## 라우터 구현

- 브라우저에서 뒤로 가기, 앞으로 가기가 동작하는 이유는 히스토리 스택에 URL 정보가 쌓이기 때문
- [`history.pushState()`](https://developer.mozilla.org/ko/docs/Web/API/History/pushState)로 브라우저의 히스토리 스택에 히스토리를 추가할 수 있음
- 사용자의 히스토리 탐색으로 현재 활성화된 히스토리가 변경되면 [`popstate`](https://developer.mozilla.org/ko/docs/Web/API/Window/popstate_event) 이벤트가 발생
  - ❗️`history.pushState()`로는 발생하지 않고 뒤로 가기, 앞으로 가기로만 발생
- [`location.pathname`](https://developer.mozilla.org/en-US/docs/Web/API/Location/pathname)을 이용하여 현재 경로를 가져올 수 있음

1. `Router` 컴포넌트는 컨텍스트 프로바이더 역할
   - 현재 pathname을 자식 컴포넌트에서 조회할 수 있게 해줌
   - `popstate` 이벤트도 여기다 1번만 등록
2. `Route` 컴포넌트는 컨텍스트를 조회해서 현재 pathname이 props로 받아온 path와 동일하면 렌더링
3. 각 페이지의 링크는 클릭하면 `useRouter` 훅을 사용해서 `pushState()`하도록 핸들러 추가
4. `pushState()`는 `popstate` 이벤트를 발생시키지 않으므로 `useRouter` 훅은 이벤트를 인위적으로 발생시켜야 함: [`dispatchEvent()`](https://developer.mozilla.org/ko/docs/Web/API/EventTarget/dispatchEvent)
