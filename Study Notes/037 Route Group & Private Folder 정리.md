# Route Group & Private Folder 정리 

### 📅 날짜:

> 2025.08.27 (수)
> 

### 📘 오늘 공부한 주제:

> URL에 포함되지 않는 폴더 구조 관리 방법
> 

---

## 📝 핵심 개념 요약

- **Route Group `(폴더명)`**
    - URL path에는 포함되지 않음.
    - 하위 폴더들은 정상적으로 path에 반영됨.
    - 관심사별 페이지 묶기 & 그룹별 별도 `layout.tsx` 가능.
- **Private Folder `_(폴더명)`**
    - URL path에서 **완전히 제외됨**.
    - 특정 영역(도메인) 내에서만 사용하는 컴포넌트 정리용.

## 📊 핵심 요약 표

| 구분 | 폴더명 | URL 반영 여부 | 특징 |
| --- | --- | --- | --- |
| **Route Group** | `(폴더명)` | ❌ 포함 안 됨 | 그룹 관리 & layout 분리 가능 |
| **Private Folder** | `_(폴더명)` | ❌ 완전히 제외 | 내부 전용 컴포넌트 관리 |

### 💻 실습 내용 정리

### 1) Route Group `(폴더명)` 사용 예시

```bash
app
 ├─ (dashboard)
 │   ├─ page.jsx   →  URL: / (root)
 │   ├─ settings
 │   │   └─ page.jsx → URL: /settings

```

➡️ `(dashboard)`는 URL에 표시되지 않지만, 내부 폴더는 라우팅됨.

### 2) Private Folder `_(폴더명)` 사용 예시

```bash
app
 ├─ dashboard
 │   ├─ _components
 │   │   └─ Chart.jsx   # private 전용
 │   └─ page.jsx → URL: /dashboard

```

➡️ `_components`는 URL에 포함되지 않고, 대시보드 전용 컴포넌트 관리에만 사용됨.

### ❗ 헷갈렸던 점 / 문제 해결:

- Route Group `(폴더)`와 Private Folder `_(폴더)`의 차이 구분이 처음엔 애매했음.
- 정리한 결과 →
    - `(폴더명)` : 라우트에는 영향 안 주되, **하위 경로 라우팅 O**
    - `_(폴더명)` : 라우트에 **아예 포함 X (완전 배제)**

### 💡 느낀 점 / 배운 점:

- Next.js는 라우팅 구조를 **폴더 네이밍 규칙만으로 제어**할 수 있어서 매우 직관적.
- 관심사 별 코드 분리(Route Group)와 내부 전용 관리(Private Folder)를 활용하면 프로젝트 구조가 훨씬 깔끔해짐.
- 특히 대규모 프로젝트에서 레이아웃 분리와 내부 컴포넌트 관리가 쉬워진다는 걸 깨달음.

### 🏷️ 키워드 (태그):

`#Next.js` `#RouteGroup` `#PrivateFolder` `#URL제외폴더` `#폴더기반라우팅` `#프로젝트구조`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-08-27 | Route Group & Private Folder | `(폴더명)`은 URL 제외 + 하위 라우팅 가능, `_(폴더명)`은 URL에서 완전히 제외 | Route Group으로 layout 분리, Private Folder에 내부 전용 컴포넌트 관리 | `(폴더)`와 `_(폴더)` 차이 확실히 이해, 폴더 네이밍만으로 구조 제어 가능성 체감 | `#Next.js` `#RouteGroup` `#PrivateFolder` `#폴더기반라우팅` | ✅ |
