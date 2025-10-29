# Next.js App Router 핵심 정리

### 📅 날짜:

> 2025.08.25 (월)
> 

### 📘 오늘 공부한 주제:

> Routing (App Router)
> 

---

## 📝 핵심 개념 요약

- **라우팅**: URL → 어떤 페이지/컴포넌트 보여줄지 결정하는 것
- **App Router**: `app/` 디렉토리 기반, 최신 Next.js 권장 방식
- **서버 컴포넌트**: 기본값, 서버에서 렌더링 후 클라이언트로 전송 (hook, 브라우저 API 불가)
- **클라이언트 컴포넌트**: `"use client"` 필요, 브라우저 API & hook 사용 가능
- **layout.tsx**: 특정 폴더/하위 모든 page에 공통 레이아웃 적용
- **Link**: 단순 페이지 이동 (SEO 반영, 프리패치 가능)
- **useRouter**: 조건부/이벤트 기반 네비게이션 (API 성공 후 이동 등)

## 📊 핵심 요약 표

| 구분 | 설명 |
| --- | --- |
| **Routing** | 주소 → 어떤 페이지를 보여줄지 결정 |
| **App Router** | `app/` 폴더 기반, 최신 Next.js 권장 |
| **서버 컴포넌트** | 기본값, 서버 렌더링, hook 불가 |
| **클라이언트 컴포넌트** | `"use client"` 필요, hook 가능 |
| **Layout** | `layout.tsx`가 같은 디렉토리 및 하위 page.tsx에 적용 |
| **Link** | 단순 페이지 이동, SEO 반영 |
| **useRouter** | 조건부/이벤트 기반 이동, SEO 반영 ❌ |

### 💻 실습 내용 정리

### 1) Layout 적용 예시

```jsx
// layout.tsx
function Layout({ children }) {
  return (
    <div className="layout">
      <Header />
      <main>{children}</main>
      <Footer />
    </div>
  );
}
```

```jsx
// page.tsx
function Page() {
  return (
    <Layout>
      <h1>페이지 제목</h1>
      <p>페이지 내용</p>
    </Layout>
  );
}
```

### 2) Link 사용 예시 (Client-side Navigation)

```jsx
import Link from 'next/link';

export default function Home() {
  return <Link href="/detail">상세보기</Link>;
}
```

### 3) useRouter 사용 예시 (이동 전에 로직 필요할 때)

```jsx
'use client'
import { useRouter } from 'next/navigation';

export default function Page() {
  const router = useRouter();

  const handleClick = () => {
    // API 요청 or 조건 확인 후 이동
    router.push('/dashboard');
  };

  return <button onClick={handleClick}>Dashboard</button>;
}
```

### ❗ 헷갈렸던 점 / 문제 해결:

- `Link`와 `useRouter`가 같은 기능처럼 보여서 혼동했으나,
    
    👉 **SEO 반영 여부, 이동 트리거 방식(API/조건문 대응 가능 여부)** 차이가 있다는 걸 알게 됨.
    
- 서버 컴포넌트와 클라이언트 컴포넌트의 차이를 다시 정리하면서, `"use client"` 선언 이유를 명확히 이해할 수 있었음.

### 💡 느낀 점 / 배운 점:

- Next.js 라우팅은 단순히 URL 매핑 이상의 의미가 있었음.
- 특히 **App Router vs Pages Router**의 차이는 단순 문법이 아니라, **React의 한계를 보완하려는 Next.js의 철학**이 반영된 선택이라는 걸 이해하게 됨.
- 앞으로 코드를 볼 때, “이게 서버 컴포넌트인지 클라이언트 컴포넌트인지”를 먼저 구분하는 습관이 필요할 것 같음.

### 🏷️ 키워드 (태그):

`#Next.js` `#AppRouter` `#PagesRouter` `#라우팅` `#서버컴포넌트` `#클라이언트컴포넌트` `#Link` `#useRouter` `#Layout`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-08-25 | Next.js App Route | App Router 동작 방식, 서버/클라이언트 컴포넌트 구분, Link vs useRouter 차이 | Layout 적용, Link 예제, useRouter 예제 | Link & useRouter 차이 명확히 이해, 서버/클라이언트 구분 확실히 정리 | `#Next.js` `#AppRouter`
 `#라우팅` 
`#서버컴포넌트` `#useRouter` | ✅ |
