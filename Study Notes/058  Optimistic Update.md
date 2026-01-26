# Optimistic Update

### 📅 날짜:

> 2025.09.25 (목)
> 

### 📘 오늘 공부한 주제:

> onMutate / onError / onSettled 의 역할
> 

---

## 📝 핵심 개념 요약

- **Optimistic Update**: 서버 성공 여부 확인 전에 UI를 먼저 업데이트하는 UX 전략.
- `onMutate`: 낙관적 업데이트 실행 + 이전 상태 백업.
- `onError`: 서버 요청 실패 시 백업 데이터로 롤백.
- `onSettled`: 성공/실패 여부 상관없이 최신 데이터 refetch.

## 📊 핵심 요약 표

| 단계 | 설명 | TanStack Query Hook |
| --- | --- | --- |
| onMutate | 요청 전 실행. 기존 쿼리 취소 + 현재 데이터 백업 + UI 즉시 업데이트 | `cancelQueries`, `getQueryData`, `setQueryData` |
| onError | 요청 실패 시 실행. UI를 롤백(백업 데이터로 복원) | `setQueryData` |
| onSettled | 요청 성공/실패 여부와 관계없이 실행. 서버 최신 데이터 가져오기 | `invalidateQueries` |

### 💻 실습 내용 정리

```jsx
import { useMutation, useQueryClient } from "@tanstack/react-query";

const queryClient = useQueryClient();

useMutation({
  mutationFn: updateTodo,

  // 💡 낙관적 업데이트 실행
  onMutate: async (newTodo) => {
    // 1. 진행 중인 todos 쿼리 취소 (중복 갱신 방지)
    await queryClient.cancelQueries({ queryKey: ['todos'] });

    // 2. 현재 상태를 백업
    const previousTodos = queryClient.getQueryData(['todos']);

    // 3. 낙관적 업데이트 (UI 먼저 업데이트)
    queryClient.setQueryData(['todos'], (old) => [...old, newTodo]);

    // 4. 백업 데이터를 반환 (onError에서 사용)
    return { previousTodos };
  },

  // ❌ 실패 시 롤백
  onError: (err, newTodo, context) => {
    queryClient.setQueryData(['todos'], context.previousTodos);
  },

  // ✅ 성공/실패 관계없이 refetch
  onSettled: () => {
    queryClient.invalidateQueries({ queryKey: ['todos'] });
  },
});
```

### ❗ 헷갈렸던 점 / 문제 해결:

- ❓ "cancelQueries는 캐시를 지우는 건가?"
    
    → ❌ 지우는 게 아니라 **해당 쿼리의 네트워크 요청만 취소**하는 것.
    
- ❓ "롤백 데이터는 어디서 가져오나?"
    
    → `getQueryData`로 백업 후, `onMutate`에서 반환 → `onError`의 `context`로 접근.
    
- ❓ "왜 onSettled에서 invalidateQueries를 실행하나?"
    
    → 서버 요청 성공/실패 상관없이 최신 상태를 보장하기 위해.
    

### 💡 느낀 점 / 배운 점:

- 낙관적 업데이트는 UI 반응성을 높여주고, 특히 좋아요/팔로우 같은 기능에서는 사실상 필수 UX임.
- `onMutate → onError → onSettled` 흐름을 이해하니 "백업 후 롤백, 마지막에 최신화"라는 구조가 명확해짐.
- 앞으로는 `cancelQueries`, `getQueryData`, `setQueryData`, `invalidateQueries`를 묶어 생각해야겠다고 느꼈음.

### 🏷️ 키워드 (태그):

`#tanstack-query` `#optimistic-update` `#useMutation` `#rollback` `#invalidateQueries` 
`#react-query`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-09-25 | Optimistic Update (낙관적 업데이트) | UI 먼저 업데이트 → 실패 시 롤백 → 성공/실패 무관 refetch | onMutate로 백업 & 즉시 반영, onError로 롤백, onSettled로 최신화 | cancelQueries는 요청 취소, 롤백은 getQueryData 백업 활용 | `#tanstack-query` `#optimistic-update` `#useMutation` `#rollback` `#invalidateQueries` | ✅ |
