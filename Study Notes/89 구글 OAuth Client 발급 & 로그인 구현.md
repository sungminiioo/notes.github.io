# 구글 OAuth Client 발급 & 로그인 구현

### 📅 날짜:

> 2025.11.07 (금)
> 

### 📘 오늘 공부한 주제:

> Google OAuth 2.0 + OIDC 기반 로그인 구현
> 

---

## 📝 핵심 개념 요약

- **OAuth 2.0:** 제3자 인증(권한 위임) 프로토콜 — 서비스가 비밀번호 없이 인증 가능
- **OIDC (OpenID Connect):** OAuth 2.0 위에서 *사용자 신원 정보(ID Token)* 까지 제공하는 확장 프로토콜
- **Client ID / Secret:** 앱의 고유 식별자 및 비밀키
- **Redirect URI:** 인증 후 사용자 정보를 되돌려받는 콜백 주소
- **Passport Google Strategy:** 구글 OAuth 인증을 Express에 연결해주는 미들웨어

## 📊 핵심 요약 표

| 구분 | 설명 |
| --- | --- |
| OAuth | 권한 위임 프로토콜, Access Token 발급용 |
| OIDC | OAuth 확장판 — 사용자 신원 정보(ID Token) 제공 |
| Client ID / Secret | 구글에서 발급받은 앱 고유 식별 정보 |
| Redirect URI | 인증 성공 후 이동할 콜백 주소 |
| passport-google-oauth20 | 구글 OAuth 인증을 위한 Passport 전략 |
| scope | 접근 권한 (profile, email 등) |
| provider / providerId | 소셜 로그인 제공자 및 고유 ID |
| oauthCreateOrUpdate | 사용자 존재 시 update, 없으면 create 수행 |

### 💻 실습 내용 정리

### ✅ 1. 구글 OAuth Client ID 발급

1. GCP(구글 클라우드 플랫폼) 접속 → 프로젝트 생성
2. **API 및 서비스 → OAuth 동의화면 설정**
    - 사용자 유형: 외부
    - 권한 범위(scope): `email`, `profile`, `openid`
3. **OAuth 클라이언트 생성**
    - 유형: 웹 애플리케이션
    - 승인된 원본: `http://localhost:3000`
    - 리디렉션 URI: `http://localhost:3000/auth/google/callback`
4. 발급된 **Client ID / Secret**을 `.env` 파일에 추가
    
    ```bash
    GOOGLE_CLIENT_ID=발급받은ID
    GOOGLE_CLIENT_SECRET=발급받은SECRET
    ```
    

---

### ✅ 2. 구글 OAuth 로그인 구현

**(1) User 모델 수정 (prisma/schema.prisma)**

```
provider     String   @default("local")
providerId   String?  @unique
```

> npm run migrate 실행 후 DB 갱신
> 

**(2) userService.js — `oauthCreateOrUpdate` 함수 작성**

- 이메일 기준으로 유저 검색
- 없으면 새로 생성, 있으면 provider 정보 업데이트

```jsx
async function oauthCreateOrUpdate(provider, providerId, email, name) {
  const existingUser = await userRepository.findByEmail(email);
  if (existingUser) {
    const updatedUser = await userRepository.update(existingUser.id, {
      provider,
      providerId,
      name,
    });
    return filterSensitiveUserData(updatedUser);
  } else {
    const createdUser = await userRepository.create({
      email,
      name,
      provider,
      providerId,
    });
    return filterSensitiveUserData(createdUser);
  }

```

**(3) googleStrategy.js 생성**

```jsx
import GoogleStrategy from 'passport-google-oauth20';
import userService from '../../services/userService.js';

const googleStrategyOptions = {
  clientID: process.env.GOOGLE_CLIENT_ID,
  clientSecret: process.env.GOOGLE_CLIENT_SECRET,
  callbackURL: '/auth/google/callback'
};

async function verify(accessToken, refreshToken, profile, done) {
  const user = await userService.oauthCreateOrUpdate(
    profile.provider,
    profile.id,
    profile.emails[0].value,
    profile.displayName
  );
  done(null, user);
}

const googleStrategy = new GoogleStrategy(googleStrategyOptions, verify);
export default googleStrategy;
```

**(4) config/passport.js**

```jsx
passport.use('google', googleStrategy);
```

**(5) userController.js**

```jsx
userController.get('/auth/google',
  passport.authenticate('google', { scope: ['profile', 'email'] })
);

userController.get('/auth/google/callback',
  passport.authenticate('google', { failureRedirect: '/' }),
  (req, res) => res.redirect('/')
);
```

테스트:

👉 `http://localhost:3000/auth/google` 로 접속 → 구글 로그인 창 → 콜백 성공 시 홈으로 이동

### ❗ 헷갈렸던 점 / 문제 해결:

- Redirect URI 불일치 에러
→GCP 설정의 URI(`http://localhost:3000/auth/google/callback`)과 코드의 callbackURL 일치시킴
- providerId 중복 에러
→prisma 모델에서 `providerId`를 `unique`로 설정해 중복 방지
- 신규/기존 유저 구분 로직 불명확
→ `findByEmail` 후 존재 여부에 따라 `create` 또는 `update` 처리

### 💡 느낀 점 / 배운 점:

- OAuth는 로그인뿐 아니라 “신원 위임” 구조임을 이해함
- OIDC를 통해 ID Token을 함께 받아 **인증 + 인가 통합** 가능
- Passport는 전략만 바꾸면 세션, 토큰, OAuth 등 다양한 인증 흐름을 쉽게 전환 가능
- 소셜 로그인은 단순 편의 기능이 아니라 **서비스 진입 장벽을 낮추는 UX 요소**임을 체감

### 🏷️ 키워드 (태그):

`#OAuth2.0` `#OIDC` `#Passport` `#GoogleLogin` `#OAuthClientID` `#passport-google-oauth20` `#RedirectURI` `#provider` `#소셜로그인` `#Node.js`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-11-07 | 구글 OAuth Client ID 발급 로그인 구현 | OIDC 구성요소(Client ID/Secret/Redirect URI) 이해 | oauthCreateOrUpdate 함수, passport 등록, /auth/google 테스트 | Redirect URI 불일치 오류 해결 | `#OAuth` `#OIDC` `#GCP``#Passport` `#GoogleLogin` `#OAuth2.0` | ✅ |
