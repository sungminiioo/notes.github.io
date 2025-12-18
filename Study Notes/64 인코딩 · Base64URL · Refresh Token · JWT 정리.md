# 인코딩 · Base64URL · Refresh Token · JWT 정리

### 📅 날짜:

> 2025.10.03 (금)
> 

### 📘 오늘 공부한 주제:

> 인코딩, Base64URL, Refresh Token, JWT
> 

---

## 📝 핵심 개념 요약

- **인코딩**: 데이터를 특정 규칙으로 변환해 전송/저장을 쉽게 함 (보안 목적 ❌)
- **ASCII**: 128개 문자만 표현 가능 → 한글·이모지 불가 → Base64 등장
- **Base64**: 64개의 안전한 문자로 데이터 인코딩
- **Base64URL**: URL에서 안전하게 사용하기 위해 `+` → , `/` → `_` 치환
- **Refresh Token**: Access Token 보완용, 긴 수명·HttpOnly 쿠키에 저장
- **JWT (JSON Web Token)**: Header.Payload.Signature 구조의 표준 토큰, Base64URL 인코딩 사용

## 📊 핵심 요약 표

| 구분 | 설명 | 특징 |
| --- | --- | --- |
| 인코딩 | 데이터 형식 변환 | 누구나 디코딩 가능, 보안 목적 아님 |
| ASCII | 기본 문자 인코딩 표준 | 영어만 포함, 확장성 한계 |
| Base64 | 이진 데이터를 64문자로 표현 | = 로 패딩, `+`, `/` 포함 |
| Base64URL | Base64 변형 | URL 안전하게(`+→-`, `/→_`) |
| Refresh Token | Access Token 보완 | 긴 만료기간, HttpOnly 쿠키 저장 |
| JWT | JSON 기반 토큰 | Header.Payload.Signature 구조, Base64URL 인코딩 |

### 💻 실습 내용 정리

- **Base64 인코딩 원리**
- 데이터를 6비트 단위로 나누어 `A-Z, a-z, 0-9, +, /` 로 매핑
- URL 문제 방지를 위해 Base64URL 변형 사용
- **Refresh Token 사용 흐름**
    1. 로그인 시 Access + Refresh Token 발급
    2. Access Token → 로컬스토리지 (짧은 수명)
    3. Refresh Token → HttpOnly 쿠키 (긴 수명, 보안 강화)
    4. Access Token 만료 시 Refresh Token으로 새 토큰 요청
- **JWT 구조**
    - **Header**: 알고리즘, 타입
    - **Payload**: 유저 정보, 발급·만료 시간(exp, iat)
    - **Signature**: Header + Payload + 비밀키 서명 (위조 방지)
    - 서버에서 시그니처 검증 + 만료시간 체크로 유효성 확인

### ❗ 헷갈렸던 점 / 문제 해결:

- 인코딩은 **보안 수단이 아님** → 단순히 호환성 문제 해결
- Refresh Token은 **HttpOnly 쿠키**에 저장해야 안전 (XSS 방지)
- JWT는 Payload가 디코딩 가능 → **민감 정보 저장 금지**

### 💡 느낀 점 / 배운 점:

- 인코딩, 암호화, 해시를 혼동하지 말아야 함
- 현대 인증 시스템은 **Access Token + Refresh Token (JWT 기반)** 조합이 표준
- Base64URL 같은 디테일이 실제 서비스에서 매우 중요하다는 점을 깨달음

### 🏷️ 키워드 (태그):

`#인코딩` `#Base64` `#Base64URL` `#ASCII` `#RefreshToken` `#JWT` `#토큰인증` `#보안`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-10-03 | 인코딩·Base64URL·Refresh Token·JWT | 인코딩=형식변환, Base64/URL=전송 호환성, Refresh=보안 강화, JWT=표준 토큰 | Base64 변환, Refresh Token 발급·저장, JWT 구조 검증 | 인코딩≠보안 / Refresh는 쿠키 / JWT는 민감정보 X → 현대 인증 표준 이해 | `#인코딩` `#Base64` `#Base64URL` `#RefreshToken` `#JWT` | ✅ |
