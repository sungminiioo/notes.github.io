# REST API 설계 실수와 개선 방법

### 📅 날짜:

> 2025.10.22 (수)
> 

### 📘 오늘 공부한 주제:

> RESTful API 설계에서 자주 발생하는 실수와 올바른 설계 패턴
> 

---

## 📝 핵심 개념 요약

- REST API는 단순히 데이터 송수신이 아니라, **표현(Representation)을 통한 자원(Resource) 조작**을 명확하게 정의하는 것이 핵심이다.
올바른 **HTTP 메소드**, **상태 코드**, **URI 설계**, **MIME Type 명시**는 API의 품질과 일관성을 결정한다.

## 📊 핵심 요약 표

| 구분 | 잘못된 예시 | 올바른 설계 | 핵심 포인트 |
| --- | --- | --- | --- |
| HTTP 메소드 | `POST /members` (조회용) | `GET /members` | 메소드는 명확한 목적에 맞게 사용 |
| 상태 코드 | 오류 응답에 `200 OK` | `400 Bad Request` | 코드로 성공/실패를 직관적으로 구분 |
| URI 표현 | `/create-members` | `POST /members` | URI는 명사(자원), 메소드는 동사(행동) |
| MIME Type | 생략됨 | `Content-Type: application/json` | 데이터 형식을 명확히 명시해야 파싱 오류 방지 |

### 💻 실습 내용 정리

### ✅ 잘못된 API

```
POST /members
```

→ 멤버 목록 조회용으로 사용 (❌)

### ✅ 개선된 API

```
GET /members
```

→ 조회용은 GET 메소드 사용

### ✅ 상태 코드 개선

```
400 Bad Request
{
  "error": "age must be greater than 0"
}
```

### ✅ URI 정리

```
POST /members        // 생성
PATCH /members/1     // 수정
DELETE /members/2    // 삭제
```

### ✅ MIME Type 명시

```
POST /members
Content-Type: application/json

{
  "name": "Harry"
}
```

### ❗ 헷갈렸던 점 / 문제 해결:

**문제:**

- POST/GET 메소드 구분을 혼동 → 조회용 API를 POST로 작성
- 오류 응답을 200 코드로 반환해 디버깅 어려움
- URI에 동사 표현(`create-`, `update-`)을 혼용

**해결:**

- HTTP 메소드별 역할을 표로 정리해 명확히 구분
- 상태 코드와 메소드 조합을 점검하며 테스트 자동화
- URI는 항상 ‘자원 중심’, 메소드는 ‘행동 중심’으로 일관성 유지

### 💡 느낀 점 / 배운 점:

- RESTful 설계는 단순히 규칙이 아니라 **“의미를 전달하는 언어”**임을 깨달음.
    
    메소드, 상태 코드, URI, MIME Type을 올바르게 사용하면 API가
    
    문서 없이도 의도를 이해할 수 있는 “자기 설명적 인터페이스”가 된다.
    
    이제는 “동작하는 API”가 아닌 “이해하기 쉬운 API”를 목표로 해야 함을 배움.
    

### 🏷️ 키워드 (태그):

`#REST` `#API디자인` `#HTTP메소드` `#상태코드` `#URI설계` `#MIMEType` `#자기서술적메시지` 

`#API베스트프랙티스`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-10-22 | REST API 설계 실수 | 잘못된 메소드/상태코드/URI/MIME Type 사용 패턴 분석 | HTTP 요청/응답 설계 교정 실습 | API는 “행동+자원+표현”의 조화로 설계해야 함을 학습 | `#REST` `#HTTP` `#API디자인` | ✅ |
