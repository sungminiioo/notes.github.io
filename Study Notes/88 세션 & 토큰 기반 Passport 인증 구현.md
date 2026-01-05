# 세션 & 토큰 기반 Passport 인증 구현

### 📅 날짜:

> 2025.11.06 (목)
> 

### 📘 오늘 공부한 주제:

> 세션 기반 인증 & JWT 기반 인증 (Passport Local / Passport JWT)
> 

---

## 📝 핵심 개념 요약

- **Passport**는 인증 전략을 쉽게 연결할 수 있는 미들웨어 프레임워크
- **Local Strategy:** 이메일 + 비밀번호 기반 로그인
- **serializeUser / deserializeUser:** 세션 저장과 사용자 복원
- **req.isAuthenticated():** 세션 로그인 여부 확인
- **JWT Strategy:** Access Token / Refresh Token 기반 인증
- **passport.authenticate():** 전략 기반으로 인증 흐름 처리

## 📊 핵심 요약 표

| 구분 | 설명 |
| --- | --- |
| LocalStrategy | 이메일과 비밀번호로 사용자 인증 |
| serializeUser | 로그인 성공 시 user.id를 세션에 저장 |
| deserializeUser | 세션에서 user.id로 사용자 정보 복원 |
| JWTStrategy | JWT 토큰을 검증해 인증 수행 |
| Access Token | 짧은 유효기간, 매 요청마다 인증용 |
| Refresh Token | 장기 유효기간, Access Token 재발급용 |
| passport.initialize() | Passport 초기화 미들웨어 |
| passport.session() | 세션 기반 인증 유지용 미들웨어 |

### 💻 실습 내용 정리

### ✅ 1. 세션 기반 Passport 구현 (LocalStrategy)

1. **`localStrategy.js` 작성**
    - `passport-local`의 `Strategy` 사용
    - `userService.getUser(email, password)`로 유효 사용자 확인
    - 성공 시 `done(null, user)` / 실패 시 `done(null, false)`
2. **`passport.js` 설정**
    - `passport.use(localStrategy)` 등록
    - `serializeUser`, `deserializeUser`로 세션 관리
3. **`app.js`**
    - `passport.initialize()` + `passport.session()` 활성화
4. **컨트롤러 적용**
    - `/session-login`에 `passport.authenticate("local")` 적용
    - `validateEmailAndPassword` 미들웨어 추가
5. **`verifySessionLogin` 리팩터링**
    - `req.isAuthenticated()`로 인증 확인
    - 로그인 후 세션 기반 제품 추가 테스트 성공

### ✅ 2. 토큰 기반 Passport 구현 (JWTStrategy)

1. **`getUserById` 메서드 추가 (userService.js)**
    - `userRepository.findById()`로 유저 조회
2. **`jwtStrategy.js` 작성**
    - `accessTokenOptions` (Bearer 토큰)
    - `refreshTokenOptions` (쿠키 기반)
    - `jwtVerify` 내부에서 `getUserById` 호출
3. **`passport.js` 등록**
    - `passport.use('access-token', accessTokenStrategy)`
    - `passport.use('refresh-token', refreshTokenStrategy)`
4. **리뷰 수정 인증 리팩터링**
    - `passport.authenticate('access-token', { session: false })` 적용
5. **토큰 갱신 리팩터링**
    - `passport.authenticate('refresh-token')` 사용
    - `req.user`에 { id, email, name, createdAt, updatedAt } 구조 확인

### ❗ 헷갈렸던 점 / 문제 해결:

- `done()` 콜백의 인자 순서가 헷갈렸음
→ `done(error, user)` 구조이며, 오류는 첫 번째, 성공 데이터는 두 번째 인자
- `serializeUser` / `deserializeUser` 개념이 명확하지 않음
→ 세션 저장(`serializeUser`) → 요청 시 사용자 복원(`deserializeUser`)의 순서로 작동함
- JWT 전략에서 토큰 추출 위치 혼동
→ Access Token은 `Authorization Header`, Refresh Token은 `쿠키`에서 추출

### 💡 느낀 점 / 배운 점:

- Passport는 미들웨어 체이닝이 깔끔하게 되어 인증 로직을 분리하기 쉽다.
- 세션 기반과 JWT 기반 모두 **“상태 관리”의 방식**이 다를 뿐 핵심 원리는 동일하다.
- 인증 로직을 서비스/미들웨어/컨트롤러로 분리하니 유지보수성이 높아졌다.
- `passport.authenticate()` 한 줄로 인증 전략을 명확히 표현할 수 있어 코드 가독성이 좋아짐.

### 🏷️ 키워드 (태그):

`#Passport` `#LocalStrategy` `#JWT` `#세션기반인증` `#AccessToken` `#RefreshToken` `#serializeUser` `#deserializeUser` `#passport.authenticate` `#Node.js` `#Expres`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-11-06 | 세션, 토큰 기반 Passport 인증 | JWTStrategy / Access & Refresh Token / passport.authenticate | 세션 로그인 및 제품 추가 인증JWT 인증 및 토큰 갱신 리팩터링 | AccessToken/RefreshToken 추출 방식 명확화 | `#JWT` `#TokenAuth` `#passport-jwt``#Passport` `#LocalStrategy` | ✅ |
