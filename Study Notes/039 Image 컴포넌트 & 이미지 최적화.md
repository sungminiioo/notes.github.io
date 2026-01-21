# Image 컴포넌트 & 이미지 최적화

### 📅 날짜:

> 2025.08.29 (금)
> 

### 📘 오늘 공부한 주제:

> 이미지 최적화 동작 원리
> 

---

## 📝 핵심 개념 요약

- **Image 컴포넌트는 `<img>`의 대체재**로, 자동 이미지 최적화를 제공
- 서버에서 리사이징·포맷 변환(WebP 등)·캐싱 수행 → 클라이언트에 최적화된 이미지 전달
- **필수 속성**: `src`, `width`, `height`, `alt`
- **fill 속성**: 부모 요소 크기에 맞춰 이미지 자동 조정 (`relative` 필요, `object-cover`와 조합)
- **placeholder="blur"**: 이미지 로딩 UX 개선, 정적 이미지는 자동 blur / 외부 이미지는 `blurDataURL` 필요
- **plaiceholder**: 외부 이미지 blur 데이터 생성 도구

## 📊 핵심 요약 표

| 개념 | 설명 | 예시 |
| --- | --- | --- |
| **최적화 원리** | 서버에서 리사이징·포맷 변환·캐싱 처리 | WebP 변환, CDN 캐싱 |
| **필수 속성** | `src`, `width`, `height`, `alt` | `<Image src="/img.png" width={500} height={500} alt="..." />` |
| **fill** | 부모 크기에 맞게 자동 조정 | `fill` + `relative` + `object-cover` |
| **placeholder** | 로딩 중 blur 이미지 제공 | `placeholder="blur"` |
| **plaiceholder** | 외부 이미지 blurDataURL 생성 | `getPlaiceholder` 활용 |

### 💻 실습 내용 정리

1. **기본 사용법**

```jsx
import Image from 'next/image';

export default function Page() {
  return (
    <div>
      <Imagesrc="/profile.png"
        width={500}
        height={500}
        alt="Picture of the author"
      />
    </div>
  );
}
```

1. **fill 속성 활용**

```jsx
<figure className="relative w-full h-64">
  <Imagesrc="/example.jpg"
    alt="풍경 사진"
    fill
    className="object-cover"
  />
</figure>
```

1. **외부 이미지 사용 (next.config.mjs 설정)**

```jsx
const nextConfig = {
  images: {
    remotePatterns: [
      {
        protocol: 'https',
        hostname: 'example.com',
        pathname: '/account123/**',
      },
    ],
  },
};
```

1. **blur placeholder (정적 & 외부)**

```jsx
import profilePic from '../public/profile.jpg';

function Profile() {
  return (
    <Imagesrc={profilePic}
      placeholder="blur"
      alt="프로필 사진"
      width={500}
      height={500}
    />
  );
}
```

1. **plaiceholder 활용 (외부 이미지 blur 처리)**

```jsx
import { getPlaiceholder } from 'plaiceholder';
import Image from 'next/image';

async function MyImageComponent() {
  const imageUrl = 'https://example.com/profile.jpg';
  const response = await fetch(imageUrl);
  const buffer = await response.arrayBuffer();
  const { base64 } = await getPlaiceholder(Buffer.from(buffer));

  return (
    <Imagesrc={imageUrl}
      alt="프로필 이미지"
      width={500}
      height={300}
      placeholder="blur"
      blurDataURL={base64}
    />
  );
}
```

### ❗ 헷갈렸던 점 / 문제 해결:

- 단순히 `<img>` 태그 대신 `<Image>`를 쓰는 게 아니라, **Next.js 서버를 통해 이미지 최적화 과정이 있다는 점**을 이해하는 것이 중요했음.
- 외부 이미지를 사용할 땐 반드시 `next.config.mjs`에 `remotePatterns` 설정을 추가해야 동작.
- `fill` 사용 시 부모 요소에 `relative` 필수 → 안 하면 이미지 표시 안 됨.

### 💡 느낀 점 / 배운 점:

- **Next.js는 이미지 관리까지 프레임워크 차원에서 지원**한다는 점이 강력하다고 느낌.
- `plaiceholder`를 통해 UX를 높이는 방법을 배움 → 실무에서도 바로 적용 가능할 듯.
- 단순한 속성들이지만, 적절히 조합하면 반응형·최적화·UX 개선을 한 번에 해결 가능.

### 🏷️ 키워드 (태그):

`#Nextjs` `#Image` `#이미지최적화` `#fill` `#placeholder` `#plaiceholder` `#remotePatterns`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-08-29 | Next.js Image 컴포넌트 & 최적화 | `<Image>`는 최적화된 이미지 제공 (리사이징·포맷 변환·캐싱). 필수 속성(`src`, `width`, `height`, `alt`) + `fill`, `placeholder` 활용 | 기본 사용법, fill 속성, remotePatterns 설정, blur 처리, plaiceholder | 외부 이미지 remotePatterns 필요, fill 사용 시 relative 필수, 최적화 원리 이해 | `#Nextjs` `#Image` `#이미지최적화` `#fill` `#placeholder` `#plaiceholder` | ✅ |
