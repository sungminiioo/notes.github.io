# 📘 d.ts 타입 정의 파일이란?

### ✔️ 역할

- JavaScript 라이브러리나 코드에 **타입 정보를 제공**
- TypeScript 프로젝트 내에서 IntelliSense 자동완성, 타입 오류 방지
- 타입만 포함하고 실제 로직은 없음 → **컴파일 시 JS로 변환되지 않음**

### ✔️ 어디서 많이 쓰나?

- 외부 라이브러리(예: Lodash, jQuery 등) 타입 정의
    
    → DefinitelyTyped의 `@types/*` 패키지들이 바로 이것
    
- JS로 작성된 프로젝트를 TS로 점진적 전환할 때
- 직접 만든 유틸, 모듈의 타입을 분리해 관리하고 싶을 때

---

# 🧩 d.ts 파일 안에 뭐가 들어가나요?

주로 다음과 같은 선언만 포함됩니다:

- `interface`
- `type`
- `function`의 시그니처
- `declare` 키워드로 모듈/변수/클래스 정의
- `namespace` 또는 `module` 선언

✔ 예시: `math.d.ts`

```tsx
declare function add(a: number, b: number): number;

declare const VERSION: string;

interface User {
  id: number;
  name: string;
}
```

이 파일은 **타입 설명만 존재**하며 실제 구현은 없습니다.

---

# 🛠️ d.ts 파일은 어떻게 만들까?

## ✅ 1) 파일 생성하기

그냥 프로젝트 안에 파일을 만듭니다:

```
src/types/math.d.ts
```

TypeScript가 자동으로 인식합니다.

만약 인식하지 않으면 `tsconfig.json`에 포함시킬 수 있습니다:

```json
{
  "compilerOptions": {
    "typeRoots": ["./src/types", "./node_modules/@types"]
  }
}
```

---

## ✅ 2) 직접 타입 선언하기

### (1) 전역 타입 선언

```tsx
// global.d.ts
declare const API_URL: string;

interface Window {
  myCustomValue: number;
}
```

---

### (2) 모듈 타입 정의

JS 파일을 TS에서 import하고 싶을 때 사용:

```tsx
// utils.d.ts
declare module "my-utils" {
  export function sum(a: number, b: number): number;
}
```

---

### (3) JS 파일에 대해 자동 타입 생성하기

이미 만들어진 JS 파일에 타입을 붙이고 싶다면:

```jsx
// calc.js
export function add(a, b) {
  return a + b;
}
```

→ 타입 정의 파일 만들기:

```tsx
// calc.d.ts
export function add(a: number, b: number): number;
```

---

# 🟦 d.ts 파일 자동 생성하기 (tsc 사용)

TypeScript 컴파일러가 자동으로 `.d.ts` 파일을 만들어주게 할 수도 있습니다:

`tsconfig.json`:

```json
{
  "compilerOptions": {
    "declaration": true,
    "outDir": "dist"
  }
}
```

그리고 빌드하면:

```
dist/
  calc.js
  calc.d.ts ← 자동 생성됨
```

---

# 📌 정리

| 항목 | 설명 |
| --- | --- |
| **역할** | JS 코드에 타입 정보를 제공 |
| **출력 여부** | 컴파일 결과에 포함되지 않음 |
| **언제 사용?** | JS 라이브러리 사용 시, JS → TS 전환 중, 별도 타입 관리 시 |
| **어떻게 만들까?** | 직접 선언 또는 tsc로 자동 생성 |
| **핵심 문법** | `declare`, `interface`, `type`, 함수 시그니처 |

# 모범 답안

# 📘 d.ts 파일, 왜 필요할까요?

타입스크립트를 사용하다 보면 자바스크립트 라이브러리를 함께 사용하는 경우가 많아요.

하지만 자바스크립트는 타입 정보를 제공하지 않기 때문에, 타입스크립트 입장에서는

**"이 라이브러리가 어떤 함수나 변수를 제공하는지" 알 수 없어요.**

