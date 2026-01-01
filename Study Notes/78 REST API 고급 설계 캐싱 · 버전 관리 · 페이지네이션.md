# REST API 고급 설계 캐싱 · 버전 관리 · 페이지네이션

### 📅 날짜:

> 2025.10.23 (목)
> 

### 📘 오늘 공부한 주제:

> REST API의 효율적 데이터 전송과 유지보수를 위한 설계 기법
> 

---

## 📝 핵심 개념 요약

- REST의 6가지 제약 중 하나인 **Cacheable**은 “자원이 자주 변하지 않는다면 클라이언트가 재사용할 수 있도록 허용하라”는 원칙입니다.
또한, 서비스 변경 시 **API 버전 관리**를 통해 하위 호환성을 유지하고,
대용량 데이터를 다룰 때는 **페이지네이션(Pagination)** 으로 효율적인 데이터 전송을 구현합니다.

## 📊 핵심 요약 표

| 구분 | 개념 | 목적 | 실무 적용 포인트 |
| --- | --- | --- | --- |
| **Cache-Control** | 브라우저 캐시 만료시간 지정 (`max-age`) | 서버 요청 최소화 | 정적 파일: 1일, 이미지: 60초 |
| **Last-Modified** | 마지막 수정 일시 비교 | 변경 없을 시 캐시 활용 | 자동으로 `If-Modified-Since`와 비교됨 |
| **ETag** | 자원 식별자(해시 기반) | 자원 변경 여부 정확히 비교 | `ETag`가 다르면 재다운로드 |
| **API 버전 명시** | `/v1/members`, `/v2/members` | 클라이언트 혼란 방지 | URI, Query, Header 방식 존재 |
| **Pagination** | 데이터 분할 전송 | UX 개선 & 서버 부하 감소 | page, limit, offset 파라미터 사용 |

### 💻 실습 내용 정리

### ✅ Express에서 캐싱 설정 예시

```jsx
app.use(express.static('public', {
  setHeaders: (res, path) => {
    if (path.endsWith('.jpg') || path.endsWith('.png')) {
      res.set('Cache-Control', 'public, max-age=60'); // 60초
    } else if (path.endsWith('.css') || path.endsWith('.js')) {
      res.set('Cache-Control', 'public, max-age=86400'); // 1일
    } else {
      res.set('Cache-Control', 'no-store'); // 캐시 비활성화
    }
  }
}));
```

### ✅ ETag 및 Last-Modified 설정

```jsx
res.set({
  'Cache-Control': 'public, max-age=120',
  'ETag': `"${etag}"`,
  'Last-Modified': stats.mtime.toUTCString()
});
```

### ✅ API 버전 관리 예시

```
GET /v1/members    → 기존 응답 구조
GET /v2/members    → 변경된 구조
```

- 큰 변경 시: **새로운 Endpoint 생성**
- 작은 변경 시: **속성 추가 → 마이그레이션 완료 후 제거**

### ✅ 페이지네이션 예시

```
GET /members?page=2&limit=10
```

- `page`: 현재 페이지 번호
- `limit`: 한 번에 가져올 데이터 개수
- `totalPages`, `totalItems` 응답 포함 시 UX 향상

### ❗ 헷갈렸던 점 / 문제 해결:

**문제:**

- 브라우저 캐시가 갱신되지 않아 구 버전 리소스가 로드됨
- API 수정 시 클라이언트 혼란 발생
- 대량 데이터 조회 시 서버 과부하

**해결:**

- `ETag`, `Last-Modified`, `Cache-Control` 조합으로 캐싱 전략 확립
- 버전별 Endpoint 분리 및 점진적 마이그레이션 적용
- 페이지네이션 도입으로 요청 단위 최소화

### 💡 느낀 점 / 배운 점:

- REST 설계의 핵심은 단순히 데이터를 “보내는 것”이 아니라 “의미 있게 유지·관리하는 것”이라는 점을 체감했다.
- 캐싱은 성능 최적화의 기본이며, 버전 명시는 서비스 안정성의 기본이다.
- 페이지네이션은 사용자 경험(UX)과 서버 효율성의 균형을 맞추는 좋은 도구임을 배움.

### 🏷️ 키워드 (태그):

`#REST` `#캐싱` `#ETag` `#LastModified` `#API버전관리` `#Pagination` `#CacheControl` `#Express` 

`#웹성능최적화`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-10-23 | 캐싱 · 버전관리 · 페이지네이션 | REST 성능 및 유지보수성 향상을 위한 3대 설계 전략 | Express 캐시 설정 / API 버전 구분 / 페이지네이션 요청 구현 | 캐시·버전·페이지 구조를 통해 확장 가능한 API 설계 방법을 체득 | `#REST` `#Cache` `#Versioning` `#Pagination` | ✅ |
