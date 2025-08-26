# 📘 React vs Next.js — Next.js를 사용하는 이유

### 📝 핵심 요약

React는 **UI 라이브러리**라서 클라이언트 측 렌더링(CSR)에 집중하는 반면,

Next.js는 React 기반의 **프레임워크**로서 **SSR, SSG, 라우팅, 최적화 기능**을 제공해 **SEO와 성능**을 크게 향상시킵니다.

---

## 1. 렌더링 방식 지원 (CSR vs SSR/SSG/ISR)

- **React 단독**: 기본적으로 CSR(Client-Side Rendering)만 가능 → SEO 불리, 초기 로딩 느림
- **Next.js**:
    - **SSR (Server-Side Rendering)**: 서버에서 HTML 생성 → SEO와 초기 로딩 속도 향상
    - **SSG (Static Site Generation)**: 빌드 시 정적 페이지 생성 → 빠르고 안전
    - **ISR (Incremental Static Regeneration)**: 정적 페이지도 일정 주기로 갱신 가능

👉 다양한 렌더링 전략을 선택할 수 있어 **유연성**이 큼

---

## 2. SEO (검색 엔진 최적화)

- **React 단독**: 초기 로딩 시 HTML이 비어 있음 → 검색엔진 크롤러가 콘텐츠 인식 어려움
- **Next.js**: 서버에서 HTML을 먼저 내려주기 때문에 **SEO 친화적**

---

## 3. 라우팅 시스템

- **React 단독**: `react-router` 같은 외부 라이브러리 필요
- **Next.js**: `pages/` 디렉토리 기반의 **파일 시스템 라우팅** 제공 → 설정 없이 라우팅 가능

---

## 4. 성능 최적화 기능 내장

- **코드 스플리팅 & 자동 최적화**: 필요 페이지별로 코드 분리
- **이미지 최적화**: `next/image` 컴포넌트 → 자동 리사이즈, lazy loading 지원
- **API Routes**: `/api` 폴더에 서버리스 API 구현 가능 → 간단한 백엔드 역할 가능

---

## 5. 개발 경험 향상

- **TypeScript 지원 내장**
- **Fast Refresh** (빠른 HMR 지원)
- **Zero Config**: 기본 설정으로 바로 시작 가능

---

## ✅ 결론

- **React만 사용**: UI 구축에는 적합하지만, SEO/라우팅/SSR/성능 최적화 기능이 부족함
- **Next.js 사용**:
    
    👉 SSR·SSG 지원, SEO 강화, 라우팅 자동화, 이미지·코드 최적화, API 라우트 등
    
    **완성도 높은 웹 애플리케이션 개발에 최적화된 프레임워크**

# 모범 답안안
