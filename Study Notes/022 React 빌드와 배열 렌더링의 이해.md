# React 빌드와 배열 렌더링의 이해

### 📅 날짜:

> 2025.08.04 (월)
> 

### 📘 오늘 공부한 주제:

> React 빌드 및 배포, Babel & Webpack 원리, 배열 렌더링(map/sort/filter), key의 역할, VSCode 단축키 정리
> 

---

### 📝 핵심 개념 요약:

- `npm run build`로 최종 번들 파일 생성, `npm serve build`로 로컬 서버 제공
- AWS S3 버킷으로 정적 웹 배포 가능
- JSX는 Babel이 JS로 변환, Webpack이 파일 묶음 → 브라우저가 실행
- map/filter/sort는 배열을 렌더링하거나 조작할 때 사용
- key는 React의 리스트 최적화를 위한 고유 식별자

### 📌 핵심 요약 표

| 개념 | 설명 |
| --- | --- |
| `npm run build` | React 프로젝트를 배포 가능한 상태로 번들링 |
| S3 배포 | AWS S3 버킷 설정 후 정적 파일 업로드 |
| Babel | JSX를 JavaScript로 변환 |
| Webpack / Vite | JS, CSS, 이미지 등을 하나의 번들로 패키징 |
| `map()` | 배열 요소를 JSX로 렌더링 |
| `sort()` | 배열을 정렬 (숫자/문자열) |
| `filter()` | 조건에 맞는 배열 요소만 추출 |
| `key` | 리스트 요소 식별을 위한 고유 값 (성능 최적화) |

### 💻 실습 내용 정리

- `npm run build` 실행 후 `build/` 폴더 생성 확인
- `npm install -g serve` 후 `serve -s build`로 정적 파일 제공
- AWS S3 접속 → 버킷 생성 → 정적 호스팅 설정
- 아래 예제 코드로 배열 렌더링 및 정렬/삭제 기능 구현:

```jsx
const items = ["apple", "banana", "cherry"];
const sortedItems = items.sort((a, b) => a.localeCompare(b));
const filteredItems = sortedItems.filter(item => item !== "banana");

<ul>
  {filteredItems.map((item, index) => (
    <li key={index}>{item}</li>
  ))}
</ul>
```

### ❗ 헷갈렸던 점 / 문제 해결:

- JSX로 작성된 코드가 브라우저에서 바로 실행되지 않는 이유 → Babel & Webpack 작동 원리 학습 후 이해
- S3 퍼블릭 엑세스 설정 시 경고 메시지 → 설정을 정확히 보고 넘어가야 함

### 💡 느낀 점 / 배운 점:

- React 앱을 실제로 배포하며 전체 구조를 이해하게 됨
- 배열 렌더링은 실무에서도 빈번히 사용되며, `key` 설정은 필수
- 빌드와 배포를 한 번 해보면 이후 작업이 크게 어렵지 않음

### 🏷️ 키워드 (태그):

`#React` `#배열렌더링` `#map` `#sort` `#filter` `#build` `#S3배포` `#JSX` `#Webpack` `#key`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-08-04 | React 빌드 & 배열 렌더링 | 빌드/배포, JSX 변환, 배열 렌더링 기초 | `npm run build`, S3 배포, map/filter/sort 실 | Babel, S3 권한 설정 이해 |  `#React` `#build` `#map` `#AWS` `#Filter` `#Key`  | ✅ |
