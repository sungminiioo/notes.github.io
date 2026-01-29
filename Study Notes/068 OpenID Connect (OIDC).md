OpenID Connect (OIDC)

### 📅 날짜:

> 2025.10.09 (목)
> 

### 📘 오늘 공부한 주제:

> OIDC 개념, OAuth와의 차이, id token 활용, 소셜 로그인 인증
> 

---

## 📝 핵심 개념 요약

- **OIDC (OpenID Connect)**: OAuth 기반에 **인증(Authentication)** 기능 추가
- **주요 특징**
    - 하나의 아이디로 여러 사이트에서 인증 가능
    - 사용자가 직접 이메일/비밀번호 입력 불필요 (소셜 로그인 가능)
    - OAuth의 access token 외에 **id token (JWT)** 제공 → 사용자 프로필 정보 포함
- **OAuth vs OIDC**
    - OAuth: 접근 권한 위임 (Authorization)
    - OIDC: 접근 권한 위임 + 인증 (Authentication)
- **id token 사용**
    - JWT 형식, 공개키 방식으로 쉽게 복호화
    - 회원가입/프로필 정보 확인 등 인증서로 활용 가능

## 📊 핵심 요약 표

| 구분 | OAuth | OIDC |
| --- | --- | --- |
| 목적 | 접근 권한 위임 (Authorization) | 접근 권한 + 인증 (Authentication) |
| 토큰 | access token | access token + id token |
| id token | 없음 | 있음 (JWT, 사용자 프로필 포함) |
| 사용자 인증 | 별도 처리 필요 | id token으로 인증 가능 |
| 로그인 방식 | 이메일/비밀번호 직접 입력 필요 | 소셜 로그인 가능, 별도 로그인 필요 없음 |
| 활용 | API 접근 제어 | 회원가입, 프로필, 인증서 활용 |

### 💻 실습 내용 정리

- 사용자가 **소셜 로그인 버튼 클릭** → 프론트 OIDC 프로바이더로 리다이렉트
- 사용자 로그인 성공 → **Authorization Code** + **id token** 발급
- 프론트는 Authorization Code를 백엔드로 전달
- 백엔드: Authorization Code + Client Secret으로 **access token 요청**
- 백엔드 → OIDC 프로바이더: access token + id token 발급
- id token JWT를 복호화 → 사용자 프로필 정보 확인
- 회원가입, 프로필 동기화 등 인증 처리 완료

### ❗ 헷갈렸던 점 / 문제 해결:

- **OAuth와 OIDC의 차이 혼동**
    
    → OAuth는 접근 권한 위임만, OIDC는 인증까지 포함
    
- **id token 활용**
    
    → JWT 형태, 공개키로 복호화 가능, 별도 secret key 불필요
    

### 💡 느낀 점 / 배운 점:

- 소셜 로그인이나 Single Sign-On(SSO)에서 OIDC가 핵심
- OAuth 기반이므로 기존 OAuth 라이브러리 활용 가능
- id token으로 인증까지 한 번에 처리 가능 → 개발 편의성↑

### 🏷️ 키워드 (태그):

`#OIDC` `#OpenIDConnect` `#OAuth` `#idToken` `#JWT` `#Authentication` `#소셜로그인` `#SSO` `#AccessToken`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-10-09 | OpenID Connect (OIDC) | OAuth 기반 인증 표준, access token + id token 제공, 소셜 로그인 가능 | Authorization Code + id token 발급 → 백엔드에서 access token 발급 → id token 복호화 후 사용자 인증 | OAuth vs OIDC 차이 이해, id token 활용법 숙지 | `#OIDC` `#OAuth` `#idToken` `#JWT` `#Authentication` `#SSO` | ✅ |
