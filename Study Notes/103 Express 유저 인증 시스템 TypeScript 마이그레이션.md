# Express 유저 인증 시스템 TypeScript 마이그레이션

### 📅 날짜:

> 2025.12.02 (화)
> 

### 📘 오늘 공부한 주제:

> Express 기반 유저 인증 시스템을 TypeScript로 마이그레이션, Controller/Service/Repository/Middleware 타입 적용
> 

---

## 📝 핵심 개념 요약

- **TypeScript 마이그레이션 목표**: JS 기반 Express 프로젝트를 완전히 TypeScript로 전환, 타입 안전성 확보
- **Controller / Service / Repository / Middleware 타입 적용**: 각 레이어 함수의 매개변수와 반환 타입 지정
- **환경 변수 타입 처리**: `process.env.JWT_SECRET` 등 `string | undefined` 타입 단언 처리 필요
- **Middleware 에러 핸들링**: `ErrorRequestHandler` 타입 적용, 불필요한 return 제거
- **tsconfig.json 설정**: `allowJS` 주석 처리 후 서버 재시작으로 JS 의존 제거

## 📊 핵심 요약 표

| 개념 | 설명 |
| --- | --- |
| 타입스크립트 환경 설정 | tsconfig.json, ts-node-dev 등 설정, 모든 파일 `.ts` 변환 |
| Controller 타입 | `Request`, `Response` 타입 적용, RequestBody/Params/Query 타입 지정 |
| Service 타입 | 입력 데이터와 반환값 타입 명시, Promise<> 적용 |
| Repository 타입 | DB 처리 함수에 타입 지정, Prisma Client 혹은 DB 반환 타입 활용 |
| Middleware 타입 | `RequestHandler`, `ErrorRequestHandler` 타입 적용, 환경 변수 단언 처리 |
| 환경 변수 타입 | `process.env.JWT_SECRET!` 처럼 단언(`!`) 처리 필요 |

### 💻 실습 내용 정리

### ✔ 실습 #1: TypeScript 환경 설정

```bash
# 모든 JS 파일을 .ts로 변경
# tsconfig.json에서 allowJS: true 주석 처리
```

### ✔ 실습 #2: Controller 타입 적용

```tsx
import { Request, Response } from "express";

export const loginController = async (req: Request, res: Response) => {
  const { email, password } = req.body;
  // 타입 안전 처리
};
```

### ✔ 실습 #3: Service 타입 적용

```tsx
interface LoginInput { email: string; password: string }
const loginService = async (data: LoginInput): Promise<string> => {
  // JWT 토큰 반환
  return token;
};
```

### ✔ 실습 #4: Repository 타입 적용

```tsx
import { PrismaClient, User } from "@prisma/client";
const prisma = new PrismaClient();

const findUserByEmail = async (email: string): Promise<User | null> => {
  return await prisma.user.findUnique({ where: { email } });
};
```

### ✔ 실습 #5: Middleware 타입 적용

```tsx
import { RequestHandler, ErrorRequestHandler } from "express";

// 인증 미들웨어
export const authMiddleware: RequestHandler = (req, res, next) => {
  const token = req.headers.authorization;
  // 타입 단언 필요
  next();
};

// 에러 핸들러
export const errorHandler: ErrorRequestHandler = (err, req, res, next) => {
  res.status(500).json({ message: err.message });
  // 불필요한 return 제거
};
```

### ✔ Git 명령

```bash
git checkout 5_middleware
git reset --hard origin/5_middleware

# 마이그레이션 완료 후
git checkout 6_done
git reset --hard origin/6_done
```

### ❗ 헷갈렸던 점 / 문제 해결:

| 문제 | 해결 |
| --- | --- |
| validateEmailAndPassword 함수 타입 오류 | 매개변수와 반환 타입 명시로 해결 |
| process.env.JWT_SECRET 타입 | `process.env.JWT_SECRET!` 단언 처리 |
| JS 파일과 TS 파일 혼용 | 모든 파일 .ts 변환, tsconfig.json allowJS 주석 처리 |
| 에러 핸들러 불필요한 return | return 제거 후 타입 안정화 |
| Middleware 타입 지정 | `RequestHandler`, `ErrorRequestHandler` 적용 |

### 💡 느낀 점 / 배운 점:

- JS 기반 Express 프로젝트도 단계별로 타입 적용 가능
- Controller/Service/Repository/Middleware 레이어별 타입 지정 중요
- 환경 변수 타입 처리 시 단언(`!`) 필요
- 타입스크립트 마이그레이션 완료 후 IDE 자동완성과 타입 안정성 확보 가능
- ErrorRequestHandler 적용으로 에러 미들웨어 안정화

### 🏷️ 키워드 (태그):

`#Express` `#TypeScript` `#마이그레이션` `#Controller` `#Service` `#Repository` `#Middleware` `#타입적용` `#환경변수단언`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-12-02 | Express 유저 인증 시스템 TS 마이그레이션 | Controller/Service/Repository/Middleware 타입 적용, 환경 변수 단언 처리, ErrorRequestHandler 적용 | 타입스크립트 환경 설정, Controller/Service/Repository/Middleware 파일 변환, 타입 오류 해결, 서버 재시작 | validateEmailAndPassword 타입 오류 해결, JWT_SECRET 단언, 불필요한 return 제거, allowJS 주석 처리 후 TS 서버 재시작 | `Express` `TypeScript` `마이그레이션` `Controller` `Service` `Repository` `Middleware` `타입적용` `환경변수단언` | ✅ |
