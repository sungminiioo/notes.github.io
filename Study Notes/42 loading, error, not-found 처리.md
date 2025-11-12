# loading, error, not-found 처리

### 📅 날짜:

> 2025.09.03 (수)
> 

### 📘 오늘 공부한 주제:

> Next.js의 `loading.jsx`, `error.jsx`, `global-error.jsx`, `not-found.jsx` 처리 방식
> 

---

## 📝 핵심 개념 요약

- **loading.jsx**: 페이지 렌더링 중 임시 UI 제공 (Suspense 기반) → SSR과 함께 주로 사용
- **error.jsx**: 특정 라우트 내에서 발생한 에러 처리 (ErrorBoundary 기반, 클라이언트 컴포넌트 필요)
- **global-error.jsx**: 앱 전체에서 발생하는 치명적 오류 처리 (루트 레벨, 반드시 설정 권장)
- **not-found.jsx**: 존재하지 않는 페이지 요청 시 404 UI 제공 (서버 컴포넌트 가능, async 지원)

## 📊 핵심 요약 표

| 처리 컴포넌트 | 특징 | 적용 기술 | 주요 활용 |
| --- | --- | --- | --- |
| `loading.jsx` | 렌더링 중 임시 UI 표시 | React Suspense | API 요청 지연 시 사용자 경험 개선 |
| `error.jsx` | 특정 페이지/세그먼트 내 에러 처리 | ErrorBoundary | 사용자 친화적 에러 메시지 제공 |
| `global-error.jsx` | 앱 전역 오류 처리 | ErrorBoundary (root) | 서버 다운/네트워크 실패 같은 치명적 오류 대응 |
| `not-found.jsx` | 없는 라우트(404) 처리 | 서버 컴포넌트 가능 | 커스텀 404 페이지, async 데이터 사용 가능 |

### 💻 실습 내용 정리

- **loading.jsx**
    
    ```jsx
    export default async function Page() {
      const data = await fetchData()
      return <Something data={data} />
    }
    ```
    
    → `fetch` 요청 대기 시간 동안 `loading.jsx` UI 표시
    
- **error.jsx**
    
    ```jsx
    'use client'
    import { useEffect } from 'react'
    
    export default function Error({ error, reset }) {
      useEffect(() => { console.error(error) }, [error])
    
      return (
        <div>
          <h2>Something went wrong!</h2>
          <button onClick={() => reset()}>Try again</button>
        </div>
      )
    }
    ```
    
    → 특정 세그먼트에서 발생한 에러 처리 및 복구 버튼 제공
    
- **global-error.jsx**
    
    ```jsx
    'use client'
    
    export default function GlobalError({ error, reset }) {
      return (
        <html>
          <body>
            <h2>Something went wrong!</h2>
            <button onClick={() => reset()}>Try again</button>
          </body>
        </html>
      )
    }
    ```
    
    → 전역 에러 핸들링, `html`/`body` 포함 필수
    
- **not-found.jsx**
    
    ```jsx
    import Link from 'next/link'
    
    export default async function NotFound() {
      return (
        <div>
          <h2>Not Found</h2>
          <p>Could not find requested resource</p>
          <p>
            View <Link href="/">홈으로 이동</Link>
          </p>
        </div>
      )
    }
    ```
    
    → 존재하지 않는 페이지 접근 시 404 UI 제공
    

### ❗ 헷갈렸던 점 / 문제 해결:

- `error.jsx`와 `global-error.jsx`의 차이를 처음에 헷갈림 → **local error vs global error** 개념으로 정리하니 명확해짐.
- `loading.jsx`가 SSR에서 주로 활용된다는 점을 놓칠 뻔 → Suspense 기반이므로 SSR과 잘 어울린다는 점을 기억하기.

### 💡 느낀 점 / 배운 점:

- Next.js는 라우트 단위에서 상태(loading)와 에러(error, not-found)를 처리할 수 있어서 **사용자 경험(UX)**을 훨씬 높여줌.
- 특히 `global-error.jsx`를 반드시 설정해둬야 예상치 못한 장애 상황에서도 안전한 fallback을 제공할 수 있음.
- 기존 CSR에서 직접 에러 바운더리를 구현하던 방식보다 훨씬 직관적이고 편리함.

### 🏷️ 키워드 (태그):

`Next.js` `loading.jsx` `error.jsx` `global-error.jsx` `not-found.jsx` `Suspense` `ErrorBoundary` `SSR` `UX 개선`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-09-03 | Next.js 상태/에러 처리 | `loading.jsx`: 임시 UI, `error.jsx`: 페이지 에러, `global-error.jsx`: 전역 에러, `not-found.jsx`: 404 처리 | API fetch 시 loading UI, error boundary, global fallback, not-found 페이지 작성 | error vs global-error 구분, SSR+Suspense 이해 | `#Next.js` `#loading` `#error` `#global-error` `#not-found` `#Suspense` | ✅ |
