# useQuery 에러 핸들링 패턴

### 📅 날짜:

> 2025.09.24 (수)
> 

### 📘 오늘 공부한 주제:

> `useQuery` 에러 핸들링 방법
> 

---

## 📝 핵심 개념 요약

- 에러 핸들링은 **컴포넌트 단위**와 **전역(Global) 단위**로 나눌 수 있음.
- **컴포넌트 단위**: `useEffect` 또는 return문에서 에러 표시.
- **글로벌 단위**: `QueryClient` 생성 시 `QueryCache.onError` 등록.
- meta 옵션을 활용하면 특정 쿼리의 에러만 식별하여 별도 처리 가능.

## 📊 핵심 요약 표

| 구분 | 설명 | 예제 포인트 |
| --- | --- | --- |
| 컴포넌트 단위 | 해당 컴포넌트에서 발생한 에러를 직접 처리 | `useEffect`로 alert, return문에서 에러 메시지 표시 |
| 글로벌 단위 | 모든 쿼리의 에러를 중앙집중적으로 관리 | `QueryCache.onError` + meta로 출처(source) 구분 |
| meta 활용 | 쿼리 실행 시 메타정보 추가 → 글로벌 에러 핸들링에서 필터링 가능 | `meta: { source: "todos" }` |

### 💻 실습 내용 정리

### 1 컴포넌트 단위 에러 핸들링

```jsx
function TodoList() {
  const { data: todos, error, isPending } = useQuery({
    queryKey: ['todos'],
    queryFn: fetchTodos,
  });

  useEffect(() => {
    if(error) {
      // alert는 사이드이펙트이므로 useEffect에서 처리
      alert(error.message);
    }
  }, [error]);

  if (isPending) return <p>Loading...</p>;

  if (error) {
    return <div style={{ fontSize: 24 }}>에러 발생: {error.message}</div>;
  }

  return (
    <div>
      {todos.data.map((todo) => (
        <Todo key={todo.id} {...todo} />
      ))}
    </div>
  );

```

### 2 글로벌 에러 핸들링 (QueryCache 활용)

```jsx
// main.jsx
const queryClient = new QueryClient({
  queryCache: new QueryCache({
    onError: (error, query) => {
      if (query.meta?.source === "todos") {
        toast.error(`TodoList 에러: ${error.message}`);
      }
    },
  }),
});

ReactDOM.createRoot(document.getElementById("root")).render(
  <QueryClientProvider client={queryClient}>
    <ReactQueryDevtools initialIsOpen={false} />
    <App />
  </QueryClientProvider>,
);

// src/components/TodoList.jsx
function TodoList() {
  const { data: todos, isPending } = useQuery({
    queryKey: ['todos'],
    queryFn: fetchTodos,
    meta: { source: "todos" },
  });

  if (isPending) return <p>Loading...</p>;

  return (
    <div>
      {todos.data.map((todo) => (
        <Todo key={todo.id} {...todo} />
      ))}
    </div>
  );
}
```

### ❗ 헷갈렸던 점 / 문제 해결:

- ❓ "에러를 alert로 띄우면 어디에 넣어야 하나?"
    
    → alert는 사이드이펙트이므로 **useEffect 안에서 처리**해야 함.
    
- ❓ "모든 컴포넌트마다 에러처리를 따로 써야 하나?"
    
    → 글로벌 `QueryCache` onError로 통합 관리 가능.
    
- ❓ "여러 쿼리가 있는데 특정 쿼리만 다른 방식으로 에러 처리하고 싶으면?"
    
    → `meta` 속성으로 구분 가능.
    

### 💡 느낀 점 / 배운 점:

- 에러 처리도 컴포넌트별 로컬 처리와 전역 처리로 나눠야 한다는 걸 알게 됨.
- meta를 활용하면 훨씬 더 유연하게 쿼리 구분이 가능해서 대규모 프로젝트에서 유용할 것 같음.
- 앞으로는 "전역 기본 에러 처리 + 일부 쿼리별 커스텀 처리" 조합으로 활용하는 게 최적이라고 느껴짐.

### 🏷️ 키워드 (태그):

`#tanstack-query` `#useQuery` `#error-handling` `#queryCache` `#meta` `#react-query`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-09-24 | useQuery 에러 핸들링 패턴 | 컴포넌트 단위(error + useEffect), 글로벌 단위(QueryCache), meta 활용 | 컴포넌트 에러 처리 코드, QueryCache onError 예제 | alert는 useEffect 안에서 / 글로벌 onError + meta로 통합 관리 | `#tanstack-query` `#useQuery` `#error-handling` `#queryCache` `#meta` | ✅ |
