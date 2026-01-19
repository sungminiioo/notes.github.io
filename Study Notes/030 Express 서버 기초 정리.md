# Express 서버 기초 정리

### 📅 날짜:

> 2025.08.18 (월)
> 

### 📘 오늘 공부한 주제:

> Express 서버 구조, 라우트, HTTP 메서드, 요청·응답 처리
> 

---

## 📝 핵심 개념 요약

- **Express 서버 기본 구조**: `express import → app 생성 → 라우트 정의 → 서버 실행`
- **라우트(Route)**: `app.method(path, handler)` 형식으로 HTTP 메서드별 엔드포인트 처리
- **리퀘스트 객체(req)**: `query`, `params`, `body`로 클라이언트 요청 정보 접근
- **리스폰스 객체(res)**: `send()`, `status()`, `sendStatus()` 등으로 응답 처리
- **HTTP 메서드 활용**: GET(조회), POST(생성), PATCH(수정), DELETE(삭제)
- **쿼리스트링 처리**: `req.query` 활용, 기본값 설정 가능
- **에러 처리**: `res.status(code).send(message)`로 조건부 에러 처리
- **JSON 바디 처리**: `app.use(express.json())` 필요
- **DELETE 처리 핵심**: id 존재 확인 후 삭제, 성공 시 204 No Content, 실패 시 404 메시지

## 📊 핵심 요약 표

| 구분 | 내용 | 예시 |
| --- | --- | --- |
| 서버 구조 | import → app 생성 → 라우트 → listen | `const app = express(); app.listen(3000)` |
| 라우트 정의 | `app.method(path, handler)` | `app.get('/hello', (req,res)=>{})` |
| GET 쿼리 | `req.query`, 기본값 설정 가능 | `const sort = req.query.sort |
| URL 파라미터 | `req.params` | `/users/:id` → `req.params.id` |
| POST 바디 | `req.body` + `express.json()` | `{ "name": "넷플릭스" }` |
| PATCH | 부분 수정, Object.assign, updatedAt 갱신 | `PATCH /sub/:id` |
| DELETE | id 존재 확인 후 배열에서 제거, 204/404 | `DELETE /subscriptions/:id` |
| 상태 코드 | `res.status(code)` / `res.sendStatus(code)` | `res.status(404).send('Not Found')` |

### 💻 실습 내용 정리

1. **서버 실행**

```jsx
import express from 'express';
const app = express();
app.listen(3000, ()=> console.log('Server Started'));
```

1. **GET 라우트**

```jsx
app.get('/hello', (req,res)=>{
  res.send('Bye Express!');
});
```

1. **POST 요청 처리**

```jsx
app.use(express.json());
app.post('/sub', (req,res)=>{
  const data = req.body;
  res.status(201).send(data);
});
```

1. **PATCH 요청 처리**

```jsx
app.patch('/sub/:id', (req,res)=>{
  const id = req.params.id;
  const target = subs.find(s=>s.id === id);
  if(!target) return res.status(404).send('Not Found');
  Object.assign(target, req.body, {updatedAt: new Date()});
  res.send(target);
});
```

1. **DELETE 요청 처리**

```jsx
app.delete('/subscriptions/:id', (req,res)=>{
  const index = subs.findIndex(s=>s.id === req.params.id);
  if(index === -1) return res.status(404).send({message:'Cannot find given id.'});
  subs.splice(index,1);
  res.sendStatus(204);
});
```

### ❗ 헷갈렸던 점 / 문제 해결:

- DELETE 시 **본문 없이 204 응답** 처리 필요, 실수로 send() 하면 상태 코드가 200으로 바뀔 수 있음.
- PATCH 요청 시 **부분 업데이트**를 Object.assign으로 처리하고, updatedAt 갱신을 누락하지 않음

### 💡 느낀 점 / 배운 점:

- Express는 간단한 구조지만 HTTP 메서드별 처리와 req/res 객체 활용법을 명확히 이해해야 API 설계가 깔끔해짐.
- 상태 코드와 응답 메시지를 정확히 구분해야 클라이언트와 서버 간 통신 오류를 줄일 수 있음.
- `express.json()`은 POST/PATCH 시 필수임을 실습으로 체감.

### 🏷️ 키워드 (태그):

`#Express` `#NodeJS` `#API` `#GET` `#POST` `#PATCH` `#DELETE` `#HTTP` `#REST` `#req` `#res` 

| 날짜       | 주제             | 핵심 요약                                               | 실습 내용                                       | 문제 해결 / 느낀 점                               | 키워드 태그                             | 복습 필요 |
|------------|------------------|--------------------------------------------------------|------------------------------------------------|------------------------------------------------|----------------------------------------|-----------|
| 2025-08-18 | Express 서버 기초 | Express 구조, 라우트, req/res, HTTP 메서드, 상태 코드 처리 | GET /hello, POST /sub, PATCH /sub/:id, DELETE /subscriptions/:id | DELETE 204 처리, PATCH updatedAt 갱신 확인 | `#Express` `#nodeJS` `#REST` `#API`  | ✅        |

