# middleware

### 📅 날짜:

> 2025.09.11 (목)
> 

### 📘 오늘 공부한 주제:

> Next.js Middleware 기능과 권한/인증 처리 방식
> 

---

## 📝 핵심 개념 요약

- **Middleware**: 특정 경로 요청이 서버에서 처리되기 전에 중간에서 작업 수행
- **대표적 용도**: 인증/인가 처리, API 요청 검증, URL 리다이렉트/리라이트
- **NextResponse 주요 메소드**:
    1. `NextResponse.next()` → 요청을 그대로 처리
    2. `NextResponse.redirect(url)` → 지정 경로로 리다이렉트
    3. `NextResponse.rewrite(url)` → 요청 URL은 유지, 다른 페이지 컴포넌트 실행
    4. `NextResponse.json(data)` → 요청 전에 JSON 응답
- **권한 처리 패턴**
    - **redirect**: 권한 없는 사용자 홈/로그인 페이지 이동
    - **rewrite**: 특정 조건에 따라 다른 페이지 컴포넌트 실행
    - **빠른 API 응답**: 토큰 없으면 JSON 응답 후 요청 종료

## 📊 핵심 요약 표

| 기능 | 사용 예시 | 설명 |
| --- | --- | --- |
| next() | `NextResponse.next()` | 요청 계속 진행 |
| redirect | `NextResponse.redirect(new URL('/about', request.url))` | 다른 경로로 이동 |
| rewrite | `NextResponse.rewrite(new URL('/faq', request.url))` | URL 유지, 다른 컴포넌트 실행 |
| json | `NextResponse.json({ data })` | 요청 전에 JSON 응답 |

| 권한 처리 패턴 | 설명 | 예시 |
| --- | --- | --- |
| redirect | 권한 없는 사용자 이동 | `/profile` 접근 시 token 없으면 `/` 이동 |
| rewrite | 특정 조건에 따라 다른 페이지 제공 | `/posts` 접근 시 admin이면 `/admin/posts`로 rewrite |
| 빠른 API 응답 | 권한 없는 API 요청 빠르게 응답 | `/api/profile` 접근 시 token 없으면 JSON 반환 |

### 💻 실습 내용 정리

1. **권한에 따른 redirect**

```jsx
import { NextResponse } from "next/server";
import type { NextRequest } from "next/server";

export function middleware(request: NextRequest) {
  if (request.nextUrl.pathname.startsWith("/profile")) {
    const token = request.cookies.get("token")?.value;
    if (!token) {
      return NextResponse.redirect(new URL("/", request.url));
    }
    return NextResponse.next();
  }
}

export const config = {
  matcher: ["/((?!api|_next/static|_next/image|favicon.ico).*)"],
};
```

1. **권한에 따른 rewrite**

```jsx
import { NextResponse } from "next/server";
import type { NextRequest } from "next/server";

export function middleware(request: NextRequest) {
  const membershipLevel = request.cookies.get('membershipLevel')?.value || 'guest';
  if (request.nextUrl.pathname.startsWith("/posts")) {
    if (membershipLevel === "admin") {
      return NextResponse.rewrite(new URL("/admin/posts", request.url));
    }
    return NextResponse.next();
  }
}

export const config = {
  matcher: ["/((?!_next/static|_next/image|favicon.ico).*)"],
};
```

1. **권한 없는 API 요청 시 JSON 응답**

```jsx
import { NextResponse } from "next/server";
import type { NextRequest } from "next/server";

export function middleware(request: NextRequest) {
  if (request.nextUrl.pathname.startsWith("/api/profile")) {
    const token = request.cookies.get("token")?.value;
    if (!token) {
      return NextResponse.json({ message: "token 이 없습니다." });
    }
  }
  return NextResponse.next();
}

export const config = {
  matcher: ["/((?!_next/static|_next/image|favicon.ico).*)"],
};
```

### ❗ 헷갈렸던 점 / 문제 해결:

- `middleware.ts`는 반드시 `src/` 경로에 위치해야 하며, `app/` 안으로 넣으면 동작하지 않음
- matcher 설정을 잘못하면 필요한 경로가 middleware 적용에서 빠질 수 있음

### 💡 느낀 점 / 배운 점:

- Middleware는 **인증/인가 처리**를 서버 레벨에서 간단하게 구현 가능
- `NextResponse` 메소드별 기능 이해가 중요
- redirect, rewrite, JSON 응답으로 다양한 요청 흐름 제어 가능

### 🏷️ 키워드 (태그):

`#Next.js` `#Middleware` `#NextResponse` `#Authorization` `#Rewrite` `#Redirect` `#API 보호`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-09-11 | Next.js Middleware | 서버 요청 전 처리, 권한/인가 제어 가능 | redirect, rewrite, API JSON 응답 | middleware 위치(src 중요), matcher 주의 | `#Next.js` `#Middleware` `#NextResponse` `#Authorization` `#Rewrite` `#Redirect` `#API 보호` | ✅ |
