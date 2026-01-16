# React Hook & Context 타입

### 📅 날짜:

> 2025.12.04 (목)
> 

### 📘 오늘 공부한 주제:

> React Hook(useState, useRef, useParams) 타입 적용, Context 타입 적용
> 

---

## 📝 핵심 개념 요약

### 1. useState 타입 적용

- `useState<T>`로 상태의 타입을 지정 가능
- 객체, 배열, 유니온, 함수형 업데이트 등 다양한 패턴에서 제네릭 활용
- 초기값과 제네릭 타입을 일치시키면 타입 안전성이 강화됨

### 2. useRef 타입 적용

- DOM 요소 참조: `useRef<HTMLInputElement>(null)`
- 값 저장용: `useRef<number>(0)`, 함수 저장용: `useRef<(() => void) | null>(null)`
- null 체크 후 사용, currentTarget 사용 권장

### 3. useParams 타입 적용

- Next.js `useParams` 기본 타입: `{ [key: string]: string | undefined }`
- 타입 안전한 방법: 제네릭으로 params 인터페이스 지정
- Provider 밖에서 undefined 접근 시 타입 가드 필요

### 4. Context 타입 적용

- **방법 1:** 기본값 제공, undefined 없음 → 사용 편리하지만 Provider 누락 체크 불가
- **방법 2 (권장):** `undefined` 허용, useContext에서 undefined 체크 → Provider 누락 시 타입 레벨에서 안전성 확보
- setState 함수 타입: `React.Dispatch<React.SetStateAction<T>>`

## 📊 핵심 요약 표

| 구분 | 설명 |
| --- | --- |
| useState | `useState<T>(initialValue)`로 제네릭 지정, 객체/배열/유니온 가능 |
| useRef | `useRef<T>(null)`로 DOM 요소 또는 값 저장, currentTarget 사용 권장 |
| useParams | `{ [key: string]: string |
| Context | 기본값 제공 방식 / undefined 허용 방식, setState 타입 안전하게 지정 |
| 고급 패턴 | 함수형 업데이트, 커스텀 훅 반환값, 제네릭 이벤트 핸들러 등 |

### 💻 실습 내용 정리

### [1] Hook 타입 적용

```bash
git checkout -b 3_hook
git reset --hard origin/3_hook

```

- PostList.tsx, PostDetail.tsx, PostForm.tsx
    - 모든 `useState` 상태값 제네릭 타입 정의
    - 모듈 파일 내 타입 오류 수정
- PostForm.tsx
    - `useRef` 제네릭으로 ref 가리키는 DOM 타입 지정
- posts/[id]/edit/page.tsx
    - `useParams`와 `useState` 제네릭으로 타입 안전하게 정의

### [2] Context 타입 적용

```bash
git checkout -b 4_context
git reset --hard origin/4_context

```

- PostContext.tsx
    - PostContextType 정의 및 createContext 적용
    - setPosts, setLoading 등 setState 함수 타입 지정
    - PostProvider props 타입 정의
    - useRef 제네릭으로 ref.current 타입 지정
    - 남은 타입 오류 모두 제거 후 npm run build 확인

### ❗ 헷갈렸던 점 / 문제 해결:

| 문제 | 해결 |
| --- | --- |
| useState 초기값과 제네릭 불일치 | 초기값과 제네릭 일치, 타입 오류 제거 |
| useRef null 체크 필요 | if(ref.current) {...} 안전하게 사용 |
| useParams 타입 안전성 | 제네릭 인터페이스 지정, undefined 체크 |
| Context Provider 누락 시 오류 | undefined 허용, useContext에서 타입 가드 적용 |

### 💡 느낀 점 / 배운 점:

- useState, useRef 제네릭 활용으로 상태 관리와 DOM 접근 시 타입 안전성 확보
- useParams와 Context에서 제네릭과 undefined 체크를 통해 Provider 누락/URL param 누락 오류 방지
- setState 타입, 커스텀 훅 반환값 등 실무 패턴 익힘
- Hook과 Context를 조합하면 TypeScript에서 React 앱 안정성이 크게 향상됨

### 🏷️ 키워드 (태그):

`#React` `#TypeScript` `#Hook` `#useState` `#useRef` `#useParams` `#Context` `#Provider` `#setState` `#제네릭` `#타입안전성`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-12-04 | React Hook & Context 타입 | useState/useRef/useParams 제네릭, Context 타입 안전성, Provider 누락 체크, currentTarget 안전성 | PostList, PostDetail, PostForm Hook 타입 정의, useRef 적용, useParams 제네릭 적용, PostContext 타입 정의 및 적용, setState 타입 지정 | 초기값과 제네릭 불일치 해결, ref null 체크, Provider 누락 타입 가드 적용, 빌드 성공 | `React` `TypeScript` `Hook` `useState` `useRef` `useParams` `Context, Provider` `제네릭` `타입안전성` | ✅ |
