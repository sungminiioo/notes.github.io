# React Query의캐싱 전략 

### 📅 날짜:

> 2025.09.22 (월)
> 

### 📘 오늘 공부한 주제:

> React Query 캐싱 전략(`stale-while-revalidate`), 캐시 데이터 보관 방식, Query Lifecycle
> 

---

## 📝 핵심 개념 요약

- **stale-while-revalidate(SWR) 전략**
- 신규 데이터 도착 전까지 기존 캐시 데이터를 우선 사용
- 캐시 응답을 즉시 보여주고, 동시에 서버에 새 요청을 보내 최신 데이터로 교체
- 브라우저 Cache-Control 헤더(`max-age`, `stale-while-revalidate`)에서 유래
- **캐시 데이터 저장 위치**
    - `QueryClientProvider` 내부의 **React Context API** 기반
    - 모든 하위 컴포넌트에서 캐시 데이터 접근 가능
    - 전역 상태처럼 동작
- **React Query Lifecycle**
    - **fresh**: 새 데이터가 필요 없음
    - **stale**: 새 데이터 필요 (refetch 발생)
    - **inactive**: 언마운트된 쿼리
    - **garbage collected (gc)**: 캐시 삭제
- **기본 설정(default config)**
    
    
    | 옵션 | 기본값 | 의미 |
    | --- | --- | --- |
    | `staleTime` | `0` | 데이터는 항상 stale 취급 |
    | `refetchOnMount` | `true` | 컴포넌트 마운트 시 자동 refetch |
    | `refetchOnWindowFocus` | `true` | 브라우저 focus 시 자동 refetch |
    | `refetchOnReconnect` | `true` | 네트워크 재연결 시 자동 refetch |
    | `gcTime` | `5분` | inactive 쿼리 캐시 삭제 시점 |
    | `retry` | `3` | 실패 시 3번까지 재시도 |
- **staleTime vs gcTime**
    - `staleTime`: 데이터를 stale로 취급할 시간
    - `gcTime`: inactive 상태 캐시를 보관할 시간

## 📊 핵심 요약 표

| 개념 | 설명 | 예시 |
| --- | --- | --- |
| SWR 전략 | 캐시 먼저, 새 데이터 나중에 반영 | Cache-Control: max-age=1, stale-while-revalidate=59 |
| 캐시 저장 | QueryClientProvider 내부(Context API 기반) | 전역적으로 공유 |
| fresh | 새 데이터 필요 없음 | staleTime > 0 동안 유지 |
| stale | 새 데이터 필요 | staleTime = 0 일 때 |
| inactive | 언마운트된 쿼리 | 5분 동안 유지 후 삭제 |
| gc | inactive 후 캐시 삭제 | gcTime 경과 시 제거 |

### 💻 실습 내용 정리

### 1. QueryClient 설정

```jsx
// App.jsx
import { QueryClient, QueryClientProvider } from "@tanstack/react-query";

const queryClient = new QueryClient();

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <Router />
    </QueryClientProvider>
  );
}
```

### 2. 캐시 라이프사이클 개념

```
fresh (staleTime 유지)
   ↓
stale (refetch 필요)
   ↓
inactive (컴포넌트 언마운트)
   ↓
gc (gcTime 지나면 삭제)
```

### 3. queryClient vs useQueryClient

```jsx
const queryClient = new QueryClient();      // 전역에서 최초 생성
const queryClient = useQueryClient();       // 컴포넌트 내부에서 사용
```

### ❗ 헷갈렸던 점 / 문제 해결:

- `staleTime`과 `gcTime`을 혼동 → `staleTime`은 fresh/stale 판정, `gcTime`은 캐시 보관 기간
- `new QueryClient()` vs `useQueryClient()` 차이를 구분해야 함 → 전역 생성 vs 훅 내부 사용

### 💡 느낀 점 / 배운 점:

- React Query는 단순 fetch 라이브러리가 아니라 **캐싱 전략 기반 상태 관리 툴**
- SWR 전략과 기본 config를 이해하면 불필요한 서버 호출을 줄이고 UX 개선 가능
- “fresh / stale / inactive / gc” 단계가 **데이터 생명주기 관리의 핵심**

### 🏷️ 키워드 (태그):

`#TanStack Query` `#React Query` `#SWR` `#staleTime` `#gcTime` `#캐싱 전략` `#QueryClient` `#전역상태`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-09-22 | React Query 캐싱 전략 | SWR 전략, fresh/stale/inactive/gc 개념, staleTime vs gcTime, 기본 설정 이해 | QueryClientProvider 설정, Lifecycle 다이어그램, queryClient vs useQueryClient 구분 | staleTime & gcTime 혼동 해결, QueryClient 생성 시점 구분 학습 | `#TanStack Query` `#React Query` `#SWR` `#staleTime` `#gcTime` 
`#캐싱 전략` | ✅ |
