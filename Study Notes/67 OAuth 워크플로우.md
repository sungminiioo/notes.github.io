OAuth 워크플로우

### 📅 날짜:

> 2025.10.08 (수)
> 

### 📘 오늘 공부한 주제:

> OAuth Authorization Code 플로우와 Implicit 플로우, Frontend-Backend 연계, access token 발급 및 API 접근
> 

---

## 📝 핵심 개념 요약

- **OAuth 워크플로우(Authorization Code 플로우)**: 프론트엔드는 **Authorization Code**만 받아 백엔드에서 access token 발급, 브라우저에 민감 토큰 노출 X
- **Implicit 플로우**: 브라우저에 바로 액세스 토큰 전달, Client Secret 사용하지 않음, 간단하지만 보안 위험 ↑
- **Frontend ↔ Backend 역할 구분**: 프론트는 인증 코드 전달, 백엔드는 토큰 발급 및 보호된 리소스 접근 처리
- **Access Token**: Scope에 따른 권한 포함, API 요청 시 사용

## 📊 핵심 요약 표

| 구분 | Authorization Code 플로우 | Implicit 플로우 |
| --- | --- | --- |
| **토큰 발급 위치** | Backend | Frontend |
| **Client Secret 사용** | O | X |
| **토큰 브라우저 노출** | X | O |
| **보안** | 높음 | 낮음 |
| **구현 난이도** | 높음 | 낮음 |
| **추천 여부** | ✅ 권장 | ⚠️ 제한적 사용 |

### 💻 실습 내용 정리

- 사용자가 **구글 로그인 버튼 클릭** → 프론트 구글 인가 서버 리다이렉트
- 사용자 로그인 성공 → Authorization Code 발급
- 프론트는 Authorization Code를 백엔드로 전달
- 백엔드: Authorization Code + Client ID/Secret으로 access token 요청
- 인가 서버: 검증 후 access token 발급 → 백엔드 전달
- 백엔드 → 클라이언트 API 요청 시 access token 포함하여 구글 캘린더 리소스 접근

### ❗ 헷갈렸던 점 / 문제 해결:

- **프론트와 백엔드의 역할 혼동**
    
    → Authorization Code만 프론트가 받고, 민감한 access token은 백엔드에서만 처리
    
- **Implicit 플로우와 Authorization Code 플로우 차이**
    
    → Implicit은 토큰 브라우저 노출로 위험 ↑, Authorization Code가 보안상 권장
    

### 💡 느낀 점 / 배운 점:

- OAuth는 **권한 위임 + 보안**을 모두 고려한 구조
- 백엔드가 토큰 발급과 보호된 API 접근을 담당함으로써 **클라이언트 노출 최소화** 가능
- 실제 서비스 연동 시 Authorization Code 플로우가 표준 권장 사항임

### 🏷️ 키워드 (태그):

`#OAuth` `#AuthorizationCode` `#ImplicitFlow` `#AccessToken` `#Scope` `#Frontend` `#Backend` `#보안` `#API연동`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-10-08 | OAuth 워크플로우 | Authorization Code 플로우: 백엔드에서 토큰 발급, 브라우저에 노출 X | 구글 로그인 → Authorization Code → 백엔드 → access token → API 요청 | 프론트/백엔드 역할 명확화, Implicit과 차이 이해 | `#OAuth` `#AuthorizationCode` `#AccessToken` `#보안` | ✅ |
