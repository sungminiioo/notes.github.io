# Tailwind CSS 핵심 정리

### 📅 날짜:

> 2025.08.22 (금)
> 

### 📘 오늘 공부한 주제:

> Tailwind CSS 기본 개념, Zero-runtime 스타일링, Reset & Custom 기능, 주요 유틸리티 클래스, 컴포넌트 예시
> 

---

## 📝 핵심 개념 요약

- **Tailwind CSS**: 유틸리티 클래스 기반 CSS 프레임워크 → 빠르고 유연한 스타일링 가능
- **Zero-runtime 스타일시트**: 런타임에 CSS를 생성하지 않고, 빌드 단계에서 미리 최적화
- **비교: styled-components**
    - 런타임에 CSS를 생성해 DOM에 삽입 → 초기 로딩 느려질 수 있음
    - Tailwind는 빌드 시점에 PurgeCSS로 불필요한 CSS 제거 → 빠르고 가볍다
- **PostCSS & PurgeCSS**
    - PostCSS: Tailwind 유틸리티 변환기
    - PurgeCSS: 쓰이지 않는 CSS 제거 (최종 CSS 파일 최소화)
- **Reset & Custom 기능**: Next.js + Tailwind 프로젝트는 기본적으로 reset.css 포함, `globals.css`에서 커스텀 설정 가능
- **주요 유틸리티 클래스**: 텍스트, 색상, 레이아웃, 컴포넌트(버튼/카드) 등 실습

## 📊 핵심 요약 표

| 구분 | 설명 | 예시/비고 |
| --- | --- | --- |
| Tailwind CSS | 유틸리티 클래스 기반 프레임워크 | `className="text-center"` |
| Zero-runtime | 실행 중이 아닌 빌드 단계에서 CSS 생성 | 속도 ↑, 용량 ↓ |
| Styled-components | 런타임에 CSS 생성 → 무거움 | 실시간 <style> 삽입 |
| PostCSS | CSS 자동 가공 도구 | Tailwind 유틸리티 처리 |
| PurgeCSS | 불필요한 CSS 제거 | 최종 파일 용량 최소화 |
| Reset CSS | 기본적으로 포함 | globals.css |
| 커스텀 기능 | 테마, 유틸리티 확장 가능 | `@theme`, `@utility` |
| 실습 | 텍스트/색상/레이아웃/컴포넌트 | 버튼, 카드 예제 |

### 💻 실습 내용 정리

1. **Tailwind 기본 클래스 적용**

```jsx
<h1 className="text-3xl font-bold text-blue-500">테일윈드 CSS 기본 클래스</h1>
<p className="text-gray-600 mt-2">웹 개발에 자주 사용되는 클래스 모음</p>
```

1. **텍스트 스타일링**
- `text-xs`, `text-lg`, `text-2xl`, `font-bold`, `italic`, `underline`
1. **색상**
- `bg-red-500`, `bg-blue-500`, `text-green-500`
1. **레이아웃**
- 여백: `p-4`, `m-4`, `px-8 py-2`
- 플렉스박스: `flex`, `justify-between`, `items-center`
- 그리드: `grid-cols-3`, `col-span-2`
1. **컴포넌트**
- 버튼: 기본, 외곽선, 호버 효과
- 카드: 이미지 + 제목 + 설명 + 버튼

### ❗ 헷갈렸던 점 / 문제 해결:

- styled-components와 Tailwind 차이: "런타임 vs 빌드타임" 이해가 핵심
- reset.css를 따로 추가하지 않아도 Next.js + Tailwind 보일러플레이트에 이미 포함됨
- PurgeCSS가 자동으로 쓰지 않는 클래스 제거해준다는 점 덕분에 CSS 최적화가 자동으로 된다는 사실

### 💡 느낀 점 / 배운 점:

- Tailwind CSS는 **빠르고 직관적인 개발**에 최적화 → 생산성이 크게 올라감
- `zero-runtime` 개념 덕분에 styled-components 대비 성능상 이점 명확
- reset, 커스텀 테마, 유틸리티 확장 기능을 활용하면 디자인 시스템을 쉽게 구축 가능

### 🏷️ 키워드 (태그):

`#TailwindCSS` `#ZeroRuntime` `#StyledComponents` `#PostCSS` `#PurgeCSS` `#유틸리티CSS` `#Nextjs스타일링`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-08-22 | Tailwind CSS 스타일링 | 유틸리티 클래스 기반, Zero-runtime, PurgeCSS 최적화 | 텍스트, 색상, 레이아웃, 버튼/카드 컴포넌트 실습 | 런타임 vs 빌드타임 이해, reset 자동 적용, CSS 최적화 방식 이해 | `#TailwindCSS` `#ZeroRuntime` `#PostCSS` `#PurgeCSS` | ✅ |
