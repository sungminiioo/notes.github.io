# 카카오 OAuth 클라이언트 등록 및 구현

### 📅 날짜:

> 2025.11.10 (월)
> 

### 📘 오늘 공부한 주제:

> 카카오 OAuth Client ID 발급 및 인증 구현
> 

--- 

## 📝 핵심 개념 요약

- OAuth는 **사용자 인증을 제3자 서비스로 위임**하는 방식
- Kakao Developers에서 애플리케이션 등록 후 **Client ID / Secret 발급**
- `passport-kakao`를 이용해 **카카오 로그인 인증 전략 구현**
- 로그인 성공 시 카카오 프로필 정보를 바탕으로 **DB 내 유저 생성/업데이트**

## 📊 핵심 요약 표

| 구분 | 내용 |
| --- | --- |
| **OAuth** | 인증 위임 방식 (Access Token으로 인증) |
| **필요 요소** | Client ID, Client Secret, Redirect URI |
| **라이브러리** | `passport-kakao` |
| **콜백 처리** | `/auth/kakao/callback` 에서 accessToken으로 사용자 정보 검증 |
| **DB 처리** | `userService.oauthCreateOrUpdate()` 사용 |
| **scope** | `['profile_nickname', 'account_email']` 로 닉네임/이메일 허용 |

### 💻 실습 내용 정리

```bash
git checkout -b 12_google-auth
git reset --hard origin/12_google-auth
npm install passport-kakao
```

1. **카카오 개발자 가입 및 앱 등록**
    - 앱 생성 후 “카카오 로그인” 활성화 및 Redirect URI 설정
    - OpenID Connect 활성화
    - 비즈 앱 전환으로 이메일 접근 허용
2. **Client ID, Secret 발급**
    - REST API 키 → `.env` 의 `KAKAO_CLIENT_ID`
    - 보안 설정 → Client Secret 생성 후 `.env` 의 `KAKAO_CLIENT_SECRET`
3. **카카오 인증 전략 구현**
    
    ```jsx
    import { Strategy as KakaoStrategy } from "passport-kakao";
    import userService from "../../services/userService.js";
    
    const kakaoStrategy = new KakaoStrategy({
      clientID: process.env.KAKAO_CLIENT_ID,
      clientSecret: process.env.KAKAO_CLIENT_SECRET,
      callbackURL: "/auth/kakao/callback",
    }, async (accessToken, refreshToken, profile, done) => {
      const user = await userService.oauthCreateOrUpdate(
        profile.provider,
        profile.id.toString(),
        profile._json.kakao_account.email,
        profile.displayName || profile._json.properties.nickname
      );
      done(null, user);
    });
    
    export default kakaoStrategy;
    ```
    
4. **passport 등록 및 라우터 연결**
    
    ```jsx
    // src/config/passport.js
    import kakaoStrategy from "../middlewares/passport/kakaoStrategy.js";
    passport.use(kakaoStrategy);
    ```
    
5. **라우터 작성**
    
    ```jsx
    userController.get('/auth/kakao',
      passport.authenticate('kakao', { scope: ['profile_nickname', 'account_email'] })
    );
    
    userController.get('/auth/kakao/callback',
      passport.authenticate('kakao', { failureRedirect: '/login' }),
      (req, res) => res.redirect('/')
    );
    ```
    
6. **테스트**
    - 브라우저에서 `http://localhost:3000/auth/kakao` 접속
        
        → 카카오 로그인 화면으로 리디렉션
        
        → 로그인 성공 시 user 정보 반환 확인
        

### ❗ 헷갈렸던 점 / 문제 해결:

- 이메일 정보가 null로 들어오는 문제
→ 비즈 앱 전환 후 “이메일 필수 동의” 설정으로 해결
- Redirect URI mismatch 에러
→ Kakao Developers 설정의 Redirect URI와 서버 콜백 경로 불일치 → 수정

### 💡 느낀 점 / 배운 점:

- OAuth 구조를 직접 구현해보며 “Access Token을 통해 인증이 이루어지는 흐름”을 명확히 이해함
- Google OAuth와 유사하지만, Kakao는 **비즈 앱 승인 과정이 필요**하다는 점이 다름
- 실제 서비스 연동 시 설정 세부사항(redirect URI, scope 등)이 매우 중요함을 깨달음

### 🏷️ 키워드 (태그):

`#OAuth` `#KakaoLogin` `#passport-kakao` `#AccessToken` `#ClientID` `#OAuthImplementation`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-11-10 | 카카오 OAuth 구현 | Kakao Developers를 통한 Client ID 발급 및 passport-kakao 인증 전략 구현 | 앱 등록 → Client ID/Secret 발급 → kakaoStrategy 설정 → /auth/kakao 테스트 | 이메일 동의 문제 해결 / OAuth 구조 명확히 이해 | `#OAuth` `#Kakao` `#passport` | ✅ |
