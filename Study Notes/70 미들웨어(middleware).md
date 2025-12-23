# 미들웨어(middleware)

### 📅 날짜:

> 2025.10.13 (월)
> 

### 📘 오늘 공부한 주제:

> Express의 미들웨어 구조와 동작 원리
> 

---

## 📝 핵심 개념 요약

- Express는 요청과 응답을 처리하기 위한 **미들웨어 기반 프레임워크**
- 모든 요청은 **미들웨어 체인**을 거쳐 처리됨
- `(req, res)`는 요청/응답 객체이며, `next()`로 다음 미들웨어로 이동
- 에러 처리 미들웨어는 `(err, req, res, next)` 구조로 정의
- `app.use()`, `app.all()`, 라우트 수준 미들웨어로 유연한 적용 가능
- `express.json()`, `express.static()` 등 내장 미들웨어로 기능 확장 용이

## 📊 핵심 요약 표

| 구분 | 설명 | 예시 코드 |
| --- | --- | --- |
| 일반 미들웨어 | 요청 → 응답 처리 | `app.get('/', (req, res) => res.json({msg:'OK'}))` |
| 체이닝 미들웨어 | 여러 단계 처리 | `app.get('/', mw1, mw2)` |
| next() | 다음 미들웨어 실행 | `next()` |
| 에러 미들웨어 | 에러 처리 전용 | `(err, req, res, next)` |
| use / all | 전역 미들웨어 등록 | `app.use(logger)` |

### 💻 실습 내용 정리

```jsx
// 1️⃣ 기본 미들웨어 예시
app.get('/', (req, res) => {
  res.json({ message: '안녕, 코드잇!' });
});

// 2️⃣ next()로 다음 미들웨어로 전달
function first(req, res, next) {
  req.user = { name: '철수' };
  next();
}
function second(req, res) {
  res.json(req.user);
}
app.get('/', first, second);

// 3️⃣ 인증 미들웨어 예시
const authentication = (req, res, next) => {
  const token = req.headers.authorization?.split(" ")[1];
  if (!token) return res.status(401).json({ message: "인증 필요" });
  next();
};

// 4️⃣ 에러 처리 미들웨어
app.use((err, req, res, next) => {
  res.status(err.statusCode || 500).json({ message: err.message });
});
```

### ❗ 헷갈렸던 점 / 문제 해결:

- **헷갈렸던 점:** 미들웨어 체이닝 순서가 바뀌면 요청 흐름이 왜 달라지는지 직관적으로 이해가 어려웠음.
- **문제 해결:** `next()` 실행 시 요청이 다음 미들웨어로 이동한다는 구조를 콘솔 로그로 직접 확인하며, **라우트 등록 순서와 next() 호출 위치의 중요성**을 체감함.

### 💡 느낀 점 / 배운 점:

- Express는 단순한 문법으로도 복잡한 서버 로직을 **미들웨어 단위로 깔끔하게 분리**할 수 있음.
- “요청 흐름을 설계한다”는 관점에서 서버 동작을 이해하게 되어, **코드 구조를 시각적으로 떠올릴 수 있게 됨.**

### 🏷️ 키워드 (태그):

`#Express` `#Middleware` `#req` `#res` `#next` `#ErrorHandler` `#NodeJS` `#미들웨어` `#서버기초`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-10-13 | Express 미들웨어 | 요청과 응답 사이의 처리 흐름 제어 구조. `next()`, 에러핸들러, use/all 등 다양한 미들웨어 방식 이해 | 기본 라우팅, next() 흐름 제어, 인증/에러 미들웨어 작성 | 미들웨어 순서 중요성 이해, 유지보수성 향상 체감 | `#Express` `#Middleware` `#next` `#ErrorHandler`  | ✅ |
