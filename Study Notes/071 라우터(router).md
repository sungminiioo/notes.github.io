# 라우터(router)

### 📅 날짜:

> 2025.10.14 (화)
> 

### 📘 오늘 공부한 주제:

> Express 라우터 구조, 중복 제거, 모듈화, 라우터 레벨 미들웨어
> 

---

## 📝 핵심 개념 요약

- Express의 `app.route()`와 `express.Router()`를 사용하면 **중복된 경로를 구조적으로 관리** 가능
- 라우트와 미들웨어를 **모듈 단위로 분리**하여 코드 가독성, 유지보수성을 높임
- `app.use()` → **앱 전체에 적용되는 미들웨어**,
    
    `router.use()` → **특정 라우터에만 적용되는 미들웨어**
    
- 라우터 파일 분리 시 `routes/`, `controllers/`, `middlewares/` 디렉토리 구조로 관리

## 📊 핵심 요약 표

| 구분 | 기능 | 주요 메서드 / 키워드 | 장점 |
| --- | --- | --- | --- |
| **app.route()** | 동일한 경로의 다양한 HTTP 메서드를 묶음 | `.get()`, `.post()`, `.patch()`, `.delete()` | 중복 제거, 가독성 향상 |
| **express.Router()** | 독립된 라우터 모듈 정의 | `express.Router()` | 유지보수성·확장성 향상 |
| **라우터 레벨 미들웨어** | 특정 라우터 전용 미들웨어 등록 | `router.use(middleware)` | 인증, 로깅 등 라우트 단위 제어 |
| **프로젝트 구조화** | 역할별 모듈 분리 | `routes/`, `controllers/`, `middlewares/` | 코드 구조 명확, 협업 용이 |

### 💻 실습 내용 정리

### ✅ 1. **app.route()로 라우트 중복 제거**

```jsx
app
  .route("/products")
  .get((req, res) => res.json({ message: "Product 목록 보기" }))
  .post((req, res) => res.json({ message: "Product 추가하기" }));

app
  .route("/products/:id")
  .patch((req, res) => res.json({ message: "Product 수정하기" }))
  .delete((req, res) => res.json({ message: "Product 삭제하기" }));
```

### ✅ 2. **express.Router()로 라우트 모듈화**

```jsx
const productsRouter = express.Router();

productsRouter
  .route("/")
  .get((req, res) => res.json({ message: "Product 목록 보기" }))
  .post((req, res) => res.json({ message: "Product 추가하기" }));

productsRouter
  .route("/:id")
  .patch((req, res) => res.json({ message: "Product 수정하기" }))
  .delete((req, res) => res.json({ message: "Product 삭제하기" }));

app.use("/products", productsRouter);
```

### ✅ 3. **라우터 레벨 미들웨어 적용**

```jsx
const authMiddleware = (req, res, next) => {
  console.log("인증 실행");
  next();
};

productRouter.use(authMiddleware);
```

### ✅ 4. **Express 프로젝트 구조 모듈화 예시**

```
project/
├── src/
│   ├── routes/
│   │   ├── productRouter.js
│   │   └── userRouter.js
│   ├── middlewares/
│   │   └── always.js
│   └── app.js
```

### ❗ 헷갈렸던 점 / 문제 해결:

- **헷갈렸던 점:**
    
    `app.use()`와 `router.use()`의 차이점이 모호했음 — 둘 다 미들웨어 등록 메서드처럼 보여 혼동
    
- **문제 해결:**
    
    공식 문서를 참고하며, **`app.use()`는 전역 레벨**, **`router.use()`는 특정 라우터 내 전용**임을 확인.
    
    인증이나 로깅 등 라우트별로 다르게 적용하는 실습으로 개념이 명확해짐.
    

### 💡 느낀 점 / 배운 점:

- 라우터 구조를 분리하니 코드 가독성이 훨씬 좋아지고, 기능 단위로 유지보수가 용이해짐.
- “서버의 길을 설계한다”는 관점으로 라우터를 구성하는 것이 Express의 핵심임을 깨달음.
- 실제 규모 있는 프로젝트에서는 모듈화된 라우터 구조가 필수라는 점을 실감함.

### 🏷️ 키워드 (태그):

`#Express` `#Router` `#appRoute` `#라우터모듈화` `#routerUse` `#Middleware` `#NodeJS` `#서버설계` `#프로젝트구조화`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-10-14 | Express 라우터 | app.route()와 express.Router()를 통한 라우트 중복 제거 및 구조화 | app.route(), express.Router(), router.use() 실습 | app.use() vs router.use() 구분이 헷갈렸으나 실습으로 해결 | `#Express` `#Router` `#Middleware` `#라우터분리` `#NodeJS` | ✅ |
