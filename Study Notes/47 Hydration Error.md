# Hydration Error 

### 📅 날짜:

> 2025.09.10 (수)
> 

### 📘 오늘 공부한 주제:

> Next.js/React에서 Hydration Error 발생 원인과 해결 방법
> 

---

## 📝 핵심 개념 요약

- **Hydration Error**: 서버에서 렌더링된 HTML과 클라이언트에서 렌더링된 결과물이 일치하지 않을 때 발생.
- 발생 원인:
    1. 클라이언트 전용 API 사용 (`window`, `localStorage` 등)
    2. 서버/클라이언트 초기 상태 불일치 (`Math.random()`, `Date.now()` 등)
    3. 조건부 렌더링 시 서버/클라이언트 차이
- **해결 방법**:
    - `useEffect` 사용 → 클라이언트에서만 실행되도록 지연
    - `dynamic import` + `ssr: false` → 서버에서 렌더링 제외
    - 초기 상태 서버/클라이언트 동일하게 맞추기

## 📊 핵심 요약 표

| 발생 원인 | 예시 | 해결 방법 |
| --- | --- | --- |
| 클라이언트 전용 API 사용 | `window.location.href` | `useEffect` 내에서 클라이언트 실행 |
| 서버/클라이언트 초기 상태 불일치 | `Math.random()` | 초기값 통일 후 `useEffect`에서 업데이트 |
| 조건부 렌더링 차이 | `localStorage` 사용 여부 | `dynamic import` + `ssr: false` |

### 💻 실습 내용 정리

1. **클라이언트 전용 API 사용 예제**

```jsx
'use client';
import { useState, useEffect } from 'react';

function UserLocation() {
  const [location, setLocation] = useState('');

  useEffect(() => {
    setLocation(window.location.href);
  }, []);

  return <div>현재 URL: {location}</div>;
}
```

1. **서버/클라이언트 초기 상태 불일치**

```jsx
'use client';
import { useState, useEffect } from 'react';

function RandomNumber() {
  const [number, setNumber] = useState(0);

  useEffect(()=>{
    setNumber(Math.random());
  }, [])

  return <div>랜덤 숫자: {number}</div>;
}
```

1. **조건부 렌더링 불일치**

```jsx
'use client';
import { useState, useEffect } from 'react';
import dynamic from 'next/dynamic';

const ClientOnlyComponent = dynamic(() => import('./ClientComponent'), { ssr: false });

function SafeComponent() {
  const [isClient, setIsClient] = useState(false);

  useEffect(() => {
    setIsClient(true);
  }, []);

  return (
    <div>
      {isClient ? <ClientOnlyComponent /> : <p>로딩 중...</p>}
    </div>
  );
}
```

### ❗ 헷갈렸던 점 / 문제 해결:

- 서버 렌더링에서는 `window`, `localStorage`, `Math.random()` 등이 사용 불가 → `useEffect` 또는 `dynamic import`로 처리
- 조건부 렌더링 시 서버와 클라이언트 상태 차이로 hydration error 발생 → 서버 초기값을 맞추거나 클라이언트 전용 컴포넌트로 분리

### 💡 느낀 점 / 배운 점:

- Next.js에서 **SSR과 CSR 간 차이**를 이해하는 것이 중요
- dynamic import(ssr: false)는 SEO가 필요 없는 UI 처리에 안전하게 사용 가능
- Hydration Error는 대부분 클라이언트 전용 코드와 상태 불일치에서 발생

### 🏷️ 키워드 (태그):

`#Next.js` `#React` `#Hydration Error` `#SSR` `#CSR` `#useEffect` `#dynamic import` `#클라이언트 전용 컴포넌트`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-09-10 | Next.js Hydration Error | 서버와 클라이언트 렌더링 불일치로 발생. 클라이언트 전용 API, 초기 상태 차이, 조건부 렌더링 원인 | useEffect 적용, 초기값 맞추기, dynamic import | 서버에서 window/Math.random 사용 주의, dynamic import 활용  | `#Next.js` `#React` `#Hydration Error` `#SSR` `#CSR` `#useEffect` `#dynamic import` | ✅ |
