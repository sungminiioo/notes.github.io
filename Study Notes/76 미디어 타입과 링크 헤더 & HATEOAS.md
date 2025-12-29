# 미디어 타입과 링크 헤더 & HATEOAS

### 📅 날짜:

> 2025.10.21 (화)
> 

### 📘 오늘 공부한 주제:

> REST의 자기 서술적 메시지(Self-descriptive Message)와 HATEOAS 제약 조건
> 

---

## 📝 핵심 개념 요약

- REST의 **Uniform Interface(일관된 인터페이스)** 제약 조건 중
    
    세 번째와 네 번째 조건이 **“자기 서술적 메시지”**와 **“HATEOAS”**
    
- 모든 요청과 응답은 결국 **문자 메시지(text)** 이며,
    
    **해당 메시지를 어떻게 해석할지 알려주는 정보(Content-Type, Link 등)** 가 함께 포함되어야 함.
    
- **자기 서술적 메시지(Self-descriptive message)**
    
    → “이 메시지가 어떤 의미인지 메시지 자체를 통해 이해할 수 있어야 함.”
    
- **HATEOAS**
    
    → “응답 안에 다음 가능한 행동(링크)을 포함하여, 클라이언트가 애플리케이션의 흐름을 알 수 있게 함.”
    

## 📊 핵심 요약 표

| 개념 | 의미 | 예시 | 특징 |
| --- | --- | --- | --- |
| **Content-Type** | 메시지의 데이터 형식(Media Type) 지정 | `Content-Type: application/json` | 서버와 클라이언트가 같은 데이터 구조로 해석 가능 |
| **Media Type (MIME Type)** | 전송 데이터의 형식 표준 | `text/html`, `application/json`, `image/png` 등 | 원래 이메일 첨부파일 전송을 위한 표준에서 시작 |
| **Link 헤더** | API 문서나 관련 리소스의 의미 제공 | `Link: <https://api.example.com/profile>; rel="profile"` | 메시지의 구조와 의미를 연결 |
| **HATEOAS** | 응답에 다음 동작을 안내하는 하이퍼미디어 포함 | `_links: { "next": { "href": "/page/2" } }` | 애플리케이션 상태를 링크 기반으로 이동 |

### 💻 실습 내용 정리

### 📜 예시 1: Media Type 지정

```
Content-Type: application/json

{
  "id": 1,
  "title": "REST API Study"
}
```

→ 클라이언트는 메시지가 JSON 구조임을 알고, 파싱 기준을 자동으로 결정.

---

### 📜 예시 2: Link 헤더 추가

```
Link: <https://api.example.com/docs/member-profile>; rel="profile"
```

→ 응답 메시지 안에서 데이터 구조와 의미를 이해할 수 있는 문서 위치를 알려줌.

---

### 📜 예시 3: HATEOAS 적용 (HAL 예시)

```json
{
  "_links": {
    "self": { "href": "/members/1" },
    "update": { "href": "/members/1/edit" },
    "delete": { "href": "/members/1" }
  },
  "id": 1,
  "username": "harry"
}
```

→ 현재 상태(`self`)와 가능한 다음 행동(`update`, `delete`)을 함께 제공

→ 클라이언트가 별도의 문서 없이도 가능한 액션을 이해 가능

### ❗ 헷갈렸던 점 / 문제 해결:

- **헷갈렸던 점:**
    
    `application/json`만 명시하면 충분하다고 생각했는데,
    
    실제로는 데이터의 **구조와 의미**를 전달하지 못해 완전한 자기서술성을 만족하지 않음을 깨달음.
    
- **문제 해결:**
    
    `Link` 헤더나 HAL(JSON 구조 내 링크 제공)을 통해 메시지 의미를 명시하는 방식 이해함.
    

### 💡 느낀 점 / 배운 점:

- REST는 단순히 자원 요청이 아니라, **데이터와 의미의 명세**까지 포함하는 철학임을 느낌.
- HATEOAS는 처음엔 복잡해 보이지만, 클라이언트 입장에서는 “API 탐색 가능성”을 높여주는 개념이라는 점이 흥미로웠음.
- 실제 프로젝트에서는 완전한 자기서술성보다 **사람이 이해하기 쉬운 문서화 중심 접근**이 실용적임을 깨달음.

### 🏷️ 키워드 (태그):

`#REST` `#MediaType` `#MIME` `#ContentType` `#LinkHeader` `#HATEOAS` `#UniformInterface` `#SelfDescriptiveMessage` `#HAL` `#API문서화`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-10-21 | 미디어 타입과 링크 헤더 & HATEOAS | REST의 자기 서술적 메시지와 링크 기반 상태 전이 개념 이해 | Content-Type, Link 헤더, HAL 예시 실습 | JSON만으로는 자기서술성 부족 → Link 헤더로 해결 | `#MediaType` `#LinkHeader` `#HATEOAS` | ✅ |
