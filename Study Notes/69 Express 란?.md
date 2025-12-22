# Express 란?

### 📅 날짜:

> 2025.10.10 (금)
> 

### 📘 오늘 공부한 주제:

> Node.js 기반 백엔드 프레임워크 Express의 개념과 기본 서버 구축 실습
> 

---

## 📝 핵심 개념 요약

- **Express란?**
    
    Node.js 환경에서 빠르고 간단하게 서버를 구축할 수 있는 **경량 웹 프레임워크**
    
    → 최소한의 규칙 + 미들웨어 기반 구조로 **생산성과 확장성** 모두 확보
    
- **Express의 특징**
    1. **간단하고 유연한 서버 개발**
        - http 모듈보다 코드가 간결하고 직관적
        - 최소한의 구조로 빠른 서버 구축 가능
    2. **미들웨어 기반 아키텍처**
        - 요청-응답 과정을 여러 단계로 분리
        - 인증, 에러 처리, 로깅 등 기능 확장이 용이
    3. **확장성 & 생태계**
        - 수많은 미들웨어 (예: `express-session`, `passport`, `helmet`)
        - npm을 통한 오픈소스 통합 가능

## 📊 핵심 요약 표

| 구분 | http 모듈 | Express |
| --- | --- | --- |
| 라우팅 | if문으로 직접 작성 | `app.get()` 등 메서드 사용 |
| 요청 파싱 | 직접 구현 | `express.json()`, `express.urlencoded()` |
| 정적 파일 제공 | 직접 구현 | `express.static()` |
| 확장성 | 낮음 (직접 작성 필요) | 매우 높음 (미들웨어 사용) |
| 유지보수 | 복잡 | 구조적, 효율적 |

### 💻 실습 내용 정리

### ✅ 1. 프로젝트 초기 설정

```bash
mkdir topic-express-essential
cd topic-express-essential
npm init -y
npm i express
npm i -D nodemon
```

### ✅ 2. 기본 서버 코드 (`src/app.js`)

```jsx
import express from "express";

const app = express();

app.get("/", (req, res) => {
  res.json({
    message: "코드잇 express 실습 서버에 오신 것을 환영합니다.",
  });
});

app.listen(3000, () => {
  console.log("서버가 3000번 포트에서 실행 중입니다.");
});
```

### ✅ 3. `package.json` 설정

```json
{
  "type": "module",
  "scripts": {
    "dev": "nodemon src/app.js"
  }
}
```

> npm run dev 명령으로 자동 재시작 서버 실행 가능
> 

### ❗ 헷갈렸던 점 / 문제 해결:

- **Q:** Node.js 내장 http 모듈로도 서버를 만들 수 있는데, Express를 왜 사용할까?
    
         → http 모듈은 모든 로직을 직접 작성해야 하지만, Express는 미들웨어를 통해 자동 처리 가능 (json         파싱, 라우팅, 정적 파일 등)
    
- **Q:** `require` 대신 `import`를 썼는데 오류 발생
    
        → `"type": "module"` 설정으로 ES 모듈 문법(`import/export`) 허용해야 함
    

### 💡 느낀 점 / 배운 점:

- Express는 단순히 “편한 서버 도구”가 아니라, **미들웨어 기반의 구조적 프레임워크**임을 이해함
- http 모듈로 기본 원리를 배우고, Express로 실무형 서버를 만드는 흐름이 중요
- `nodemon` 덕분에 서버 재시작 없이 실시간 반영 가능 → 개발 효율 극대화

### 🏷️ 키워드 (태그):

`#Express` `#NodeJS` `#백엔드` `#웹프레임워크` `#미들웨어` `#라우팅` `#nodemon` `#npm` `#서버기초`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-10-10 | Express 기본 개념 및 서버 구축 | Node.js 기반 경량 웹 프레임워크, 미들웨어 중심 구조로 생산성과 확장성 제공 | Express 설치, 기본 서버(app.js), package.json 설정 | http 모듈 대비 Express의 장점 이해, "type": "module" 설정 문제 해결 | `#Express` `#NodeJS` `#백엔드` `#미들웨어` `#라우팅` `#서버기초`  | ✅ |
