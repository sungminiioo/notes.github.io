# useQuery에서 자주 사용하는 must-know 옵션들

### 📅 날짜:

> 2025.09.23 (화)
> 

### 📘 오늘 공부한 주제:

> useQuery must-know 옵션 (`enabled`, `select`)
> 

---

## 📝 핵심 개념 요약

- `enabled`: 특정 조건이 충족될 때만 queryFn 실행 가능 (수동 실행, 의존 쿼리 구현 가능).
- `select`: queryFn의 결과를 변형해서 컴포넌트로 전달, 캐시 원본은 그대로 유지.
- `isPending`: 캐시가 없고 아직 쿼리 실행 대기 중일 때 true.
- `isFetching`: 쿼리 실행 중일 때 true (초기 + refetch 모두 포함).
- 상세 데이터 패턴: queryKey에 id를 포함하고, `QueryFunctionContext` 활용 가능.

## 📊 핵심 요약 표

| 개념 | 설명 | 예제/포인트 |
| --- | --- | --- |
| enabled | 쿼리 실행 여부 제어 (기본값 true) | - 버튼 클릭 시 refetch- userId 존재 시에만 실행 (dependent queries) |
| select | queryFn 리턴값 변형 | `select: (user) => user.username` |
| isPending | 데이터 없고 대기 중 | 초기 로딩 상태 관리 |
| isFetching | 실행 중 (refetch 포함) | UX 깜빡임 주의 |
| QueryFunctionContext | queryFn 실행 시 전달되는 객체 | `const { queryKey } = context` 로 id 접근 |

### 💻 실습 내용 정리

### 1 enabled 옵션 - 수동 실행

```jsx
const { data, refetch } = useQuery({
  queryKey: ["todos"],
  queryFn: getTodos,
  enabled: false
});

<button onClick={() => refetch()}>데이터 불러오기</button>
```

### 2 enabled 옵션 - 의존 쿼리

```jsx
const { data: user } = useQuery({
  queryKey: ["user", email],
  queryFn: getUserByEmail,
});

const userId = user?.id;

const { data: projects } = useQuery({
  queryKey: ["projects", userId],
  queryFn: getProjectsByUser,
  enabled: !!userId
});
```

### 3 select 옵션 사용

```jsx
const { data: username } = useQuery({
  queryKey: ["user"],
  queryFn: fetchUser,
  select: (user) => user.username,
});
```

### 4 TanStack Query 적용 전 → 후 비교

- 적용 전: useState + useEffect + try/catch 로 직접 관리
- 적용 후: useQuery 로 단순화 (isPending, error, data 관리 자동화)

### 5 상세 데이터 패턴 (QueryFunctionContext)

```jsx
const { data } = useQuery({
  queryKey: ["todos", id],
  queryFn: async ({ queryKey }) => {
    const [, id] = queryKey;
    const response = await todoApi(`/todos/${id}`);
    return response.data;
  },
});
```

### ❗ 헷갈렸던 점 / 문제 해결:

- `isPending`과 `isFetching` 차이:
- isPending = **초기 로딩 대기 상태** (데이터 없을 때만 true)
- isFetching = **실행 중** (초기 로딩 + refetch 포함)
    
    👉 UX 고려 시 기본은 `isPending`, 특정 상황에만 `isFetching` 활용
    

### 💡 느낀 점 / 배운 점:

- TanStack Query가 없던 시절의 로딩/에러/데이터 관리 코드를 직접 짰다면 훨씬 복잡했을 것.
- `enabled` 옵션으로 실행 시점 제어가 가능한 점이 특히 강력함.
- 앞으로 상세 데이터 로직(id 기반 fetch)에서는 `queryKey`와 `QueryFunctionContext` 패턴을 적극 활용할 수 있겠다.

### 🏷️ 키워드 (태그):

`#tanstack-query` `#useQuery` `#enabled` `#select` `#isPending` `#isFetching` `#queryKey` 
`#react-query`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-09-23 | useQuery 옵션 심화 | enabled: 실행 제어 / select: 데이터 가공 / isPending vs isFetching | enabled, select, dependent query, QueryFunctionContext 예제 코드 | isPending vs isFetching 차이 명확히 이해 / enabled로 실행 시점 제어 가능 | `#tanstack-query` `#useQuery` `#enabled` `#select` `#isPending` `#isFetching` | ✅ |
