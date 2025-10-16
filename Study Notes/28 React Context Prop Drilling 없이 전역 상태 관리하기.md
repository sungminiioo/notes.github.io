# React Context Prop Drilling 없이 전역 상태 관리하기

### 📅 날짜:

> 2025.08.13 (수)
> 

### 📘 오늘 공부한 주제:

> createContext, Provider, useContext 사용법
> 

---

## 📝 핵심 개념 요약

- **Context**: 컴포넌트 트리 전반에 걸쳐 데이터를 공유할 수 있는 기능
- **Prop Drilling**: 상위 → 하위 컴포넌트로 불필요하게 계속 prop을 전달하는 현상
- **해결 방법**: Context Provider로 범위를 지정하고 value로 데이터 전달
- **State와 함께 사용**: Provider에서 상태를 만들어 value에 포함
- **코드 분리**: Context 생성과 Provider 로직을 별도 파일로 분리해 유지보수성 향상

## 📊 핵심 요약 표

| 개념 | 설명 | 예시 |
| --- | --- | --- |
| Context | 여러 컴포넌트에서 공통 데이터 공유 | 다국어 설정, 사용자 정보 |
| Prop Drilling | 여러 단계 거쳐 props 전달 | App → Header → Nav → Button |
| Provider | Context 값 공유 범위 설정 | `<MyContext.Provider value={...}>` |
| useContext | Context 값 가져오기 | `const data = useContext(MyContext)` |
| State + Context | 상태를 전역에서 관리 | `value={{state, setState}}` |
| 코드 분리 | context.js, provider.js로 분리 | 유지보수 편리 |

### 💻 실습 내용 정리

---

**1. Context 생성**

```jsx
import { createContext } from 'react';
const LocaleContext = createContext('ko');
export default LocaleContext;
```

**2. Provider로 값 전달**

```jsx
<LocaleContext.Provider value="en">
  <ChildComponent />
</LocaleContext.Provider>
```

**3. useContext로 값 사용**

```jsx
import { useContext } from 'react';
import LocaleContext from './LocaleContext';

function Board() {
  const locale = useContext(LocaleContext);
  return <div>언어: {locale}</div>;
}
```

**4. State와 함께 Context 사용**

```jsx
import { createContext, useState, useContext } from 'react';

const LocaleContext = createContext({});

export function LocaleProvider({ children }) {
  const [locale, setLocale] = useState('ko');
  return (
    <LocaleContext.Provider value={{ locale, setLocale }}>
      {children}
    </LocaleContext.Provider>
  );
}

export function useLocale() {
  return useContext(LocaleContext);
}
```

### ❗ 헷갈렸던 점 / 문제 해결:

- **헷갈렸던 점**: Provider 안에서만 useContext로 값 접근 가능하다는 점을 처음에 놓침
- **해결 방법**: Provider 범위를 컴포넌트 트리 최상단(App.jsx)로 확장하여 해결

### 💡 느낀 점 / 배운 점:

- Context는 잘 쓰면 전역 상태 관리가 깔끔해지지만, 남용 시 코드 추적이 어려워질 수 있음
- Provider 범위를 최소화하고, 필요한 데이터만 공유하는 게 중요함
- Custom Hook과 함께 사용하면 재사용성과 안전성이 높아짐

### 🏷️ 키워드 (태그):

`#React` `#Context` `#PropDrilling` `#Provider` `#useContext` 

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-08-13 | React Context | Prop Drilling 없이 전역 상태 공유 가능. Provider로 범위 설정, useContext로 값 사용. State와 함께 사용 시 관리 용이 | Context 생성 → Provider 값 전달 → useContext로 접근 → State와 함께 사용 | Provider 범위 설정을 잊어 값이 안 보였으나 App.jsx 최상단에서 감싸 해결. 남용 시 추적 어려움 주의 |  `#React`  `#Context` `#PropDrilling` `#Provider` `#useContext`  | ✅ |
