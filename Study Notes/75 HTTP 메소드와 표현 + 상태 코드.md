# HTTP 메소드와 표현 + 상태 코드

### 📅 날짜:

> 2025.10.20 (월)
> 

### 📘 오늘 공부한 주제:

> REST에서의 HTTP 메소드의 의미와 상태 코드의 역할
> 

---

## 📝 핵심 개념 요약

- **REST의 핵심 제약 중 하나: "표현을 통한 자원 조작"**
    
    → 자원을 직접 변경하지 않고, **표현(Representation)**을 통해 **어떻게 조작할지**를 명시
    
- HTTP 메소드는 클라이언트가 자원에 대해 수행하려는 **행위(조작)**를 표현
    - 자원 조작의 표현 → **HTTP 메소드**
    - 자원의 표현 → **JSON 데이터**
- 자원 상태의 결과는 **HTTP 상태 코드**로 전달되어
    
    요청의 처리 결과를 명확히 알 수 있음.
    

## 📊 핵심 요약 표

| 메소드 | 역할 | 특징 | 예시 |
| --- | --- | --- | --- |
| **GET** | 자원 조회 | 서버의 상태 변경 ❌ | `GET /members` |
| **POST** | 자원 생성 / 상태 변경 | 새 데이터 생성, 서버 측 동작 | `POST /members` |
| **PUT** | 자원 전체 수정(교체) | 기존 자원을 완전히 대체 | `PUT /members/1` |
| **PATCH** | 자원 일부 수정 | 특정 필드만 변경 | `PATCH /members/1` |
| **DELETE** | 자원 삭제 | 자원 제거 요청 | `DELETE /members/1` |

> 🔸 PUT vs PATCH
> 
> - **PUT:** 자원 전체 교체
> - **PATCH:** 일부 수정

---

### 📊 **상태 코드 핵심 요약 표**

| 범위 | 의미 | 대표 코드 | 설명 |
| --- | --- | --- | --- |
| **1xx** | 정보 알림 | 100 Continue | 요청 계속 진행 가능 |
| **2xx** | 성공 | 200 OK / 201 Created / 204 No Content | 요청이 성공적으로 처리됨 |
| **3xx** | 리다이렉션 | 301 Moved Permanently / 302 Found / 304 Not Modified | 다른 위치로 이동 또는 캐시 재사용 |
| **4xx** | 클라이언트 오류 | 400 Bad Request / 401 Unauthorized / 403 Forbidden / 404 Not Found | 잘못된 요청 또는 권한 문제 |
| **5xx** | 서버 오류 | 500 Internal Server Error / 502 Bad Gateway / 503 Service Unavailable | 서버 내부 문제나 응답 불가 상태 |

### 💻 실습 내용 정리

### 📜 예시 1: GET — 자원 조회

```
GET /members
→ 모든 회원 정보 조회 (자원 상태 변경 없음)
```

### 🧾 예시 2: POST — 자원 생성

```
POST /members
{
  "username": "harry",
  "age": 30
}
→ 새로운 회원 생성 (201 Created)
```

### 🔁 예시 3: PUT vs PATCH

```
PUT /members/1
{
  "username": "mike"
}
→ {"username": "mike", "age": null}  // 전체 교체
```

```
PATCH /members/1
{
  "username": "mike"
}
→ {"username": "mike", "age": 30}    // 일부 수정
```

### ❌ 예시 4: DELETE — 자원 삭제

```
DELETE /members/1
→ 해당 회원 정보 삭제 (204 No Content)
```

### ❗ 헷갈렸던 점 / 문제 해결:

- **헷갈렸던 점:**
    
    `PUT`과 `PATCH`의 차이가 모호했음 — 둘 다 수정으로 보였기 때문.
    
- **문제 해결:**
    
    PUT은 “전체 교체”, PATCH는 “부분 수정”이라는 개념으로 명확히 구분.
    
    또한, 상태 코드와 함께 응답 형태를 보는 습관을 들임.
    

### 💡 느낀 점 / 배운 점:

- REST의 진짜 핵심은 **“명확한 의도를 가진 통신”**이라는 점을 이해함.
- HTTP 메소드만 잘 써도 API의 가독성과 유지보수성이 크게 향상됨.
- 상태 코드는 단순 숫자가 아니라, “API의 대화 언어”라는 점이 인상 깊었음.

### 🏷️ 키워드 (태그):

`#HTTPMethod` `#REST` `#GET` `#POST` `#PUT` `#PATCH` `#DELETE` `#StatusCode` `#RESTfulAPI` 

`#표현을통한자원조작`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-10-20 | HTTP 메소드와 표현 & 상태 코드 | HTTP 메소드로 자원 조작 의도 표현, 상태 코드로 결과 전달 | GET/POST/PUT/PATCH/DELETE 실습 및 응답 코드 확인 | PUT과 PATCH의 차이 명확히 구분 | `#REST` `#HTTPMethod` `#StatusCode` | ✅ |