이럴 때 필요한 게 바로 **d.ts 타입 정의 파일**이에요!

---

# 📘 d.ts 파일이란?

d.ts 파일은 타입스크립트에서 **외부 자바스크립트 라이브러리나 프로젝트 코드의 타입 정보를 선언하는 파일**이에요.

타입스크립트 컴파일러는 이 파일을 참고해서

- 코드의 타입을 체크하고
- 코드 자동 완성 같은 개발자 친화적인 기능을 제공

할 수 있게 됩니다.

👉 쉽게 말해, **자바스크립트 라이브러리의 설명서를 타입스크립트 언어로 작성한 것**이라고 생각하면 돼요!

---

# 📌 d.ts 파일은 어디에 쓰일까요?

## 1️⃣ 타입 체크 지원

d.ts 파일은 타입스크립트 컴파일러가 라이브러리의 타입을 이해할 수 있게 도와줘요.

라이브러리 함수에 잘못된 인수를 전달하거나 반환 타입을 잘못 사용했을 때,

→ 컴파일러가 바로 오류를 알려줍니다.

## 2️⃣ 자동 완성 지원

타입 정의가 있으면 IDE에서 함수나 변수의 자동 완성을 제공해 줍니다.

함수에 어떤 매개변수를 전달해야 하는지, 반환값이 무엇인지 쉽게 알 수 있어요.

---

# 🛠️ 어떻게 만들까요?

## ✔ 방법 1. 직접 작성하기

예를 들어, 우리가 `mathLib`라는 라이브러리를 사용한다고 가정해볼게요.

이 라이브러리가 `add`, `multiply` 함수만 제공한다고 하면 다음처럼 작성해요:

```tsx
// mathLib.d.ts
declare module 'mathLib' {
  export function add(a: number, b: number): number;
  export function multiply(a: number, b: number): number;
}
```

이제 타입스크립트가 이 라이브러리를 완벽하게 이해할 수 있게 되었어요!

---

## ✔ 방법 2. @types 패키지 사용하기

더 쉬운 방법은 npm의 `@types` 패키지를 사용하는 거예요.

`@types`는 유명한 자바스크립트 라이브러리에 대한 타입 정의 묶음이에요.

예: lodash 사용 중이라면:

```
npm install @types/lodash
```

설치 후 lodash를 타입 오류 없이 안전하게 사용할 수 있어요.

---

# 🔎 d.ts 파일의 기본 구조

### ✔ 전역 선언 (Global Declaration)

전역에서 사용할 타입이나 변수, 함수 등을 선언할 때 사용합니다.

```tsx
declare const API_URL: string;
declare function fetchData(url: string): Promise<any>;
```

### ✔ 모듈 선언 (Module Declaration)

특정 모듈에서 사용할 타입과 함수 등을 정의합니다.

```tsx
declare module 'myLibrary' {
  export function doSomething(): void;
}
```

---

# 🧪 실습 — 직접 만들어보기

만약 `hello-world`라는 라이브러리를 사용하는데 타입 정의가 없다면, 이렇게 만들 수 있어요:

```tsx
// hello-world.d.ts
declare module 'hello-world' {
  export function sayHello(name: string): string;
  export const version: string;
}
```

이제 타입스크립트 코드에서 자동 완성과 타입 체크를 모두 지원받을 수 있어요:

```tsx
import { sayHello, version } from 'hello-world';

console.log(sayHello('Alice')); // 자동 완성 지원!
console.log(`Library version: ${version}`);
```

---

# 📝 요약

- **d.ts 파일**은 타입스크립트에서 외부 자바스크립트 라이브러리를 안전하고 편리하게 쓰기 위한 핵심 도구
- **직접 작성**하거나 **@types 패키지**로 쉽게 설치할 수 있음
- 타입 체크 & 자동 완성 → 개발 생산성 ⬆
- 타입스크립트 프로젝트에서 거의 필수 요소 😊
