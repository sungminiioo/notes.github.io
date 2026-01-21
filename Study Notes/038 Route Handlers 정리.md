# Route Handlers 정리

### 📅 날짜:

> 2025.08.28 (목)
> 

### 📘 오늘 공부한 주제:

> GET / POST / Dynamic Route / searchParams 처리 방법
> 

---

## 📝 핵심 개념 요약

- `app/폴더/route.js`를 만들면 API 엔드포인트 자동 생성됨.
- 서버에서만 실행 → 민감 정보 보호 가능.
- 클라이언트에서 직접 API 요청 시 발생하는 **보안 문제, CORS 문제 해결** 가능.
- GET, POST 등 HTTP 메서드를 직접 정의해서 백엔드 역할 수행 가능.

## 📊 핵심 요약 표

| 구분 | 특징 | 사용 예시 |
| --- | --- | --- |
| **GET** | 데이터 조회용 | DB 조회, API 응답 |
| **POST** | 데이터 생성/등록 | Form 제출, 새 리소스 생성 |
| **Dynamic Route** | `params` 처리 | `/items/[id]` 형태 |
| **searchParams** | URL 쿼리 처리 | `/search?query=react` |
| **활용 포인트** | - 민감정보 보호  - CORS 우회  - REST API 제공 | 클라이언트-서버 중간 API 레이어 |

### 💻 실습 내용 정리

### 1) 기본 GET 핸들러

```jsx
export async function GET() {
  const res = await fetch('https://data.mongodb-api.com/...', {
    headers: {
      'Content-Type': 'application/json',
      'API-Key': process.env.DATA_API_KEY,
    },
  })
  const data = await res.json()

  return Response.json({ data })
}
```

### 2) Dynamic Route (params 처리)

```jsx
// app/items/[id]/route.js
export async function GET(request, { params }) {
  const { id } = await params
  return Response.json({ itemId: id })
}
```

### 3) searchParams 처리

```jsx
import { type NextRequest } from 'next/server'

export function GET(request: NextRequest) {
  const searchParams = request.nextUrl.searchParams
  const query = searchParams.get('query')
  return Response.json({ query })
}
```

### 4) POST 요청 처리

```jsx
export async function POST(request) {
  const body = await request.json()
  await fetch(`${API_URL}/todo`, {
    method: "POST",
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(body)
  })
  return Response.json({ body })
}
```

### ❗ 헷갈렸던 점 / 문제 해결:

- Route Handlers가 **서버 전용**이라는 점을 놓치면, 클라이언트 코드처럼 작성해서 오류 발생.
- `params`와 `searchParams` 차이를 명확히 구분 →
    - `params`: URL 경로 변수 (`/items/[id]`)
    - `searchParams`: URL 쿼리 변수 (`/items?id=123`)

### 💡 느낀 점 / 배운 점:

- Route Handlers는 Next.js가 단순 프론트엔드 프레임워크가 아니라 **풀스택 프레임워크**임을 보여주는 기능.
- API 서버를 따로 만들지 않아도 프론트와 함께 백엔드 레이어를 쉽게 구축 가능.
- 보안적으로도 API Key, Token을 서버 측에 숨길 수 있어 안심.

### 🏷️ 키워드 (태그):

`#Next.js` `#RouteHandlers` `#API` `#GET` `#POST` `#DynamicRoutes` `#searchParams` `#CORS우회` 
`#정보보안`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-08-28 | Route Handlers | `app/폴더/route.js`로 API 엔드포인트 생성, 서버 전용 실행, GET/POST 지 | GET, POST, params, searchParams 예시 실습 | 서버 전용 실행 개념 헷갈렸으나 params vs searchParams 차이 명확히 이해, Next.js의 풀스택 프레임워크 강점 체감 | `#Next.js` `#RouteHandlers` `#API` `#GET` `#POST` `#DynamicRoutes` | ✅ |
