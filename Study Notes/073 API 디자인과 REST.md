# API 디자인과 REST

### 📅 날짜:

> 2025.10.16 (목)
> 

### 📘 오늘 공부한 주제:

> REST의 등장 배경과 핵심 개념, REST 제약 조건 6가지 구조 이해
> 

---

## 📝 핵심 개념 요약

- **웹 API 디자인**은 “서버와 클라이언트 간의 약속”을 설계하는 것
    
    → 요청 구조를 단순하고 직관적으로 만드는 것이 핵심
    
- 초기 웹 서비스의 문제점
    - API 설계 방식이 제각각
    - 복잡하고 비효율적
    - 확장성 부족
- **REST (Representational State Transfer)**
    - 로이 필딩 박사가 제안한 **웹 아키텍처 스타일**
    - HTTP의 원리를 최대한 활용하여 **간결하고 확장성 있는 API 설계** 목표
- REST 원칙을 완벽히 지키기 어려우므로, 대부분의 서비스는 **RESTful API** 형태로 구현
- REST의 6가지 제약 조건 중 **Uniform Interface**는 반드시 지켜야 하는 핵심

## 📊 핵심 요약 표

| 구분 | 개념 | 비유 | 핵심 포인트 |
| --- | --- | --- | --- |
| **Client-Server** | 요청과 응답의 역할 분리 | 손님(클라이언트) ↔ 주방(서버) | 관심사 분리, 독립적 확장 |
| **Stateless** | 요청은 독립적 | 택시 기사에게 매번 목적지 말하기 | 서버는 상태 저장 X |
| **Cache** | 응답 재사용 | 패스트푸드 조리음식 | 성능 향상, 서버 부하 감소 |
| **Uniform Interface** | 일관된 인터페이스 | 스마트폰 충전 단자 | URI, METHOD, 메시지 규격화 |
| **Layered System** | 계층 구조 설계 | 택배 허브 시스템 | 보안, 로드밸런싱 가능 |
| **Code on Demand** | 실행 코드 전송 (선택적) | 스마트 TV 앱 설치 | JavaScript 실행 가능 |

### 💻 실습 내용 정리

REST 제약 조건을 **HTTP 프로토콜**에서 자연스럽게 충족하는 방식 이해

```jsx
// 예시 1️⃣ : Stateless 요청
fetch("/api/users", {
  method: "GET",
  headers: { Authorization: `Bearer ${accessToken}` }, // 매 요청에 인증 포함
});

// 예시 2️⃣ : Cache 제어
app.get("/data", (req, res) => {
  res.set("Cache-Control", "public, max-age=3600");
  res.json({ message: "데이터 캐싱 예시" });
});

// 예시 3️⃣ : Layered System (중간 계층 프록시 설정 가능)
app.use("/api", proxyMiddleware("https://backend-server.com"));
```

**Uniform Interface의 4가지 하위 제약 조건**

| 하위 조건 | 설명 | 예시 |
| --- | --- | --- |
| 자원 식별 (Identification of Resources) | URI로 자원 식별 | `/users/123` |
| 표현을 통한 자원 조작 (Manipulation through Representations) | HTTP Method 사용 | GET, POST, PATCH, DELETE |
| 자기 서술적 메시지 (Self-descriptive Messages) | 요청/응답이 자체적으로 의미 포함 | `Content-Type: application/json` |
| HATEOAS (Hypermedia as the Engine of Application State) | 하이퍼링크로 상태 전이 | 응답 안에 관련 링크 포함 |

### ❗ 헷갈렸던 점 / 문제 해결:

- **헷갈렸던 점:** REST와 RESTful의 차이점이 모호함
- **문제 해결:** REST는 아키텍처 원칙이고, RESTful은 그 원칙을 **얼마나 잘 지켰는가의 정도**를 나타내는 표현임을 명확히 이해
    
    (즉, “REST 원칙을 충실히 따른 API = RESTful API”)
    

### 💡 느낀 점 / 배운 점:

- REST는 단순한 API 설계 방식이 아니라, **웹 전체의 철학을 반영한 아키텍처 스타일**임을 배움.
- HTTP 프로토콜 자체가 이미 REST의 제약 조건을 자연스럽게 구현하고 있음을 이해함.
- 특히 “Uniform Interface”가 핵심이라는 점을 통해 API를 설계할 때 **일관성과 표준화의 중요성**을 실감.

### 🏷️ 키워드 (태그):

`#REST` `#RESTfulAPI` `#HTTP` `#API디자인` `#UniformInterface` `#Stateless` `#Cache` `#LayeredSystem` `#CodeOnDemand` `#ClientServer` `#HATEOAS`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-10-16 | API 디자인과 REST | REST는 HTTP 기반의 아키텍처 스타일로, 단순하고 확장 가능한 API 설계를 목표 | HTTP 요청-응답, 캐시, 계층 구조 실습 | REST vs RESTful 차이 명확히 이해 | `#REST` `#HTTP``#API디자인` `#RESTful` `#UniformInterface` | ✅ |
