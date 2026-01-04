# JWT 기반 인증 Refresh Token & 슬라이딩 세션

### 📅 날짜:

> 2025.11.04 (화)
> 

### 📘 오늘 공부한 주제:

> Access Token 재발급 및 Refresh Token 보안 설계, 슬라이딩 세션 구현
> 

---

## 📝 핵심 개념 요약

- JWT 인증에서 **Access Token**은 짧은 수명(보안 강화),
**Refresh Token**은 긴 수명(UX 개선)을 위한 토큰입니다.
Access Token이 만료되면 Refresh Token을 이용해 새 Access Token을 발급받습니다.
또한, 사용 중 자동 갱신되는 슬라이딩 세션(Sliding Session)을 적용해
UX와 보안의 균형을 잡습니다.

## 📊 핵심 요약 표

| 구분 | 내용 |
| --- | --- |
| Access Token | 짧은 만료 시간(1시간), API 요청 인증용 |
| Refresh Token | 긴 만료 시간(2주), Access Token 재발급용 |
| DB 저장 이유 | Refresh Token 탈취 방지 및 무효화 가능 |
| 쿠키 전송 방식 | `HttpOnly`, `secure`, `sameSite: none` 설정으로 JS 접근 차단 |
| 슬라이딩 세션 | 유저 활동 중 자동으로 Access/Refresh Token 갱신 처리 |

### 💻 실습 내용 정리

### ✅ 1. 토큰 발급 로직 통합

- `userService.js`
    
    `createToken(type)` 함수에서 `type`이 `'refresh'`일 때 만료기한 `'2w'`,
    
    아닐 때 `'1h'`로 설정.
    

### ✅ 2. 로그인 시 Refresh Token 발급 및 DB 저장

- `userController.js`
    
    로그인 성공 시:
    
    - Access + Refresh Token 동시 발급
    - `updateUser()`를 통해 DB에 `refreshToken` 저장
    - 응답 시 `Set-Cookie`로 `HttpOnly` 쿠키 전송

```jsx
res.cookie('refreshToken', refreshToken, {
  httpOnly: true,
  sameSite: 'none',
  secure: true,
});
```

### ✅ 3. Prisma 스키마 수정

```
model User {
  id            Int     @id @default(autoincrement())
  email         String  @unique
  password      String
  refreshToken  String?   // optional
}
```

- `npm run migrate` 수행 후 DB 스키마 반영

### ✅ 4. 토큰 갱신 API 구현

```jsx
userController.post('/token/refresh',
  auth.verifyRefreshToken,
  async (req, res, next) => {
    const refreshToken = req.cookies.refreshToken;
    const { userId } = req.auth;
    const accessToken = await userService.refreshToken(userId, refreshToken);
    res.json({ accessToken });
  });
```

### ✅ 5. 슬라이딩 세션 구현

- refreshToken 만료 전 사용 시 자동 갱신 처리
- `/token/refresh` 호출 시 Access + Refresh Token 모두 재발급
- 새 refreshToken은 다시 `Set-Cookie`로 설정

### ❗ 헷갈렸던 점 / 문제 해결:

**문제:**

- 미들웨어 체이닝 순서가 틀리면 요청 흐름이 깨짐
- Refresh Token 검증 시 쿠키 기반 추출 로직 누락

**해결:**

- `verifyRefreshToken` 미들웨어에 `getToken` 옵션 추가
    
    ```jsx
    getToken: (req) => req.cookies.refreshToken
    ```
    
- 토큰 재발급 시 DB 값과 유효성 모두 검증하도록 수정
- 라우트 등록 순서 및 미들웨어 체이닝 순서를 재정렬

### 💡 느낀 점 / 배운 점:

- **Express 미들웨어 체이닝**은 요청 흐름 설계의 핵심임을 실감.
- **JWT 인증 구조**가 단순하지만 보안 설계의 미세 조정(쿠키, DB 저장, 만료 관리 등)이 UX와 보안의 밸런스를 결정한다는 걸 배움.
- “사용자 경험을 해치지 않으면서 보안을 지키는 방법”이 백엔드 설계의 핵심이라는 인사이트 획득.

### 🏷️ 키워드 (태그):

`#JWT` `#AccessToken` `#RefreshToken` `#HttpOnlyCookie` `#Express` `#Middleware` `#슬라이딩세션` `#보안` `#UX`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-11-04 | JWT Refresh Token & Sliding Session | Access Token 재발급, Refresh Token DB 저장 및 쿠키 적용 | createToken 통합, verifyRefreshToken, 쿠키 설정 | 체이닝 순서/쿠키 검증 문제 해결, 보안·UX 균형 이해 | `#JWT` `#Express` `#보안` `#UX` `#슬라이딩세션` | ✅ |
