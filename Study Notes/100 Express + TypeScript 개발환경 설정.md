# Express + TypeScript 개발환경 설정

### 📅 날짜:

> 2025.11.27 (목)
> 

### 📘 오늘 공부한 주제:

> Express + TypeScript 개발환경 세팅 및 주요 설정 이해
> 

---

## 📝 핵심 개념 요약

- TypeScript 프로젝트에서 Express를 사용하려면 **TS 컴파일 환경 + 라이브러리 타입 설치**가 필수
- `ts-node-dev`는 **자동 리스타트 + 빠른 반응성**으로 개발 효율이 높음
- `tsconfig.json`은 TS 프로젝트의 핵심 — module, esModuleInterop, sourceMap 등 주요 옵션 숙지 필요
- Node.js 환경에서는 ESModule 설정과 TS 환경이 충돌할 수 있어 **“type": "module" 제거 필요**
- JS로 컴파일하면 `dist` 폴더에서 실행, 개발 시에는 TS 파일을 바로 실행

## 📊 핵심 요약 표

| 개념 | 설명 |
| --- | --- |
| 패키지 설치 | express, typescript, ts-node-dev 등 기본 개발 패키지 설치 |
| 타입 적용 | `@types/express` 설치해 Express의 타입 지원 |
| tsconfig 주요 옵션 | module=CommonJS, esModuleInterop=true, sourceMap=true 등 필수 설정 |
| 실행 스크립트 | dev(개발), build(컴파일), start(JS 실행)로 구성 |
| 재시작 도구 | ts-node-dev 사용 권장 (속도+편의성 우수) |

### 💻 실습 내용 정리

### ✔ 프로젝트 생성 및 패키지 설치

```bash
mkdir express-ts
cd express-ts
npm init -y
npm i express
npm i -D typescript ts-node-dev ts-node nodemon
```

### ✔ app.ts 작성

```tsx
import express from "express";

const app = express();

app.get("/", (req, res) => {
  res.send("Hello World");
});

app.listen(3000, () => {
  console.log("Server is running on port 3000");
});
```

### ✔ 타입 설치

```bash
npm i -D @types/express
```

### ✔ 스크립트 구성

```json
"scripts": {
  "dev": "ts-node-dev --respawn src/app.ts",
  "build": "tsc",
  "start": "node --enable-source-maps dist/app.js"
}
```

### ✔ tsconfig.json 주요 설정

- CommonJS 모듈 사용
- sourceMap 활성화
- outDir/src 구조 명확화

### ❗ 헷갈렸던 점 / 문제 해결:

| 문제 | 해결 |
| --- | --- |
| “type": "module” 설정할 경우 import 충돌 | ts-node 환경과 충돌 → package.json에서 제거 |
| nodemon vs ts-node-dev 차이 | ts-node-dev가 ts-node를 기반으로 빠르게 재시작 가능하여 개발 효율 증가 |
| sourceMap 적용 시 JS에서 TS 위치 트래킹 필요 | `node --enable-source-maps dist/app.js`로 해결 |

### 💡 느낀 점 / 배운 점:

- TypeScript는 단순히 타입을 쓰는 것을 넘어, **컴파일 환경 설정이 매우 중요**하다는 것을 체감함
- ts-node-dev가 개발 속도 향상에 꽤 도움이 된다는 점을 알게 됨
- Express를 TS에서 사용할 때 `@types`를 설치해야 한다는 구조 이해가 확실해짐
- TS 프로젝트의 골격은 거의 **tsconfig.json + scripts**라고 해도 될 만큼 중요함

### 🏷️ 키워드 (태그):

`#TypeScript` `#Express` `#tsconfig` `#ts-node-dev` `#Node.js` `#Backend` `#DevEnvironment`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-11-27 | Express + TypeScript 개발환경 설정 | TS + Express 개발환경 구성, tsconfig 필수 옵션 이해, 타입 적용 방법 학습 | 프로젝트 생성, express 설치, ts-node-dev 사용, tsconfig 구성, 스크립트 정리 | type module 충돌 해결, ts-node-dev의 장점 이해, TS 프로젝트 구조 이해 | `TypeScript` `Express` `ts-node-dev` `tsconfig` `Node.js` | ✅ |
