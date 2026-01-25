# useMutation 기본 사용법

### 📅 날짜:

> 2025.09.19 (금)
> 

### 📘 오늘 공부한 주제:

> `useMutation` 기본 사용법과 캐시 무효화(invalidateQueries) 개념
> 

---

## 📝 핵심 개념 요약

- `useMutation`: 서버 데이터 조작(Create, Update, Delete) 담당 (CRUD 중 **CUD**)
- 데이터 변경이 발생하면, 화면(UI) 최신화가 필요 → `invalidateQueries` or `revalidate` 사용
- `mutationFn`은 **Promise를 반환**해야 하며, **매개변수는 하나만 받을 수 있음**
    
    → 여러 개가 필요하면 **객체 구조분해** 방식으로 전달
    
- `onSuccess` 훅을 활용해 `queryClient.invalidateQueries` 호출 → 관련 데이터 새로고침

## 📊 핵심 요약 표

| 개념 | 설명 | 예시 |
| --- | --- | --- |
| useMutation | 서버 데이터 조작(CUD) | `useMutation({ mutationFn, onSuccess })` |
| mutationFn | Promise 반환 필수, 매개변수는 1개만 가능 | `(todo) => postTodo(todo)` |
| invalidateQueries | 지정된 queryKey의 캐시를 stale 상태로 → useQuery 재실행 | `queryClient.invalidateQueries({ queryKey: ['todos'] })` |
| 매개변수 처리 | 여러 값은 객체로 묶어 전달 | `({ id, liked }) => ...` |

### 💻 실습 내용 정리

### 1. 기본 사용법

```jsx
import { useMutation, useQueryClient } from "@tanstack/react-query";

const queryClient = useQueryClient();

const postTodoMutation = useMutation({
  mutationFn: postTodo, // Promise 반환 함수
  onSuccess: () => {
    // ['todos'] 캐시 무효화 → 자동 refetch
    queryClient.invalidateQueries({ queryKey: ['todos'] });
  },
});

// 버튼 클릭 시 실행
<buttononClick={() =>
    postTodoMutation.mutate({
      id: Date.now(),
      title: "Tanstack Query 공부",
    })
  }
>
  투두 추가
</button>
```

---

### 2. 매개변수 한 개만 허용되는 규칙

### ❌ 잘못된 예시 (매개변수 2개 전달)

```jsx
const { mutate: handleLike } = useMutation({
  mutationFn: (id, currentLiked) =>
    todoApi.patch(`/todos/${id}`, { liked: !currentLiked }),
  onSuccess: () => {
    queryClient.invalidateQueries(["todos"]);
  },
});
```

### ✅ 올바른 예시 (객체 구조분해)

```jsx
const { mutate: handleLike } = useMutation({
  mutationFn: ({ id, currentLiked }) =>
    todoApi.patch(`/todos/${id}`, { liked: !currentLiked }),
  onSuccess: () => {
    queryClient.invalidateQueries(["todos"]);
  },
});
```

### ❗ 헷갈렸던 점 / 문제 해결:

- `mutationFn`은 반드시 **매개변수 하나만** 받을 수 있다는 점에서 자주 실수 → 객체 구조분해 패턴으로 해결
- 데이터 변경 후 UI 최신화를 위해서는 **invalidateQueries** 필수

### 💡 느낀 점 / 배운 점:

- `useMutation`은 단순 요청 훅이 아니라, **데이터 최신화를 보장하는 캐시 무효화 흐름**과 함께 이해해야 함
- `invalidateQueries`를 통해 데이터 동기화가 자동으로 이뤄져서 로직이 단순해지고 유지보수가 쉬워짐

### 🏷️ 키워드 (태그):

`#TanStack Query` `#useMutation` `#invalidateQueries` `#캐시 무효화` `#CUD` `#Next.js` `#React Query`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-09-19 | useMutation 기본 사용법 | 서버 데이터 조작(CUD), invalidateQueries로 캐시 무효화, mutationFn은 매개변수 하나만 | postTodo 예제, 객체 구조분해 패턴 | mutationFn 인자 개수 제한 → 객체 구조분해로 해결, invalidateQueries로 최신화 흐름 이해 | `#TanStack Query` `#useMutation` `#invalidateQueries` `#캐시 무효화` `#CUD` | ✅ |
