Authorization 헤더와 인증 사용 이유

### 📅 날짜:

> 2025.10.06 (월)
> 

### 📘 오늘 공부한 주제:

> `Authorization` 헤더와 인증(Authentication) / 인가(Authorization) 개념 구분 및 표준 사용 이유
> 

---

## 📝 핵심 개념 요약

- `Authorization` 헤더는 원래 **인가(Authorization)** 목적이지만, 현재는 **인증(Authentication) 정보(토큰 등)**를 담는 데도 사용됨.
- 표준(RFC 7235)에 명시된 헤더이며, 이미 광범위하게 쓰이고 있어 **호환성 문제 때문에 변경 불가**.
- 결국 `Bearer Token` 등 인증 데이터를 전달하는 방식으로 자리 잡음.
- 이름과 실제 역할이 다소 불일치하나, 표준을 따르는 것이 가장 현실적임.

## 📊 핵심 요약 표

| 구분 | 내용 |
| --- | --- |
| **인증(Authentication)** | 사용자가 누구인지 확인 (예: 로그인, 토큰 검증) |
| **인가(Authorization)** | 인증된 사용자가 어떤 권한을 가졌는지 확인 |
| **Authorization 헤더** | 원래 인가용 헤더지만, 현재는 인증 토큰 전달 용도로 표준화 |
| **사용 이유** | 표준(RFC 7235) 기반, 광범위하게 이미 사용, 호환성 문제 방지 |
| **핵심 포인트** | 이름은 `Authorization`이지만 실제론 인증 정보 포함 |

### 💻 실습 내용 정리

```
DELETE http://www.example.com/api/some_resource/1/
Authorization: Bearer <token>
```

- `Authorization` 헤더에 **Bearer 토큰** 포함 → 서버에서 토큰 검증 후 권한 확인
- 표준에 따라 대부분의 REST API, OAuth2, JWT 인증 방식에서 동일하게 사용됨

### ❗ 헷갈렸던 점 / 문제 해결:

- 왜 “Authorization(인가)”라는 이름의 헤더에 인증 데이터를 담는가 → **표준 역사와 호환성 유지 때문**
- 용어 혼란은 존재하지만, 개발자는 `Authorization 헤더 = 인증 토큰 전달`로 이해하고 사용하면 됨

### 💡 느낀 점 / 배운 점:

- 실제 개발에서는 "이름"보다는 "표준과 호환성"이 더 중요하다는 걸 알게 됨
- Authorization 헤더는 **인증 + 인가를 모두 아우르는 용도**로 받아들이는 게 현실적임

### 🏷️ 키워드 (태그):

`#HTTP` `#Authorization` `#Authentication` `#BearerToken` `#RFC7235` `#API보안`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-10-06 | Authorization 헤더 인증 사용 이유 | 인가 헤더지만 인증 데이터 담아 표준화됨 | Bearer 토큰 예시 요청 | 용어 혼란 있으나 표준과 호환성 유지가 핵심 | `#HTTP` `#Authorization` `#Authentication` | ✅ |
