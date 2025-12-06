useQuery 기본 사용법

### 📅 날짜:

> 2025.09.18 (목)
> 

### 📘 오늘 공부한 주제:

> `useQuery` 기본 사용법과 Next.js(App Router) 환경에서의 Provider 설정
> 

---

## 📝 핵심 개념 요약

- `useQuery`: 서버로부터 **데이터를 가져오기(Read)** 전용 훅
- CRUD 중 **R(Read)** 담당
- `queryKey`: **배열 필수** (데이터 식별자)
- `queryFn`: 실제 API 호출 함수 (Promise 반환해야 함)
- Next.js App Router에서는 **Root Layout**에서 `QueryClientProvider`로 래핑해야 함
- `Provider 주입 패턴`으로 프로젝트 전역에 TanStack Query 환경 구성

## 📊 핵심 요약 표

| 개념 | 설명 | 예시 |
| --- | --- | --- |
| useQuery | 서버에서 데이터 가져오기 | `useQuery({ queryKey, queryFn })` |
| queryKey | 쿼리 캐싱 식별자, 배열 필수 | `["todos"]` |
| queryFn | 실제 API 호출 함수 | `async () => fetch(...)` |
| Provider | 전역에서 Query 사용 가능하게 만듦 | `QueryClientProvider` |

### 💻 실습 내용 정리

### 1. `useQuery` 기본 예제

```jsx
import { useQuery } from "@tanstack/react-query";

const getTodos = async () => {
  // 실제라면 fetch(API_URL) 사용
  return [1, 2, 3];
};

export default function TodoList() {
  const { data, isPending, error } = useQuery({
    queryKey: ["todos"],  // ✅ 배열 필수
    queryFn: getTodos,
  });

  if (isPending) return <p>로딩중...</p>;
  if (error) return <p>에러: {error.message}</p>;

  return <ul>{data.map((todo) => <li key={todo}>{todo}</li>)}</ul>;
}
```

---

### 2. Next.js Provider 주입 패턴

👉 `App Router`에서는 **Root Layout**에서 `QueryClientProvider`로 전역 래핑해야 함

### `/src/app/providers.jsx`

```jsx
"use client";

import { QueryClient, QueryClientProvider } from "@tanstack/react-query";
import { ReactQueryDevtools } from "@tanstack/react-query-devtools";

export default function Providers({ children }) {
  const [queryClient] = useState(() => new QueryClient());

  return (
    <QueryClientProvider client={queryClient}>
      <ReactQueryDevtools initialIsOpen={false} />
      {children}
    </QueryClientProvider>
  );
}
```

### `/src/app/layout.jsx`

```jsx
import Providers from "./providers";

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body className="antialiased">
        <Providers>
          {children}
        </Providers>
      </body>
    </html>
  );
}
```

👉 이렇게 하면 프로젝트 전역에서 `useQuery`, `useMutation`을 자유롭게 사용 가능

### ❗ 헷갈렸던 점 / 문제 해결:

- `queryKey`를 문자열로 쓰면 에러 발생 → 반드시 배열이어야 함 (`["todos"]`)
- Next.js에서는 `pages/_app.js` 대신 `App Router` 기준으로 `RootLayout`에서 `Provider` 등록

### 💡 느낀 점 / 배운 점:

- 기존 `useState + useEffect` 방식보다 `useQuery`가 훨씬 간결하고 안전하다.
- Provider 패턴으로 한 번 세팅해두면 전역 어디서든 쓸 수 있어서 확장성 뛰어남.
- ReactQueryDevtools를 함께 쓰면 쿼리 상태를 시각적으로 확인할 수 있어서 디버깅이 편리하다.

### 🏷️ 키워드 (태그):

`#TanStack Query` `#useQuery` `#Next.js App Router` `#QueryClientProvider` `#ReactQueryDevtools` `#Provider 패턴`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-09-18 | useQuery & Provider 패턴 | `useQuery`로 데이터 Fetching, Next.js에서는 Provider 주입 필수 | `useQuery` 기본 예제 + RootLayout Provider 설정 | queryKey 배열 필수, Provider 설정 위치(App Router) 이해 | `#TanStack Query` `#useQuery` `#Provider` `#Next.js` `#ReactQueryDevtools` | ✅ |
