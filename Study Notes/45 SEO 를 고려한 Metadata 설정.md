# SEO 를 고려한 Metadata 설정

### 📅 날짜:

> 2025.09.08 (월)
> 

### 📘 오늘 공부한 주제:

> Next.js에서 SEO를 고려한 Metadata 설정 방법
> 

---

## 📝 핵심 개념 요약

- **정적 Metadata 설정**
- 컴포넌트 내에서 `export const metadata = { ... }` 로 정의
- title, description 등 간단한 SEO 정보 설정 가능
- 모든 페이지 렌더링 시 동일하게 적용
- **동적 Metadata 설정**
    - `generateMetadata` 함수를 사용하여 route params, searchParams, 외부 API 데이터 기반으로 동적으로 설정 가능
    - 기존 부모 metadata를 가져와서 확장 가능 (overwrite 아님)
    - 예: Open Graph 이미지 추가, 페이지별 고유 title 적용

## 📊 핵심 요약 표

| 구분 | 정의 | 사용 시점 | 특징 | 예시 |
| --- | --- | --- | --- | --- |
| 정적 Metadata | `metadata` 객체로 직접 설정 | 페이지 렌더링 전 | 간단, 모든 사용자 동일 | `title: 'Home', description: 'Welcome'` |
| 동적 Metadata | `generateMetadata` 함수 사용 | 페이지 렌더링 전, async 가능 | params/API 기반, 부모 metadata 확장 가능 | `title: product.title, openGraph.images: [...]` |

### 💻 실습 내용 정리

- **정적 Metadata 설정**

```jsx
export const metadata = {
  title: 'My Page',
  description: 'This is my page description.',
};

export default function Page() {
  return <div>Page Content</div>;
}
```

- **동적 Metadata 설정**

```jsx
export async function generateMetadata({ params, searchParams }, parent) {
  const { id } = params;
  const product = await fetch(`https://api.example.com/products/${id}`)
    .then(res => res.json());

  const previousImages = (await parent).openGraph?.images || [];

  return {
    title: product.title,
    openGraph: {
      images: ['/specific-image.jpg', ...previousImages],
    },
  };
}

export default function ProductPage({ params, searchParams }) {
  return <div>Product ID: {params.id}</div>;
}
```

### ❗ 헷갈렸던 점 / 문제 해결:

- ❌ 동적 Metadata에서 부모 metadata를 덮어쓰면 기존 Open Graph 설정이 사라짐
- ✅ 해결: `parent`를 활용하여 기존 metadata를 확장하는 방식으로 유지

### 💡 느낀 점 / 배운 점:

- SEO 최적화를 위해 페이지별 고유 title, description, OG 이미지 설정은 필수
- 정적과 동적 metadata를 상황에 맞게 구분하여 사용하면 성능과 SEO를 동시에 잡을 수 있음

### 🏷️ 키워드 (태그):

`#Nextjs` `#SEO` `#Metadata` `#generateMetadata` `#OpenGraph` `#Static` `#Dynamic`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-09-08 | SEO & Metadata | 정적: metadata 객체
동적:generate
Metadata 함수 | 정적/동적 Metadata 코드 작성 | 부모 metadata 확장 vs 덮어쓰기 이해 | `#Nextjs` `#SEO` `#Metadata` `#OpenGraph` | ✅ |
