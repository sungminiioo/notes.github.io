# SSR 페이지 Streaming

### 📅 날짜:

> 2025.09.12 (금)
> 

### 📘 오늘 공부한 주제:

> SSR 페이지에서 Streaming 적용과 Suspense를 통한 UX/SEO 최적화
> 

---

## 📝 핵심 개념 요약

- **Streaming**: 서버가 클라이언트에게 HTML을 한 번에 보내는 대신, 부분적으로 점진적으로 보내는 방식
- 페이지 일부가 준비되면 바로 렌더링 → 사용자 체감 속도 개선
- **Suspense와 함께 사용**:
    - `Suspense fallback`으로 부분 로딩 처리
    - SEO 친화적, UX 개선 가능
- **SSR Streaming 장점**:
    1. 초기 로딩 시간 단축
    2. 점진적 렌더링으로 UX 향상
    3. SEO에도 영향 적음 (콘텐츠가 서버에서 렌더링되어 전달됨)

## 📊 핵심 요약 표

| 개념 | 설명 | 장점 |
| --- | --- | --- |
| Streaming | 서버가 HTML을 부분적으로 전송 | 초기 화면 빠르게 표시, UX 개선 |
| Suspense | 로딩 중 fallback UI 표시 | 사용자에게 로딩 상태 안내, 점진적 렌더링 가능 |
| SSR Streaming | SSR + Streaming | SEO 유지, 사용자 경험 향상 |

### 💻 실습 내용 정리

```jsx
// page.jsx (SSR 페이지)
import { Suspense } from "react";

export default async function SSRPage() {
  await fetch("서버URL"); // 이 부분은 loading.jsx 에서 처리

  return (
    <>
      <Suspense fallback={<div>로딩중...</div>}>
        <SSRComponent1 />
      </Suspense>
      <Suspense fallback={<div>로딩중...</div>}>
        <SSRComponent2 />
      </Suspense>
    </>
  );
}
```

- 각 컴포넌트가 준비되는 즉시 렌더링
- `fallback`으로 로딩 UI 제공

### ❗ 헷갈렸던 점 / 문제 해결:

- Streaming 시 SEO 문제 여부 → 서버에서 이미 HTML이 렌더링되므로 일반적인 SSR과 동일하게 검색엔진 인덱싱 가능
- Suspense 내 비동기 데이터 fetch 시 로딩 처리 필요 → `loading.jsx`로 분리

### 💡 느낀 점 / 배운 점:

- SSR Streaming + Suspense 조합은 SEO를 유지하면서 UX를 크게 개선할 수 있는 전략
- 초기 로딩 화면과 부분 렌더링을 쉽게 구현 가능
- 점진적 HTML 전송 방식 이해가 SSR 최적화에 중요

### 🏷️ 키워드 (태그):

`#Next.js` `#SSR` `#Streaming` `#Suspense` `#점진적 렌더링` `#SEO 최적화` `#UX 개선`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-09-12 | SSR 페이지 Streaming | 서버가 HTML을 부분 전송, Suspense로 로딩 UI 제공, SEO 영향 없음 | SSRPage에서 Suspense fallback 적용 | SEO 영향 없음 확인, 부분 렌더링으로 UX 개선 | `#Next.js` `#SSR` `#Streaming` `#Suspense` `#SEO` `#UX` | ✅ |
