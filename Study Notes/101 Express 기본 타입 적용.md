# Express 기본 타입 적용

### 📅 날짜:

> 2025.11.28 (금)
> 

### 📘 오늘 공부한 주제:

> Express에서 TypeScript 기본 타입 적용 및 Request 객체 확장
> 

---

## 📝 핵심 개념 요약

- **Express 미들웨어 타입**
    - `Request`, `Response`, `NextFunction`으로 각 매개변수 타입 지정 가능
    - 반복되는 경우 `RequestHandler` 타입을 활용하여 코드 간결화
- **에러 핸들러**
    - `ErrorRequestHandler`를 사용해 에러 미들웨어 타입 지정
- **Request 객체 타입 지정**
    - `req.params`, `req.body`, `req.query`에 제네릭으로 타입 지정 가능
    - 인터페이스를 만들어 구조화하면 코드 안전성 ↑
- **Request 객체 타입 확장**
    - `src/types/express.d.ts`에서 `Express.Request` 인터페이스 확장 가능
    - TS는 같은 이름의 인터페이스를 자동으로 병합(Merge)

## 📊 핵심 요약 표

| 개념 | 설명 |
| --- | --- |
| Request/Response/NextFunction | 각 매개변수에 타입 지정 가능 |
| RequestHandler | 핸들러에 직접 타입 지정 가능, 간결함 |
| ErrorRequestHandler | 에러 미들웨어 타입 지정 |
| req.params | 라우트 파라미터 타입 지정 (`Request<{id: string}>`) |
| req.body | 요청 본문 타입 지정, 인터페이스 활용 권장 |
| req.query | 쿼리 스트링 타입 지정 |
| Request 타입 확장 | `src/types/express.d.ts`에서 커스텀 속성 추가 가능 (`req.valid`) |

### 💻 실습 내용 정리

### ✔ 기본 Request/Response/NextFunction 타입 적용

```tsx
import express, { Request, Response, NextFunction } from "express";

const app = express();

app.get("/", (req: Request, res: Response, next: NextFunction) => {
  res.send("Hello World");
});

app.listen(3000, () => {
  console.log("Server is running on port 3000");
});
```

### ✔ RequestHandler 사용

```tsx
import express, { RequestHandler } from "express";

const app = express();
const handler: RequestHandler = (req, res) => {
  res.send("Hello World");
};

app.get("/", handler);
```

### ✔ ErrorRequestHandler 사용

```tsx
import { ErrorRequestHandler } from "express";

const errorMiddleware: ErrorRequestHandler = (err, req, res, next) => {
  res.status(500).send("Something broke!");
};

app.use(errorMiddleware);
```

### ✔ Request 객체 타입 지정 예시

```tsx
// 라우트 파라미터
app.get('/users/:id', (req: Request<{ id: string }>, res: Response) => {
  const userId = req.params.id;
  res.json({ userId });
});

// 요청 본문
interface CreateUserBody {
  name: string;
  email: string;
  age: number;
}
app.post('/users', (req: Request<{}, {}, CreateUserBody>, res: Response) => {
  const { name, email, age } = req.body;
  res.json({ name, email, age });
});

// 쿼리
app.get(
  "/search",
  (req: Request<{}, {}, {}, { keyword: string; page?: string }>, res: Response) => {
    const { keyword, page } = req.query;
    res.json({ keyword, page });
  },
);
```

### ✔ Request 객체 타입 확장

```tsx
// src/types/express.d.ts
import 'express';

declare global {
  namespace Express {
    interface Request {
      valid?: boolean;
    }
  }
}
```

- 확장 후 미들웨어에서 `req.valid` 사용 가능

### ❗ 헷갈렸던 점 / 문제 해결:

| 문제 | 해결 |
| --- | --- |
| Request/Response 타입 반복 지정이 번거로움 | `RequestHandler`를 사용해 간결하게 처리 |
| req.body, req.query 제네릭 이해 필요 | 인터페이스를 만들어 명시적 타입 지정 |
| Express Request 타입 확장 방법 | global namespace + interface merge 활용 |

### 💡 느낀 점 / 배운 점:

- Express 미들웨어 타입 지정은 개발 생산성과 타입 안전성을 높이는 핵심 기술
- RequestHandler/ ErrorRequestHandler를 활용하면 코드가 훨씬 깔끔해짐
- Request 객체를 타입 확장하면 커스텀 속성 추가도 안전하게 가능
- TypeScript + Express에서 타입 설계는 코드 안정성 및 협업 효율을 크게 개선함

### 🏷️ 키워드 (태그):

`#TypeScript` `#Express` `#RequestHandler` `#ErrorRequestHandler` `#RequestType` `#RequestBody` `#RequestQuery` `#TypeSafety`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-11-28 | Express 기본 타입 적용 | Request/Response/NextFunction 타입 적용, RequestHandler/에러 핸들러, Request 객체 타입 확장 | 기본 핸들러 작성, ErrorRequestHandler 적용, req.params/body/query 타입 지정, 타입 확장 | 타입 반복 문제 → RequestHandler 활용, Request 객체 확장 시 merge 활용 | `TypeScript` `Express` `RequestHandler` `ErrorRequestHandler` `RequestType` `TypeSafety` | ✅ |
