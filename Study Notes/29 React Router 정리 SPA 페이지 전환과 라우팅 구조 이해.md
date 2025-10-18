# React Router 정리 SPA 페이지 전환과 라우팅 구조 이해

### 📅 날짜:

> 2025.08.14 (목)
> 

### 📘 오늘 공부한 주제:

> React Router 개념과 필요성
> 

---

## 📝 핵심 개념 요약

- **SPA(Single Page Application)**: 페이지 새로 고침 없이 화면 전환
- **React Router**: URL 경로에 따라 다른 컴포넌트 렌더링
- **장점**: 빠른 페이지 전환, 코드 분리, 동적 라우팅 지원
- **단점**: 초기 로딩 느림, SEO 불리, 서버 설정 필요할 수 있음
- **주요 컴포넌트**
    - `BrowserRouter`/`Router`: 최상위 라우팅 기능 제공
    - `Routes`: 여러 Route 그룹화
    - `Route`: 경로(path)와 컴포넌트(element) 연결
    - `Link`: SPA 방식 페이지 이동, `<a>` 대체
    - `NavLink`: 현재 경로 기반 스타일 적용, 네비게이션 활성화 표시
    - `Outlet`: 상위 레이아웃에서 하위 페이지 렌더링 위치 지정

## 📊 핵심 요약 표

| 개념 | 설명 | 예시/사용법 |
| --- | --- | --- |
| SPA | 새로고침 없이 페이지 전환 | React 앱 전체 |
| Router | 라우팅 최상위 컴포넌트 | `<BrowserRouter>...</BrowserRouter>` |
| Routes | Route 묶음 관리 | `<Routes><Route path="/" element={<Home />} /></Routes>` |
| Route | 경로와 컴포넌트 연결 | `path="/about" element={<About />}` |
| Link | 페이지 이동, 새로고침 없음 | `<Link to="/about">About</Link>` |
| NavLink | Link + 현재 경로 스타일 적용 | `<NavLink to="/about" className={({isActive})=>isActive?'active':''}>` |
| Outlet | 상위 레이아웃에서 하위 페이지 렌더링 | `<Header /><Outlet /><Footer />` |

### 💻 실습 내용 정리

**1. Routes와 Route로 페이지 분리**

```jsx
import { BrowserRouter, Routes, Route } from 'react-router-dom';
import Home from './Home';
import About from './About';

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </BrowserRouter>
  );
}
```

**2. Link로 SPA 페이지 이동**

```jsx
import { Link } from 'react-router-dom';

<Link to="/about">About 페이지 이동</Link>
```

**3. NavLink로 활성화 메뉴 표시**

```jsx
import { NavLink } from 'react-router-dom';

<NavLinkto="/about"
  className={({ isActive }) => isActive ? 'active' : ''}
>
  About
</NavLink>
```

**4. 하위 페이지와 Outlet 활용**

```jsx
import { Outlet, Link } from 'react-router-dom';

function DashboardLayout() {
  return (
    <div>
      <nav>
        <Link to="stats">Stats</Link>
        <Link to="settings">Settings</Link>
      </nav>
      <Outlet /> {/* 하위 페이지 렌더링 */}
    </div>
  );
}

// Routes 구성
<Routes>
  <Route path="/dashboard" element={<DashboardLayout />}>
    <Route path="stats" element={<Stats />} />
    <Route path="settings" element={<Settings />} />
  </Route>
</Routes>
```

### ❗ 헷갈렸던 점 / 문제 해결:

- **헷갈렸던 점**: Link와 NavLink 차이, Outlet 역할
- **해결 방법**: NavLink는 isActive 기반 스타일 적용, Outlet은 하위 페이지 렌더링 위치임을 실습으로 확인

### 💡 느낀 점 / 배운 점:

- React Router는 SPA에서 효율적인 페이지 전환과 코드 분리를 도와줌
- 하위 페이지 구조와 공통 레이아웃을 Outlet으로 쉽게 관리 가능
- NavLink 활용으로 UX 향상 가능

### 🏷️ 키워드 (태그):

`#React` `#ReactRouter` `#SPA` `#Routes` `#Route` `#Link` `#Outlet` 

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-08-14 | React Router | SPA에서 URL 경로 기반 컴포넌트 렌더링. Routes/Route/Link/NavLink/Outlet 활용 | Routes+Route 구성, Link/Navigate 이동, NavLink 스타일링, Outlet으로 하위 페이지 렌더링 | Link와 NavLink 차이, Outlet 역할 이해. 공통 레이아웃 관리 쉬움 |  `#React` `#ReactRouter` `#SPA` `#Routes` `#Route` `#Link` `#Outlet`  | ✅ |
