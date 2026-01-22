# Provider 주입 패턴

### 📅 날짜:

> 2025.09.04 (목)
> 

### 📘 오늘 공부한 주제:

> Provider 주입 패턴, ThemeProvider를 통한 다크모드 구현
> 

---

## 📝 핵심 개념 요약

- **Provider 주입 패턴**: 클라이언트 컴포넌트로 Provider를 작성하고 RootLayout에서 children을 감싸 앱 전역에 컨텍스트를 전달하는 방식.
- **ThemeProvider**: Context API를 활용하여 다크모드/라이트모드 상태 관리 및 토글 제공.
- **서버-클라이언트 컴포넌트 컴포지션 패턴**: RootLayout(서버 컴포넌트) → Providers(클라이언트 컴포넌트) → children 전달 구조.
- **Tailwind 다크모드 지원**: `dark:` prefix를 통해 다크모드일 때의 스타일을 정의.

## 📊 핵심 요약 표

| 개념 | 설명 | 적용 포인트 |
| --- | --- | --- |
| Provider 주입 패턴 | RootLayout에서 Providers로 children 감싸 전역 컨텍스트 제공 | 앱 전체에서 동일한 상태 관리 |
| ThemeProvider | 다크/라이트 모드 전환 상태 관리 (Context API 활용) | `useTheme` 커스텀 훅으로 접근 |
| 다크모드 전환 | localStorage 저장 + `document.documentElement.classList`로 HTML에 클래스 추가/제거 | Tailwind `dark:` prefix와 함께 사용 |
| UI 적용 | Header에 다크모드 토글 버튼 추가 | 아이콘 전환 + 접근성(aria-label) 고려 |

### 💻 실습 내용 정리

- **Provider 생성 (`app/providers.jsx`)**
    
    ```jsx
    "use client";
    import ThemeProvider from "@/providers/ThemeProvider";
    
    export default function Providers({ children }) {
      return <ThemeProvider>{children}</ThemeProvider>;
    }
    ```
    
- **ThemeProvider 생성 (`src/providers/ThemeProvider.jsx`)**
    - Context API + localStorage 저장
    - `toggleTheme`, `setTheme`, `isDarkMode` 제공
    - `useEffect`로 HTML 태그에 `dark` 클래스 추가/제거
- **tailwind.config.js 수정**
    
    ```jsx
    module.exports = {
      darkMode: "class",
      // ...
    }
    ```
    
- **Header에 다크모드 버튼 추가 (`Header.jsx`)**
    
    ```jsx
    const { isDarkMode, toggleTheme } = useTheme();
    
    <buttononClick={toggleTheme}
      className="p-2 rounded-full"
      aria-label={isDarkMode ? "라이트 모드로 전환" : "다크 모드로 전환"}
    >
      {isDarkMode ? <SunIcon /> : <MoonIcon />}
    </button>
    ```
    
- **Header에 다크모드 배경/텍스트 적용**
    
    ```jsx
    <header className="bg-white text-black dark:bg-gray-900 dark:text-white">
      ...
    </header>
    ```
    

### ❗ 헷갈렸던 점 / 문제 해결:

- **서버 컴포넌트에서 Context를 직접 주입할 수 없는 이유** → `'use client'` 지시어가 필요했음.
- Tailwind `darkMode: "class"` 설정을 하지 않으면 `dark:` prefix가 작동하지 않는 문제를 확인하고 해결.

### 💡 느낀 점 / 배운 점:

- Next.js의 **서버-클라이언트 컴포넌트 조합** 구조를 이해하게 됨.
- Provider 패턴을 적용하면 전역 상태 관리가 깔끔해지고, **테마 변경 같은 기능을 전역적으로 쉽게 적용**할 수 있음.
- 접근성을 고려한 버튼(aria-label)과 Tailwind의 `dark:` 유틸리티가 조합되니 훨씬 실무적이라는 걸 체감함.

### 🏷️ 키워드 (태그):

`Next.js` `Provider` `ThemeProvider` `Context API` `다크모드` `localStorage` `TailwindCSS` `use client`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-09-04 | Provider & 다크모드 구현 | Provider로 전역 상태 관리, ThemeProvider로 다크/라이트 모드 전환, Tailwind `dark:` 적용 | Provider 주입, ThemeProvider 생성, 다크모드 토글 버튼 & 스타일 적용 | 서버-클라이언트 조합 이해, Tailwind `dark:` prefix 문제 해결 | `#Next.js` `#Provider` `#ThemeProvider` `#ContextAPI` `#DarkMode` `#Tailwind` | ✅ |
