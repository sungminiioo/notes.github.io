# Next.js 핵심 개념 정리

### 📅 날짜:

> 2025.08.21 (목)
> 

### 📘 오늘 공부한 주제:

> Next.js 소개, 프레임워크 개념, 프리렌더링(SSR/SEO), 번들러(Webpack vs Turbopack), 프로젝트 시작하기
> 

---

## 📝 핵심 개념 요약

- **Next.js**: 리액트 기반 풀스택 프레임워크, 프론트엔드/백엔드 모두 지원
- **Framework**: 코드 실행 제어권이 개발자가 아닌 프레임워크에 있음 (제어 역전)
- **프리렌더링 (Pre-rendering)**: 서버에서 HTML을 미리 생성 → 초기 로딩 속도 개선, SEO 최적화
- **번들러(Webpack, Turbopack)**: JS/CSS/이미지를 묶어 브라우저가 이해할 수 있도록 변환
- **Next.js 시작하기**: `npx create-next-app@latest` → App Router, Tailwind CSS 선택

## 📊 핵심 요약 표

| 구분 | 설명 | 예시/비고 |
| --- | --- | --- |
| Next.js | React 기반 풀스택 프레임워크 | UI + API + 배포까지 지원 |
| 프레임워크 | 제어 역전(Inversion of Control) 구조 | 라우팅, 렌더링 규칙 제공 |
| 프리렌더링 | 서버에서 HTML 미리 생성 | 초기 속도 ↑, SEO ↑ |
| CSR vs SSR | CSR: 클라이언트 렌더링, SSR: 서버 렌더링 | SSR이 SEO에 유리 |
| Webpack | 안정적, 플러그인 다양, 속도는 다소 느림 | 실무/프로덕션 권장 |
| Turbopack | 매우 빠름, Rust 기반, 아직 베타 | 학습/실험 환경 권장 |
| 프로젝트 시작 | `npx create-next-app@latest` | `npm run dev` 실행 |

### 💻 실습 내용 정리

1. **Next.js 프로젝트 생성**

```bash
npx create-next-app@latest
```

- 옵션 선택
    - App Router ✅
    - Tailwind CSS ✅
    - src 폴더 ✅
    - Turbopack ❌ (Webpack 사용)
1. **개발 서버 실행**

```bash
npm run dev
```

→ [http://localhost:3000](http://localhost:3000/) 접속

1. **번들러 비교**
- Webpack: 안정적, 다양한 설정/플러그인 지원
- Turbopack: 빠른 빌드/핫 리로드, 하지만 아직 베타

### ❗ 헷갈렸던 점 / 문제 해결:

- "서버 = 백엔드"라는 단순화된 개념이 아님 → **서버는 클라이언트 요청에 응답하는 시스템**
- CSR/SSR/SSG 구분이 처음엔 헷갈렸으나, 렌더링 시점(브라우저 vs 서버 vs 빌드 시점)으로 이해하면 명확해짐
- Turbopack은 Next.js 팀에서 만든 차세대 번들러지만 아직 기능 미흡 → 실무에선 Webpack 사용 권장

### 💡 느낀 점 / 배운 점:

- Next.js는 "리액트만 쓰던 시절"보다 훨씬 더 **웹 개발 전체를 포괄**하는 프레임워크임을 체감
- SSR 기반의 프리렌더링이 SEO와 초기 로딩에 유리한 점이 큰 장점
- 번들러(Webpack, Turbopack) 이해는 Next.js 내부 동작 원리를 파악하는 데 도움 됨

### 🏷️ 키워드 (태그):

`#Nextjs` `#프레임워크`  `#제어역전` `#CSR` `#SSR` `#프리렌더링` `#Webpack` `#Turbopack` `#React`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-08-21 | Next.js 핵심 개념 | React 기반 풀스택 프레임워크, 프리렌더링(SSR/SEO), Webpack vs Turbopack | `create-next-app`, `npm run dev` 실행, 번들러 비교 | 서버 개념, CSR/SSR 구분, Turbopack 한계 이해 | `#Nextjs` `#프레임워크`  `#제어역전` `#CSR` `#SSR` `#프리렌더링` `#Webpack` `#Turbopack` `#React` | ✅ |
