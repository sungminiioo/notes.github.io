# Tanstack Query & React Hook Form + Zod

### 📅 날짜:

> 2025.12.05 (금)
> 

### 📘 오늘 공부한 주제:

> Tanstack Query 타입 적용, React Hook Form 타입 적용, Zod 스키마 통합
> 

---

## 📝 핵심 개념 요약

### 1. Tanstack Query 타입

- **useQuery**
    
    ```tsx
    useQuery<TQueryFnData, TError, TData, TQueryKey>(options)
    ```
    
    - TQueryFnData: queryFn 반환 데이터 타입
    - TError: 에러 타입
    - TData: select 함수 반환 타입
    - TQueryKey: queryKey 타입
- **useMutation**
    
    ```tsx
    useMutation<TData, TError, TVariables, TContext>(options)
    ```
    
    - TData: mutationFn 반환 타입
    - TVariables: mutationFn 매개변수 타입
    - TContext: optimistic update context 타입
- **useInfiniteQuery**
    
    ```tsx
    useInfiniteQuery<TQueryFnData, TError, TData, TQueryKey, TPageParam>(options)
    ```
    
    - TPageParam: pageParam 타입

### 2. React Hook Form

- **폼 상태 관리 최적화**
- TypeScript 제네릭으로 폼 데이터 타입 지정 가능
- 자동 유효성 검사 및 불필요한 리렌더링 최소화

### 3. Zod

- TypeScript 우선 스키마 검증 라이브러리
- 스키마에서 타입 자동 추출 (`z.infer<typeof schema>`)
- 프론트엔드/백엔드 재사용 가능
- React Hook Form과 통합 시 검증 로직 컴포넌트 외부로 분리 가능

## 📊 핵심 요약 표

| 라이브러리 | 특징 | 타입 적용 방식 |
| --- | --- | --- |
| Tanstack Query | useQuery, useMutation, useInfiniteQuery | 제네릭: 반환 타입, 에러 타입, select 타입, queryKey 타입, pageParam 타입 등 |
| React Hook Form | 폼 상태 관리, 유효성 검사 자동화 | useForm, SubmitHandler, register() |
| Zod | TypeScript 스키마 검증 | z.object({...}) → z.infer |

### 💻 실습 내용 정리

### [1] Tanstack Query 타입 적용

```bash
git checkout 5_tanstack
git reset --hard origin/5_tanstack
npm i
```

- PostList.tsx: useQuery 제네릭 타입 적용
- posts/new/page.tsx: useMutation 제네릭 타입 적용
- PostDetail.tsx: useMutation 제네릭 타입 적용

### [2] useInfiniteQuery 타입 적용

```bash
git checkout -b 6_infinite
git reset --hard origin/6_infinite
```

- PostList.tsx: useInfiniteQuery 제네릭 타입 적용

### [3] React Hook Form 마이그레이션

1. **useState 기반 기본 폼** → 상태, 핸들러, validateForm 수동 구현
2. **React Hook Form 적용** → register + handleSubmit, formState.errors 사용
3. **Zod 통합** → 외부 스키마 정의 + zodResolver 연결, 타입 추출 자동화

### ❗ 헷갈렸던 점 / 문제 해결:

| 문제 | 해결 |
| --- | --- |
| useQuery / useMutation 제네릭 타입 | queryFn, mutationFn 반환 타입과 매개변수 타입 정확히 지정 |
| useInfiniteQuery pageParam 타입 | 제네릭 TPageParam 적용, fetchNextPage 안전성 확보 |
| RHF 초기 useState 비교 | register + handleSubmit로 state 관리 자동화 |
| Zod 통합 시 타입 불일치 | z.infer로 FormData 타입 추출 후 useForm 제네릭 적용 |

### 💡 느낀 점 / 배운 점:

- Tanstack Query 제네릭 타입 적용으로 **쿼리/변이/무한 스크롤 데이터 타입 안전성** 확보
- React Hook Form으로 **코드 간소화 및 자동 유효성 검사** 구현
- Zod와 통합하면 **검증 로직 외부 분리, 타입 자동 생성, 서버와 공유 가능**
- 실무 프로젝트에서 폼 + 서버 데이터 연동 시 **타입 안정성과 코드 유지보수성 향상**

### 🏷️ 키워드 (태그):

`#React` `#TypeScript` `#TanstackQuery` `#useQuery` `#useMutation` `#useInfiniteQuery` `#ReactHookForm` `#Zod` `#폼관리` `#유효성검사` `#타입안전성`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-12-05 | Tanstack Query & React Hook Form + Zod | Tanstack Query 제네릭 타입, RHF + Zod 타입 안전 폼, 자동 유효성 검사 | PostList, PostDetail useQuery/useMutation 타입 적용, useInfiniteQuery 적용, RHF + Zod 폼 구현 | 제네릭 타입 지정, pageParam 안전성 확보, RHF register + handleSubmit 사용, Zod 타입 추출 | `React` `TypeScript` `TanstackQuery` `useQuery` `useMutation` `useInfiniteQuery` `ReactHookForm` `Zod` `폼관리` `유효성검사` `타입안전성` | ✅ |
