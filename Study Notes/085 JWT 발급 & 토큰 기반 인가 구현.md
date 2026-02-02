# JWT 발급 & 토큰 기반 인가 구현

### 📅 날짜:

> 2025.11.03 (월)
> 

### 📘 오늘 공부한 주제:

> JWT(Json Web Token)를 이용한 인증 및 인가 처리
> 

---

## 📝 핵심 개념 요약

- JWT는 사용자의 인증 정보를 **서버 상태 저장 없이(stateless)** 유지할 수 있는 토큰 기반 인증 방식이다.
    
    로그인 성공 시 서버가 `accessToken`을 발급하고, 클라이언트는 이후 요청마다 `Authorization: Bearer {token}` 헤더를 통해 인증을 수행한다.
    
    세션과 달리 서버에 상태를 저장하지 않아 확장성이 높다.
    

## 📊 핵심 요약 표

| 개념 | 설명 |
| --- | --- |
| **JWT** | Json Web Token, 인증용 서명 토큰 |
| **jsonwebtoken** | JWT 생성 및 검증 라이브러리 |
| **express-jwt** | JWT 검증을 자동 처리하는 미들웨어 |
| **accessToken** | 인증 후 클라이언트가 보관하는 토큰 |
| **secret (JWT_SECRET)** | 토큰 서명에 사용되는 비밀 키 |
| **Bearer 토큰** | HTTP Authorization 헤더를 통한 인증 방식 |
| **401 Unauthorized** | 토큰이 없거나 유효하지 않을 때의 응답 코드 |

### 💻 실습 내용 정리

### ✅ 1. JWT 발급하기

```bash
npm i jsonwebtoken
```

**src/services/userService.js**

```jsx
import jwt from "jsonwebtoken";

function createToken(user) {
  const payload = { userId: user.id };
  const secret = process.env.JWT_SECRET;
  const options = { expiresIn: "1h" }; // 토큰 만료시간 설정
  return jwt.sign(payload, secret, options);
}
```

**src/controllers/userController.js**

```jsx
userController.post("/login", async (req, res, next) => {
  const { email, password } = req.body;
  try {
    const user = await userService.getUser(email, password);
    const accessToken = userService.createToken(user);
    res.json({ ...user, accessToken });
  } catch (error) {
    next(error);
  }
});
```

🧠 **결과:**

로그인 시 `accessToken`이 응답에 포함됨

(기존 세션 기반 로그인에서 sid 대신 accessToken이 발급)

```json
{
  "id": 3,
  "email": "test1@test.com",
  "name": "test",
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5c..."
}
```

---

### ✅ 2. 토큰 기반 인가 구현

```bash
npm i express-jwt
```

**src/middlewares/auth.js**

```jsx
import { expressjwt } from "express-jwt";

export const verifyAccessToken = expressjwt({
  secret: process.env.JWT_SECRET,
  algorithms: ["HS256"]
});
```

**src/middlewares/errorHandler.js**

```jsx
if (error.name === "UnauthorizedError") {
  res.status(401).send("invalid token...");
}
```

**src/controllers/reviewController.js**

```jsx
import { verifyAccessToken } from "../middlewares/auth.js";

reviewController.post(
  "/reviews",
  verifyAccessToken,
  async (req, res, next) => {
    // 유효한 토큰일 경우에만 리뷰 생성
  }
);
```

**테스트**

```bash
POST http://localhost:3000/reviews
Content-Type: application/json
Authorization: Bearer {{token}}

{
  "title": "Great Product",
  "description": "Very good!",
  "rating": 5,
  "productId": 3
}
```

✅ 토큰 없으면 → `401 invalid token...`

✅ 토큰 있으면 → 리뷰 등록 성공

---

### ✅ 3. 본인 리뷰 여부 검증 (추가 인가)

**src/middlewares/auth.js**

```jsx
async function verifyReviewAuth(req, res, next) {
  const review = await reviewRepository.findById(req.params.id);
  if (review.userId !== req.auth.userId) {
    return res.status(403).send("Forbidden: not your review");
  }
  next();
}
```

**src/controllers/reviewController.js**

```jsx
reviewController.put(
  "/reviews/:id",
  verifyAccessToken,
  verifyReviewAuth,
  updateReview
);

reviewController.delete(
  "/reviews/:id",
  verifyAccessToken,
  verifyReviewAuth,
  deleteReview
);
```

### ❗ 헷갈렸던 점 / 문제 해결:

- **헷갈렸던 점:**
    - JWT가 세션과 어떤 점에서 다른지
    - express-jwt 미들웨어 사용 시 에러 처리 방식
    - Bearer 토큰이 헤더에 없을 때 발생하는 오류 구조
- **문제 해결:**
    - 세션은 서버가 상태를 저장하지만 JWT는 **서버 상태를 저장하지 않음(stateless)**
    - express-jwt에서 검증 실패 시 `UnauthorizedError`로 전달되며 errorHandler에서 처리
    - Bearer 토큰 미포함 → `credentials_required` 코드로 401 응답
    - `verifyReviewAuth`를 추가하여 사용자 권한 검증까지 명확히 구현함

### 💡 느낀 점 / 배운 점:

- 세션과 달리 JWT는 서버 부하가 적고, **확장성 있는 인증 구조**를 만든다는 점을 이해함.
- 미들웨어 체인으로 **로그인 → 토큰 발급 → 인가 검증** 흐름이 자연스럽게 연결됨을 체감함.
- JWT 구조(header.payload.signature)를 직접 확인하며 암호화 원리를 시각적으로 이해할 수 있었음.
- 토큰 기반 시스템은 클라이언트 단에서 상태를 유지하기 때문에 **SPA(React 등)**과 궁합이 좋다는 점도 알게 됨.

### 🏷️ 키워드 (태그):

`#JWT` `#jsonwebtoken` `#express-jwt` `#accessToken` `#Bearer` `#인증` `#인가` `#미들웨어` `#401` 
`#토큰기반인증`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-11-03 | JWT 발급 & 토큰 기반 인가 구현 | 로그인 성공 시 JWT 발급 후 Authorization 헤더를 통해 인가 검증 | JWT 발급 로직 구현 → express-jwt 적용 → 리뷰 엔드포인트에 토큰 검증 추가 | 세션과 JWT 차이 명확히 이해, express-jwt 에러 구조 파악, 토큰 기반 인증 흐름 완성 | `#JWT` `#express-jwt` `#accessToken` `#인증` `#인가` | ✅ |
