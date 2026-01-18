#  React 별점 컴포넌트, 글 CRUD, Hook 활용

### 📅 날짜:

> 2025.08.12 (화)
> 

### 📘 오늘 공부한 주제:

> React 파일 인풋 다루기 (파일 선택, ref 사용, 초기화)
> 

---

## 📝 핵심 개념 요약

| 주제 | 핵심 내용 |
| --- | --- |
| 별점 컴포넌트 | `useState`로 현재 별점 값 관리, `Array.from().map()`으로 별 아이콘 반복 출력, 클릭·호버 이벤트 처리, Props(`max`, `value`, `onChange`) 전달, 접근성 고려(키보드 입력·aria-label) |
| Axios 응답 데이터 | `{ data } = await axios.get(...)` 후 상태 업데이트, 에러 발생 시 `error.response.data` 확인 |
| 글 수정하기 | `useState`에 초기값 설정 → `onChange`로 값 변경 → PUT/PATCH 요청으로 저장 |
| 글 삭제하기 | DELETE 요청 후 목록 갱신 또는 페이지 이동 |
| Hook 개념 | 특정 기능을 React 상태·생명주기에 연결하는 방법 |
| Hook 규칙 | ① 최상위에서만 호출 ② 함수 컴포넌트/커스텀 훅에서만 사용 ③ 이름은 `use`로 시작 ④ 호출 순서 고정 |
| useState | 상태 관리, 이전 값 기반 업데이트 가능 |
| useEffect | 사이드 이펙트 실행, Cleanup 함수로 정리 가능 |
| useRef | DOM 참조·값 유지 |
| useCallback | 의존성 변경 시에만 함수 생성 |
| Custom Hook | 반복되는 로직을 재사용 가능(`useAsync`, `useToggle`, `useTimer`) |

## 📊 핵심 요약 표

| 주제 | 핵심 요약 |
| --- | --- |
| 별점 컴포넌트 | 별 아이콘 반복 출력 + 상태 관리 + 이벤트 처리 + 접근성 |
| Axios 데이터 | 데이터 구조 분해·에러 처리 주의 |
| 글 수정 | 초기값 로딩 → 상태 업데이트 → PUT/PATCH 요청 |
| 글 삭제 | DELETE 요청 후 UI 반영 |
| Hook 규칙 | 최상위 호출, `use` 네이밍, 호출 순서 고정 |
| Custom Hook | 로직 재사용(`useAsync`, `useToggle`, `useTimer`) |

### 💻 실습 내용 정리

---

### 1. 별점 컴포넌트

```jsx
function StarRating({ max = 5, value, onChange }) {
  const [hover, setHover] = useState(null);

  return (
    <div>
      {Array.from({ length: max }, (_, i) => (
        <spankey={i}
          role="button"
          aria-label={`${i + 1}점`}
          style={{ color: (hover || value) > i ? 'gold' : 'gray' }}
          onClick={() => onChange(i + 1)}
          onMouseEnter={() => setHover(i + 1)}
          onMouseLeave={() => setHover(null)}
        >
          ★
        </span>
      ))}
    </div>
  );
}
```

---

### 2. Axios 응답 데이터 사용

```jsx
const { data } = await axios.get(`/api/posts`);
setPosts(data);
```

- 에러 처리:

```jsx
try {
  const { data } = await axios.get('/api/posts');
} catch (error) {
  console.log(error.response.data);
}
```

---

### 3. 글 수정하기

```jsx
const [title, setTitle] = useState(post.title);

<inputvalue={title}
  onChange={(e) => setTitle(e.target.value)}
/>

await axios.put(`/api/posts/${id}`, { title });
```

---

### 4. 글 삭제하기

```jsx
await axios.delete(`/api/posts/${id}`);
navigate('/posts');
```

---

### 5. Hook 규칙 예시

```jsx
// ✅ 최상위에서만 호출
function MyComponent() {
  const [count, setCount] = useState(0);
  useEffect(() => console.log(count), [count]);
}
```

---

### 6. Custom Hook 예시

```jsx
function useToggle(initial = false) {
  const [value, setValue] = useState(initial);
  const toggle = () => setValue(v => !v);
  return [value, toggle];
}
```

### ❗ 헷갈렸던 점 / 문제 해결:

- 별점 컴포넌트에서 **hover 값**과 **선택 값**을 분리해야 깜빡임 없이 UI 표시 가능
- Axios 에러 객체에서 **`error.response`** 유무를 먼저 확인해야 안전
- Hook 규칙 위반 시 ESLint에서 자동 경고 발생 → 규칙 숙지 필요

### 💡 느낀 점 / 배운 점:

- UI 컴포넌트는 **상태 관리·이벤트 처리·접근성**을 함께 고려해야 완성도가 높음
- Axios 요청 시 항상 에러 케이스를 대비해야 함
- Hook 규칙은 단순 암기가 아니라 **동작 원리 이해**가 중요
- Custom Hook을 쓰면 중복 로직을 줄이고 가독성을 높일 수 있음

### 🏷️ 키워드 (태그):

`#React` `#별점컴포넌트` `#CRUD` `#Hook` `#useState` 

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-08-12 | React 별점·CRUD·Hook | 별점 구현, Axios 응답 처리, 글 수정·삭제, Hook 규칙·Custom Hook | 별점 컴포넌트, Axios 요청, 글 수정/삭제 코드, Hook 예시 | 상태·이벤트 분리, 에러 처리, Hook 규칙 숙지, 로직 재사용 |  `#React` `#Hook` `#CRUD` | ✅ |
