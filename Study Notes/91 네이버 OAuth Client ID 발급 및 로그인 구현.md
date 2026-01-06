# 네이버 OAuth Client ID 발급 및 로그인 구현

### 📅 날짜:

> 2025.11.11 (화)
> 

### 📘 오늘 공부한 주제:

> 네이버 OAuth Client 등록 및 인증 구현
> 

---

## 📝 핵심 개념 요약

- **OAuth 2.0 기반의 네이버 로그인 연동**
- 네이버 개발자 센터에서 **Client ID / Secret 발급**
- `passport-naver` 라이브러리를 이용하여 **OAuth 인증 전략 구현**
- 로그인 성공 시 네이버 프로필 정보(email, nickname)를 이용해 **사용자 DB 생성 또는 갱신**

## 📊 핵심 요약 표

| 구분 | 내용 |
| --- | --- |
| **OAuth 목적** | 외부 서비스(네이버)를 통한 로그인 인증 |
| **필요 요소** | Client ID, Client Secret, Callback URL |
| **라이브러리** | `passport-naver` |
| **콜백 URL** | `/auth/naver/callback` |
| **scope 설정** | `['nickname', 'email']` |
| **DB 처리** | provider, providerId 필드 추가 |

### 💻 실습 내용 정리

```bash
git checkout -b 13_kakao-auth
git reset --hard origin/13_kakao-auth
npm install passport-naver
```

### 1️⃣ 네이버 개발자 등록

- [NAVER Developers](https://developers.naver.com/main/) 접속 후 로그인
- “애플리케이션 등록” → `네이버 로그인` API 선택
- 권한 설정: **이메일, 별명** 필수 항목
- 환경 추가: **PC 웹**,
    - 서비스 URL: `http://localhost:3000`
    - Callback URL: `http://localhost:3000/auth/naver/callback`
- 등록 후 **Client ID / Secret** 발급
    
    → `.env`에 아래처럼 추가
    
    ```
    NAVER_CLIENT_ID=발급받은_ID
    NAVER_CLIENT_SECRET=발급받은_SECRET
    ```
    

---

### 2️⃣ 인증 전략 구현

```jsx
// src/middlewares/passport/naverStrategy.js
import { Strategy as NaverStrategy } from "passport-naver";
import userService from "../../services/userService.js";

const naverStrategyOptions = {
  clientID: process.env.NAVER_CLIENT_ID,
  clientSecret: process.env.NAVER_CLIENT_SECRET,
  callbackURL: "/auth/naver/callback",
};

async function verify(accessToken, refreshToken, profile, done) {
  const user = await userService.oauthCreateOrUpdate(
    profile.provider,
    profile.id,
    profile._json.email,
    profile._json.nickname
  );
  done(null, user); // req.user = user;
}

const naverStrategy = new NaverStrategy(naverStrategyOptions, verify);
export default naverStrategy;
```

---

### 3️⃣ Passport 설정 및 라우터 추가

```jsx
// src/config/passport.js
import naverStrategy from "../middlewares/passport/naverStrategy.js";
passport.use(naverStrategy)
```

```jsx
// src/controllers/userController.js
userController.get(
  '/auth/naver',
  passport.authenticate('naver', { scope: ['nickname', 'email'] })
);

userController.get(
  '/auth/naver/callback',
  passport.authenticate('naver', { failureRedirect: '/login' }),
  (req, res) => res.redirect('/')
);
```

✅ 테스트

> 브라우저에서 http://localhost:3000/auth/naver 접속
> 
> 
> → 네이버 로그인 화면 → 로그인 성공 시 callback 처리 확인
> 

---

### 4️⃣ DB 스키마 수정 (provider 정보 저장)

```jsx
// src/repositories/userRepository.js
async function save(user) {
  return prisma.user.create({
    data: {
      email: user.email,
      name: user.name,
      password: user.password,
      provider: user.provider,
      providerId: user.providerId,
    },
  });
}
```

### ❗ 헷갈렸던 점 / 문제 해결:

- 이메일 정보가 반환되지 않음
→ 네이버 앱 설정에서 “이메일” 항목을 필수 동의로 지정 후 해결
- Redirect URI mismatch 에러
→ Callback URL 등록 누락 → `http://localhost:3000/auth/naver/callback` 추가로 해결

### 💡 느낀 점 / 배운 점:

- Kakao와 유사한 구조지만 **scope 설정 및 API 접근 권한 구조가 다름**
- 소셜 로그인 통합 시 provider 필드 관리가 필수적임을 체감
- OAuth 구조를 여러 플랫폼에서 구현하며 **공통 로직과 차이점 모두 정리 가능**

### 🏷️ 키워드 (태그):

`#OAuth` `#NaverLogin` `#passport-naver` `#AccessToken` `#ClientID` `#소셜로그인`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-11-11 | 네이버 OAuth 구현 | 네이버 OAuth Client ID 발급 후 passport-naver로 인증 구현 | 앱 등록 → Client ID/Secret 발급 → naverStrategy 구현 → 콜백 처리 | 이메일 동의 설정 및 Redirect URI 문제 해결 / OAuth 구조 재이해 | `#OAuth` `#Naver` `#passport` `#소셜로그인` | ✅ |
