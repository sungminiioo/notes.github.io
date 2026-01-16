# React 시작

### 📅 날짜:

> 2025.07.30 (수)
> 

### 📘 오늘 공부한 주제:

> React 프로젝트 생성 및 JSX 기본 문법
> 

---

### 📝 핵심 개념 요약:

- React 프로젝트는 `create-react-app` 또는 `Vite`로 생성
- JSX는 JavaScript + XML로, HTML 유사 문법을 JS 코드에 삽입
- JSX는 반드시 닫는 태그 사용, 속성은 camelCase
- 컴포넌트는 재사용 가능한 함수형 UI 단위
- props는 부모 → 자식 전달용 데이터

### 📌 핵심 요약 표

| 구분 | 내용 |
| --- | --- |
| 프로젝트 생성 | `npm init react-app .` 또는 `npm create vite@latest . -- --template react` |
| 실행 | `npm run start` (CRA), `npm run dev` (Vite) |
| 종료 | `Ctrl + C` |
| JSX 주의점 | `class` → `className`, `for` → `htmlFor`, 태그 반드시 닫기 |
| 컴포넌트 조건 | 함수 이름 대문자, 하나의 부모 태그 반환, JSX 반환 |
| props | 부모 → 자식 전달, 자식은 읽기 전용 |

### 💻 실습 내용 정리:

- VSCode 터미널에서 CRA로 React 프로젝트 생성:

```bash
npm init react-app .
```

- 실행:
    
    ```bash
    npm run start
    ```
    
- 종료:
    
    `Ctrl + C`
    
- JSX 문법 실습:
    
    ```jsx
    function Hello({ name }) {
      return <p className="greeting">Hello, {name}!</p>;
    }
    ```
    
- props 활용 예시:
    
    ```jsx
    <Hello name="React" />
    ```
    

### ❗ 헷갈렸던 점 / 문제 해결:

- `class`가 아닌 `className` 사용
- `<label for>` → `htmlFor`로 변경
- 여러 태그 반환 시 `Fragment` 또는 `<div>`로 감싸야 에러가 나지 않음

### 💡 느낀 점 / 배운 점:

- JSX는 HTML과 비슷해 보이지만 실제로는 JavaScript임
- 작은 실수(class → className)를 방지하기 위해 에디터 설정과 린터가 중요함
- props로 컴포넌트를 더 유연하게 만들 수 있어 재사용성이 높아짐

### 🏷️ 키워드 (태그):

`#React` `#JSX` `#props` `#컴포넌트` `#Vite` `#create-react-app` `#리액트시작` `#개발환경`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-07-30 | React 프로젝트 생성 및 JSX 문법 | React 초기 설정, JSX 문법, props 및 컴포넌트 기본 개념 | CRA 및 Vite로 프로젝트 생성, JSX 예제, props 전달 | JSX 문법 오류 해결, class → className, 태그 닫기 습관화 |  `#React` `#JSX` `#Vite` `#컴포넌트` `#create-react-app`  | ✅ |
