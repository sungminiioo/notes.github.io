# Dynamic Routing 정리

### 📅 날짜:

> 2025.08.26 (화)
> 

### 📘 오늘 공부한 주제:

> Dynamic Routing (동적 라우팅)
> 

---

## 📝 핵심 개념 요약

- **Dynamic Routing**: 경로의 특정 부분을 동적으로 처리 (`/breeds/:id`, `/detail/[id]`)
- **폴더명 대괄호 규칙**: `app/detail/[id]/page.tsx` → `id` 값에 따라 다른 페이지 제공
- **params**: 서버 컴포넌트에서 props로 전달되는 값
- **useParams**: 클라이언트 컴포넌트에서 동적 라우트 값 접근 가능
- **searchParams**: 같은 페이지 내 상태(필터/정렬/페이지네이션/검색)를 URL 쿼리로 관리
- **장점**: URL 공유 가능, 새로고침 시 상태 유지, 북마크/뒤로가기 대응

## 📊 핵심 요약 표

| 구분 | 설명 |
| --- | --- |
| **Dynamic Routing** | `/:id` 형태로 동적 경로 처리 |
| **폴더명 규칙** | `[id]` 폴더명으로 동적 라우트 구현 |
| **params (서버)** | 서버 컴포넌트에서 props로 전달되는 동적 값 |
| **useParams (클라이언트)** | 클라이언트 컴포넌트에서 경로 값 접근 |
| **searchParams** | URL 쿼리 기반 상태 관리 (필터링, 검색, 페이지네이션 등) |

### 💻 실습 내용 정리

### 1) React Router 기반 동적 라우트 예시

```jsx
<BrowserRouter>
  <Routes>
    <Route path="/breeds/:id" element={<Detail />} />
  </Routes>
</BrowserRouter>
```

### 2) Next.js 서버 컴포넌트에서 `params` 받기

```jsx
// app/detail/[id]/page.tsx
const DetailPage = async ({ params }) => {
  const { id } = await params;
  return <div>Detail 페이지 : {id}</div>;
};
export default DetailPage;
```

### 3) Next.js 클라이언트 컴포넌트에서 `useParams` 사용

```jsx
"use client"
import { useParams } from 'next/navigation';

const DetailPage = () => {
  const { id } = useParams();
  return <div>Detail 페이지 : {id}</div>;
};
export default DetailPage;
```

### 4) searchParams 사용 (검색, 필터링, 페이지네이션)

```jsx
'use client'
import { useSearchParams } from 'next/navigation'

export default function SearchBar() {
  const searchParams = useSearchParams()
  const search = searchParams.get('search')
  return <>Search: {search}</>
}
```

✅ 활용 예시

- **상품 리스트 필터링**: `/products?category=shoes&color=black&sort=price_asc`
- **게시판 페이지네이션**: `/board?page=3&limit=20`
- **검색 결과 유지**: `/search?query=react`

### ❗ 헷갈렸던 점 / 문제 해결:

- React Router의 `:id` 방식과 Next.js의 `[id]` 방식이 비슷하지만, Next.js는 폴더 구조에 따라 자동 매핑되는 점이 새로웠음.
- `params`와 `searchParams`의 차이를 정리하면서 →
    
    👉 `params`는 **경로값**, `searchParams`는 **쿼리스트링 상태값**이라는 걸 확실히 이해.
    

### 💡 느낀 점 / 배운 점:

- Next.js는 폴더 기반으로 라우팅을 구성하기 때문에, 개발자가 직접 라우터를 설정할 필요가 줄어듦.
- `searchParams`를 통해 상태를 URL에 반영하면, 공유성과 유지보수성이 크게 좋아진다는 걸 체감함.
- 단순한 라우팅을 넘어 “**URL로 상태를 관리한다**”는 패턴의 중요성을 배움.

### 🏷️ 키워드 (태그):

`#Next.js` `#DynamicRouting` `#params` `#useParams` `#searchParams` `#Routing` `#URLState`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-08-26 | Dynamic Routing | `[id]` 폴더명으로 동적 라우팅, params & searchParams 활용 | params 받기, useParams 사용, searchParams 실습 | params vs searchParams 차이 명확히 이해, URL 상태 관리 장점 체감 | `#Next.js` `#DynamicRouting` `#params` `#searchParams` `#URLState` | ✅ |
