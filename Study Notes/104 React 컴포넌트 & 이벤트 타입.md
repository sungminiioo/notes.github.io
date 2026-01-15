# React 컴포넌트 & 이벤트 타입

### 📅 날짜:

> 2025.12.03 (수)
> 

### 📘 오늘 공부한 주제:

> React 컴포넌트 props 타입 적용, 이벤트 핸들러 타입 정의 및 제네릭 활용
> 

---

## 📝 핵심 개념 요약

- **Props 타입 정의:** `interface` 사용, 선택적 props는 `?`, 기본값 설정 권장
- **React.FC 사용 지양:** children 자동 포함, defaultProps/displayName 문제, 제네릭/타입 추론 제한
- **Children 타입:** `React.ReactNode` 사용, 필요시 `React.ReactElement` 또는 `string` 가능
- **이벤트 타입:** React SyntheticEvent 기반, 각 이벤트별 구체적 타입 제공
- **e.currentTarget vs e.target:** 타입 안전성 때문에 `currentTarget` 사용 권장
- **데이터 타입 vs Props 타입:** 데이터/상태는 `type`, 컴포넌트 Props는 `interface`

## 📊 핵심 요약 표

| 구분 | 설명 |
| --- | --- |
| Props 타입 | interface 사용, 선택적 props `?`, 기본값 설정 |
| Children | React.ReactNode, 명시적 정의 필요 |
| 이벤트 타입 | React.SyntheticEvent 기반, 제네릭으로 엘리먼트 타입 지정 가능 |
| 자주 쓰는 이벤트 타입 | ChangeEvent, FormEvent, MouseEvent, KeyboardEvent, FocusEvent 등 |
| currentTarget vs target | currentTarget: 타입 안정성 보장, target: 타입 좁히기 필요 |
| 데이터 타입 | type alias 사용 (비즈니스 로직 관련) |

### 💻 실습 내용 정리

### [1] 컴포넌트 Props 타입 적용

```tsx
// 데이터 타입 (type alias)
type Post = {
  id: number;
  title: string;
  content: string;
  authorId: number;
};

// Props 타입 (interface)
interface PostFormProps {
  initialData?: Partial<Post>; // Partial 활용
  onSubmit: (data: Post) => void;
}

// 컴포넌트
function PostForm({ initialData, onSubmit }: PostFormProps) {
  // state 및 JSX 구현
}
```

- `src/types/index.ts` 에 Post 타입 정의 후 import
- Button, Input, Select 등 UI 컴포넌트 props도 interface로 정의, onClick 등 이벤트는 선택적 매개변수 사용

---

### [2] 이벤트 타입 적용

```tsx
interface SearchInputFormProps {
  onSearch: (query: string) => void;
  onFocus?: () => void;
  onBlur?: () => void;
}

function SearchInputForm({ onSearch, onFocus, onBlur }: SearchInputFormProps) {
  const handleSubmit: React.FormEventHandler<HTMLFormElement> = (e) => {
    e.preventDefault();
    onSearch('검색어');
  };

  const handleInputChange: React.ChangeEventHandler<HTMLInputElement> = (e) => {
    console.log(e.currentTarget.value);
  };
}
```

- SyntheticEvent 기반, 제네릭으로 이벤트 발생 엘리먼트 타입 지정
- 재사용 가능한 이벤트 핸들러 타입 정의 가능

```tsx
type ButtonClickHandler = React.MouseEventHandler<HTMLButtonElement>;
type InputChangeHandler = React.ChangeEventHandler<HTMLInputElement>;
type FormSubmitHandler = React.FormEventHandler<HTMLFormElement>
```

- currentTarget 사용으로 타입 안전성 확보
- Native DOM target 사용 시 타입 가드 필요

---

### [3] 제네릭 이벤트 핸들러 예제

```tsx
function createInputHandler<T extends HTMLInputElement | HTMLTextAreaElement>(
  setter: (value: string) => void
) {
  return (e: React.ChangeEvent<T>) => {
    setter(e.currentTarget.value);
  };
}
```

- 입력과 텍스트 영역 등 다양한 엘리먼트에서 재사용 가능
- TypeScript 추론과 안전성 강화

---

### [4] 실습 안내

```bash
git checkout 2_event
git reset --hard origin/2_event
```

- `src/components/PostForm.tsx` 의 4개의 이벤트 핸들러 함수 각각의 event 타입 정의
- `ChangeEvent<HTMLInputElement>` / `FormEvent<HTMLFormElement>` / `MouseEvent<HTMLButtonElement>` 등 적용

### ❗ 헷갈렸던 점 / 문제 해결:

| 문제 | 해결 |
| --- | --- |
| React.FC 사용 시 children 자동 포함 | 함수 선언 + interface 방식으로 명시적 children 정의 |
| 이벤트 타입 지정 어려움 | SyntheticEvent 제네릭 활용, currentTarget vs target 이해 |
| Partial 사용법 | initialData와 같은 선택적 객체 타입에 Partial 활용 |
| input, textarea 재사용 이벤트 핸들러 | 제네릭 함수 createInputHandler로 재사용 가능 |

### 💡 느낀 점 / 배운 점:

- interface와 type의 용도 차이를 명확히 구분할 수 있음
- SyntheticEvent와 제네릭 기반 타입 지정으로 이벤트 타입 안전성을 확보
- currentTarget 사용 시 불필요한 타입 가드 없이 안전하게 이벤트 처리 가능
- Partial, 제네릭, 명시적 Props 정의 등 실무에서 자주 쓰이는 TypeScript 패턴 학습

### 🏷️ 키워드 (태그):

`#React` `#TypeScript` `#컴포넌트` `#Props` `#Event` `#SyntheticEvent` `#currentTarget` `#Partial` `#제네릭` `#타입안전성`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-12-03 | React 컴포넌트 & 이벤트 타입 | Props interface, 이벤트 SyntheticEvent 타입, currentTarget 안전성, Partial & 제네릭 활용 | 컴포넌트 Props 타입 지정, 이벤트 타입 정의, createInputHandler 제네릭 핸들러, PostForm 이벤트 타입 적용 | React.FC 사용 지양, 이벤트 타입 적용, currentTarget vs target 이해, 재사용 핸들러 구현 | `React` `TypeScript` `Props, Event` `SyntheticEvent` `currentTarget` `Partial` `제네릭` `타입안전성` | ✅ |
