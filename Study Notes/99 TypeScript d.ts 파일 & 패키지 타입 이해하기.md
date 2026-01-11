# TypeScript d.ts 파일 & 패키지 타입 이해하기

### 📅 날짜:

> 2025.11.26 (수)
> 

### 📘 오늘 공부한 주제:

> d.ts 파일, 패키지 타입, @types, DefinitelyTyped, namespace vs module, 전역 타입 확장
> 

---

## 📝 핵심 개념 요약

- **d.ts(Declaration File)**: 타입 선언만 포함하는 파일, JS 라이브러리의 타입 정보를 TS에 제공함
- **@types 패키지**: DT(DefinitelyTyped) 저장소에 있는 타입 정의를 npm으로 설치
- **Built-in Type 포함 패키지**: 패키지 로고에 TS 표시, 별도 타입 설치 불필요
- **DT 타입 필요한 패키지**: 별도로 `@types/패키지명` 설치
- **namespace vs module**:
    - namespace → 전역, 레거시
    - module → ES 모듈 기반, 현대적
- **d.ts 사용 위치**: 외부 라이브러리 타입 정의, 전역 타입 확장, 정적 파일 import 시 타입 선언
- **ts 파일이 더 적절한 상황**: 내부 타입, API 관련 타입, 클래스/상수 등 로직 포함 시

## 📊 핵심 요약 표

| 개념 | 설명 |
| --- | --- |
| d.ts | 타입 선언만 포함하는 파일, 구현 없음 |
| @types | DefinitelyTyped 저장소의 타입 정의 패키지 |
| Built-in 타입 패키지 | 패키지 자체에 TS 지원 내장 |
| namespace | 전역 스코프, 오래된 방식 |
| module | import/export 기반, 최신 방식 |
| d.ts 사용 예 | 외부 라이브러리, 전역 객체 확장, 이미지/정적 파일 타입 |
| d.ts 대신 ts 사용 | 서비스 내부 타입, API 타입, 클래스 로직 포함 시 |

### 💻 실습 내용 정리

### ✔ 1. lodash 타입이 없을 때 d.ts 직접 작성

```tsx
// lodash.d.ts
declare module "lodash" {
  interface Lodash {
    lowerCase: (param: string) => string;
  }
  const _: Lodash;
  export default _;
}
```

### ✔ 2. @types 패키지 설치

```bash
npm install @types/lodash
```

### ✔ 3. namespace 방식 타입 선언

```tsx
declare namespace MyLibrary {
  interface User { id: number; name: string; }
  function getUser(id: number): Promise<User>;
}
```

### ✔ 4. module 방식 타입 선언

```tsx
declare module 'modern-lib' {
  export interface User { id: number; name: string; }
  export function getUser(id: number): Promise<User>;
}
```

### ✔ 5. 전역 Window 확장 (kakao map CDN 사용 시)

```tsx
declare global {
  interface Window {
    kakao: any;
  }
}
```

### ❗ 헷갈렸던 점 / 문제 해결:

- **namespace와 module 차이가 가장 헷갈렸지만**, 핵심은 “전역 vs 모듈”이라는 큰 개념으로 정리하니 이해가 쉬워짐.
- **타입이 내장된 패키지와 아닌 패키지 구분법**(npm TS/DT 표시)을 알고 나니 라이브러리 타입 오류 해결이 훨씬 수월해짐.
- d.ts 파일은 “타입 제공용”이고 TS 파일은 “타입 + 로직”이라는 구분을 통해 실제 프로젝트 구조를 더 명확하게 잡을 수 있게 됨.

### 💡 느낀 점 / 배운 점:

- d.ts 파일은 “외부 타입을 가져오는 입구”라는 개념으로 이해하니 전체 흐름이 명확해짐.
- 실제 서비스에서 외부 JS SDK 쓸 때 전역 Window 확장이 얼마나 필수적인지 깨달음.
- @types / DT / Built-in 타입 개념을 정확히 알게 되어 타입 에러 처리 시간 줄어들 것 같음.

### 🏷️ 키워드 (태그):

`#TypeScript` `#Generics` `#MappedTypes` `#ConditionalTypes` `#전역타입확장` `#JS라이브러리타입`

`#namespace`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-11-26 | d.ts 파일과 패키지 타입 | d.ts 선언 파일 개념, @types와 DT 구조, namespace vs module 차이, 전역 타입 확장 방식 이해 | lodash.d.ts 작성, @types 설치, namespace/module 선언, window 확장 타입 작성 | namespace·module 차이가 명확해졌고 외부 JS 라이브러리 타입을 TS에 연결하는 구조 이해 완료 | `#TypeScript` `#dts` `#@types` `#DefinitelyTyped` `#namespace` `#module` `#타입확장` | ✅ |
