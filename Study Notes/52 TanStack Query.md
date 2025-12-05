# TanStack Query

### 📅 날짜:

> 2025.09.17 (수)
> 

### 📘 오늘 공부한 주제:

> TanStack Query(React Query v4+)를 활용한 서버 상태 관리
> 

---

## 📝 핵심 개념 요약

- **서버 상태(Server State)** = API 호출을 포함한 **데이터 + 부수 상태들** (로딩, 에러, 성공 등)
- **TanStack Query가 쉽게 해주는 4가지**
    1. **Fetching**: API 호출로 데이터 가져오기
    2. **Caching**: 동일 데이터 재사용 (서버 재호출 방지)
    3. **Synchronizing**: 서버와 캐시 데이터 동기화
    4. **Updating**: 서버 데이터 변경 시 캐시 무효화 & UI 자동 업데이트
- **클라이언트 상태 vs 서버 상태**
    
    
    | 구분 | 클라이언트 상태 | 서버 상태 |
    | --- | --- | --- |
    | 저장 위치 | 브라우저/기기 내부 | 원격 서버 |
    | 제어 여부 | 직접 제어 가능 | 직접 제어 불가 |
    | 접근 방식 | 즉시 접근 | 네트워크 요청 필요 |
    | 예시 | 다크모드, 모달 열림 상태 | API 데이터, CRUD 요청 상태 |
- **TanStack Query 특징**
    - React Query v4부터 이름 변경 (React → TanStack)
    - Vue 등 다른 프레임워크 확장 가능
    - v4 이상에서는 `queryKey`를 **배열 형태**로 작성 필수

## 📊 핵심 요약 표

| 개념 | 설명 | 예시 |
| --- | --- | --- |
| Fetching | 서버에서 데이터 가져오기 | API 호출 |
| Caching | 가져온 데이터 저장/재사용 | 동일 요청 시 캐시 활용 |
| Synchronizing | 서버와 캐시 데이터 동기화 | invalidateQueries 사용 |
| Updating | 서버 데이터 수정 및 UI 갱신 | Mutation → 캐시 무효화 |

### 💻 실습 내용 정리

1. **기존 방식 (useState + useEffect)**

```jsx
function TodoList() {
  const [todos, setTodos] = useState([]);
  const [isLoading, setIsLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const getTodos = async () => {
      try {
        const data = await fetch(`${API_URL}/todos`).then(res => res.json());
        setTodos(data);
      } catch (error) {
        setError(error);
      } finally {
        setIsLoading(false);
      }
    };
    getTodos();
  }, []);

  if (isLoading) return <p>로딩중...</p>
  if (error) return <p>error: {error.message}</p>

  return <TodoListUI todos={todos} />
}
```

👉 **문제점**: 로딩/에러/데이터를 전부 직접 상태 관리해야 해서 번거롭고 에러 가능성 높음

1. **TanStack Query 방식**

```jsx
import { useQuery } from "@tanstack/react-query";

function TodoList() {
  const { data: todos, isLoading, error } = useQuery({
    queryKey: ["todos"],     // ✅ 배열 형태 필수
    queryFn: getTodos,       // API 호출 함수
  });

  if (isLoading) return <p>로딩중...</p>
  if (error) return <p>error: {error.message}</p>

  return <TodoListUI todos={todos} />
}
```

👉 **장점**: 로딩/에러/데이터 관리 자동화, 캐싱 및 재사용, 자동 동기화 제공

### ❗ 헷갈렸던 점 / 문제 해결:

- `queryKey`를 문자열로 쓰면 에러 발생 → v4 이상은 배열 필수
- 클라이언트 상태와 서버 상태의 차이 혼동 → 서버 상태는 "데이터 + 요청 상태" 전체를 포함

### 💡 느낀 점 / 배운 점:

- TanStack Query는 서버 상태 관리에 특화된 라이브러리로, 기존 `useState + useEffect` 대비 압도적으로 편리
- 데이터 가져오기 + 로딩/에러/성공 상태 관리가 한 줄 코드로 해결되는 점이 강력
- 실제 프로젝트에서 서버 데이터 CRUD에 활용하면 생산성과 안정성이 크게 향상될 것 같음

### 🏷️ 키워드 (태그):

`#TanStack Query` `#React Query` `#서버 상태 관리` `#Caching` `#Synchronizing` `#Mutation` `#invalidateQueries` `#React`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-09-17 | TanStack Query로 서버 상태 관리 | 서버 상태 관리(fetch, cache, sync, update)를 쉽게 해주는 라이브러리, queryKey 배열 필수 | useState/useEffect vs useQuery 비교 실습 | queryKey 배열 필수 이해, 클라이언트 vs 서버 상태 차이 명확해짐 | `#TanStack Query` `#React Query` `#서버 상태` `#Caching` `#Mutation` | ✅ |
