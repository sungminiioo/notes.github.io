# 쿠키 인증 & Authorization 헤더 인증 비교

### 📅 날짜:

> 2025.10.01 (수)
> 

### 📘 오늘 공부한 주제:

> 쿠키 인증 절차와 보안 속성 (Secure, HttpOnly, SameSite)
> 

---

## 📝 핵심 개념 요약

- **쿠키 인증**: 서버가 발급한 인증서를 `Set-Cookie`로 내려주고, 브라우저가 자동으로 요청 시 쿠키를 포함시킴
- **쿠키 보안 속성**
    - `Secure`: HTTPS 연결에서만 전송
    - `HttpOnly`: JS 접근 차단 (XSS 방어)
    - `SameSite`: CSRF 방지 (Strict / Lax / None 옵션)
- **Authorization 헤더 인증**: 클라이언트가 인증서를 직접 저장(주로 localStorage) 후 `Authorization: Bearer <token>` 형태로 서버 요청에 포함
- **Basic 인증**: email:password를 Base64 인코딩하여 전송 (현대 웹에서는 거의 사용 X)
- **Bearer 인증**: JWT 등 토큰 기반 인증, OAuth2.0에서도 활용

## 📊 핵심 요약 표

| 구분 | 쿠키 인증 | Authorization 헤더 인증 |
| --- | --- | --- |
| 저장 위치 | 브라우저 쿠키 (자동 전송) | localStorage / sessionStorage (수동 전송) |
| 인증서 전달 | `Set-Cookie` 응답 후 자동 포함 | `Authorization` 헤더에 직접 포함 |
| 장점 | 자동 관리, 편리 | 도메인 제약 적음, 개발자가 제어 가능 |
| 단점 | CSRF/XSS 취약 → 보안 속성 필요 | 저장소 탈취 위험(localStorage) |
| 보안 속성 | Secure / HttpOnly / SameSite | HTTPS 필수, 토큰 만료 전략 필요 |
| 대표 형식 | 세션 쿠키 | `Bearer <token>` |

### 💻 실습 내용 정리

- 브라우저 개발자도구 → Application → Cookies에서 쿠키 확인
- Express에서 `res.cookie('authToken', token, { httpOnly, secure })` 설정
- Authorization 헤더에 `Bearer <token>` 직접 추가해서 요청

### ❗ 헷갈렸던 점 / 문제 해결:

- 쿠키는 단순 저장소일 뿐, 보안 속성을 꼭 붙여야 안전
- localStorage에 토큰 저장 시 XSS 취약 가능 → 필요 시 `httpOnly 쿠키` 사용 고려
- Basic 인증은 단순하지만 보안성 낮아 HTTPS + 토큰 기반 인증으로 대체

### 💡 느낀 점 / 배운 점:

- 쿠키 인증은 자동 관리라 편리하지만 보안 속성을 잘 활용해야 함
- Authorization 헤더 인증은 개발자가 제어 가능해 유연하지만 보안 관리 책임이 클 수 있음
- Bearer vs Basic의 차이를 이해하면서 왜 현대 웹이 토큰 기반으로 넘어왔는지 알게 됨

### 🏷️ 키워드 (태그):

`#쿠키인증` `#Authorization헤더` `#Secure` `#HttpOnly` `#SameSite` `#Bearer` `#Basic` `#JWT` `#OAuth2`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-10-01 | 쿠키 vs 헤더 인증 | 쿠키=자동 전송, 보안속성 필요 / 헤더=수동 전송, 유연성↑ | 쿠키 속성 설정, Authorization 헤더 추가 | 쿠키=편리하지만 보안취약 / 헤더=제어가능하지만 관리책임↑ | `#쿠키인증` `#헤더인증` `#JWT` | ✅ |
