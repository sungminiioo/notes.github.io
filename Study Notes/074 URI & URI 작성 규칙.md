# URI & URI 작성 규칙

### 📅 날짜:

> 2025.10.17 (금)
> 

### 📘 오늘 공부한 주제:

> URI의 개념과 RESTful API에서의 올바른 URI 설계 원칙
> 

---

## 📝 핵심 개념 요약

- **URI (Uniform Resource Identifier)**
    
    → 인터넷 상의 특정 자원을 식별하기 위한 **고유한 주소 체계**
    
    → “무엇을(자원)” + “어디서(위치)” 서버가 알아볼 수 있도록 명시
    
- REST에서의 URI는 **서버의 자원(Resource)을 명확히 표현**하는 역할을 하며,
    
    잘 설계된 URI는 **사람이 읽기에도 직관적**이어야 함.
    
- **핵심 포인트:**
    - URI는 **자원의 이름(명사)**만 표현
    - 행위(동사)는 **HTTP Method(GET, POST, PUT, DELETE)**로 표현
    - 확장자(.html, .json 등) 제거, 일관된 규칙 유지

## 📊 핵심 요약 표

| 원칙 | 설명 | 예시 (좋은 / 나쁜 예) |
| --- | --- | --- |
| **자원은 명사로 표현** | 행위 대신 대상 중심으로 표현 | ✅ `/members`❌ `/get-member-list` |
| **단수/복수 구분** | 컬렉션과 개별 자원 구분 | `/members` / `/members/1` |
| **계층 구조는 슬래시(`/`)** | 자원 간 관계 표현, 마지막 `/` 금지 | ✅ `/articles/1/comments/2` |
| **소문자 + 대시(`-`) 사용** | kebab-case 규칙, 가독성 향상 | ✅ `/top-voted-articles` |
| **확장자 제외** | 표현 형식은 `Accept` 헤더로 결정 | ✅ `/articles`❌ `/articles.json` |
| **필터링은 쿼리로 처리** | 정렬·검색 등은 쿼리 스트링으로 | `/articles?sort=latest` |
| **HTTP 메서드로 동작 구분** | URI에 동사 대신 메서드 사용 | ✅ `DELETE /members/1`❌ `/members/1/delete` |

### 💻 실습 내용 정리

### 🎬 영화관 예시

- 자원: 좌석(seat)
- 특정 좌석 식별: `H열 11번`
- URI 변환 → `/seats/h/11`

```
GET /seats/h/11
→ 서버는 H열 11번 좌석 정보를 반환
```

### 🌐 블로그 예시

```
GET https://example.com/blogs/1/comments
→ ID가 1인 블로그 글의 모든 댓글 조회
```

### ✏️ 실습 코드 예시

```jsx
// 서버 예시
app.get("/members", (req, res) => {
  res.json({ message: "모든 회원 목록 반환" });
});

app.get("/members/:id", (req, res) => {
  res.json({ message: `${req.params.id}번 회원 조회` });
});

// ❌ 잘못된 예시
// app.get("/get-member-list", ...)
// app.post("/members/create-new-member", ...)
```

### ❗ 헷갈렸던 점 / 문제 해결:

- **헷갈렸던 점:**
    
    URI에 ‘행위(동사)’를 넣어야 할지 헷갈림 (예: `/create-member`)
    
- **문제 해결:**
    
    행위는 URI가 아니라 **HTTP Method로 표현**해야 함을 명확히 이해함.
    
    URI는 오직 “무엇(자원)”만 나타내야 RESTful한 구조가 됨.
    

### 💡 느낀 점 / 배운 점:

- URI 설계는 단순한 주소 작성이 아니라 **API의 명확성과 일관성을 결정하는 핵심 요소**임을 깨달음.
- 작은 차이(대문자, 확장자, 동사 사용 등)도 유지보수성에 큰 영향을 준다는 점을 체감함.
- “사람이 봐도 읽히는 URI”가 좋은 API의 첫걸음임을 배움.

### 🏷️ 키워드 (태그):

`#URI` `#RESTful` `#API디자인` `#Resource` `#HTTPMethod` `#명사형URI` `#KebabCase` `#QueryString` `#REST원칙`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-10-17 | URI & URI 작성 규칙 | URI는 자원을 식별하는 고유 주소로, 명사형 구조와 일관된 규칙이 핵심 | `/members`, `/articles?sort=latest` 등 RESTful URI 설계 실습 | URI에 동사 넣는 오류 → HTTP Method로 동작 표현 | `#URI` `#RESTful` `#API디자인` `#명사형URI` `#KebabCase` | ✅ |
