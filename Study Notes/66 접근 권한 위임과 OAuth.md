접근 권한 위임과 OAuth

### 📅 날짜:

> 2025.10.07 (화)
> 

### 📘 오늘 공부한 주제:

> 접근 권한 위임(Delegated Access)과 OAuth 표준, Client ID/Secret, Scope 설정
> 

---

## 📝 핵심 개념 요약

- **접근 권한 위임**: 한 서비스가 다른 서비스의 보호된 리소스에 접근할 권한을 **직접 인증정보를 받지 않고** 얻는 것
- OAuth(Open Authorization): **인증정보를 제3자에게 제공하지 않고**, 제3자가 보호된 리소스에 접근할 수 있게 하는 표준
- OAuth 구성 주체 5가지: **Resource Owner, Client, Authorization Server, Resource Server, Redirect URI**
- Client ID / Client Secret: OAuth 등록 시 발급, **내 웹서비스를 인가 서버가 식별하도록 함**
- Scope: 최소 권한 원칙, 서비스와 유저 모두를 위해 필요한 권한만 허용

## 📊 핵심 요약 표

| 구분 | 내용 |
| --- | --- |
| **접근 권한 위임** | 다른 서비스 리소스 접근 권한을 안전하게 위임 |
| **OAuth 목적** | 인증정보 없이 제3자가 리소스 접근 가능하게 함 |
| **주체** | Resource Owner, Client, Authorization Server, Resource Server, Redirect URI |
| **Client ID/Secret** | 내 웹서비스를 인가 서버가 식별하도록 발급 |
| **Scope** | 허용 범위, 최소 권한 원칙 |

### 💻 실습 내용 정리

- 내 웹서비스를 구글 인가 서버에 OAuth 등록 → Client ID, Client Secret 발급
- 백엔드 서버에 ID/Secret 저장
- Scope 설정: CRUD 중 필요한 권한만 허용
- 사용자 로그인 → 구글 인증 후 Authorization Code 발급 → 내 서버에서 Access Token 교환 → 보호된 리소스 접근

### ❗ 헷갈렸던 점 / 문제 해결:

- 유저가 직접 비밀번호를 제3자에 넘겨주는 방식 vs OAuth
    
    → OAuth는 **인증정보를 제3자에 전달하지 않고 안전하게 리소스 접근** 가능하게 함
    
- Scope 권한 범위 설정 고민
    
    → 최소 권한 원칙 적용, 유저 동의 필요
    

### 💡 느낀 점 / 배운 점:

- OAuth 덕분에 사용자 인증정보를 안전하게 보호하면서 서비스 연동 가능
- 실제 서비스 연동 시 **Client ID/Secret 관리와 Scope 설정**이 핵심
- 접근 권한 위임 이해하면 OAuth 구조와 흐름이 훨씬 명확

### 🏷️ 키워드 (태그):

`#OAuth` `#접근권한위임` `#DelegatedAccess` `#ClientID` `#ClientSecret` `#Scope` `#API연동` `#보안`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-10-07 | 접근 권한 위임과 OAuth | 인증정보 없이 안전하게 리소스 접근 가능, OAuth 표준 활용 | 구글 OAuth 등록, Client ID/Secret 저장, Scope 설정 후 Access Token으로 API 접근 | 유저 비밀번호를 제3자에게 전달하지 않아도 됨, 최소 권한 원칙 중요 | `#OAuth``#접근권한위임` `#API연동` | ✅ |
