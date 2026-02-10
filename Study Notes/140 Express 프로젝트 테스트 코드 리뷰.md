# Express 프로젝트 테스트 코드 리뷰

### 📅 날짜:

> 2025.01.22 (목)
> 

### 📘 오늘 공부한 주제:

> Express 프로젝트에서 테스트 코드가 적용되는 구조 이해
> 
> 
> `app.ts` / `server.ts` 분리 이유
> 
> 인증(회원가입/로그인) 테스트 흐름
> 
> CRUD(Create / Read / Update / Delete) API 테스트 패턴
> 
> Supertest + Jest 조합 활용 방식
> 
> 실제 프로젝트 테스트 코드 **리뷰 방법**
> 

---

## 📝 핵심 개념 요약

- **Express 테스트의 핵심은 app과 server 분리**
- 테스트에서는 **실제 서버를 띄우지 않는다**
- API 테스트는 **요청 → 응답 → 상태코드 → 데이터 검증** 흐름
- 인증 테스트는 **성공 / 실패 케이스를 반드시 분리**
- CRUD 테스트는 **패턴이 거의 동일** → 재사용 가능
- 테스트 코드는 “검증 로직”이 아니라 **행동 시나리오 문서**

## 📊 핵심 요약 표

| 구분 | 핵심 포인트 | 이유 |
| --- | --- | --- |
| app.ts | Express 설정만 담당 | 테스트에서 import |
| server.ts | listen만 담당 | 테스트 제외 |
| Supertest | HTTP 요청 시뮬레이션 | 실제 서버 불필요 |
| 인증 테스트 | 성공/실패 분리 | 보안 핵심 |
| CRUD 테스트 | 패턴 반복 | 유지보수 용이 |

### 💻 실습 내용 정리

### 1️⃣ 프로젝트 클론 및 리뷰 목적

```bash
gitclone https://github.com/wonee09/topic-jest.git
```

📌 **목표**

- “테스트를 직접 작성” ❌
- “실제 프로젝트에서 테스트가 어떻게 구성되는지 이해” ⭕

---

## 2️⃣ app.ts 와 server.ts 구조 확인

### 📁 app.ts 역할

- Express 앱 생성
- 미들웨어 등록
- 라우터 연결
- **export app**

```tsx
const app =express();

app.use(express.json());
app.use('/users', userRouter);

exportdefault app;
```

📌 **테스트에서 Supertest가 직접 app을 사용**

---

### 📁 server.ts 역할

```tsx
import appfrom'./app';

app.listen(3000,() => {
console.log('Server running');
});
```

📌 테스트에서는 **절대 server.ts를 실행하지 않음**

👉 **왜?**

- 포트 충돌
- 테스트 속도 저하
- 제어 불가능한 서버 상태

---

## 3️⃣ 회원가입(Sign Up) 테스트 코드 리뷰

### 🔍 테스트 흐름

1. 회원가입 API 호출
2. 정상 입력 → 성공 응답
3. 잘못된 입력 → 에러 응답

```tsx
test('회원가입 성공',async () => {
const res =awaitrequest(app)
    .post('/users/signup')
    .send({
email:'test@test.com',
password:'password123'
    });

expect(res.status).toBe(201);
expect(res.body).toHaveProperty('id');
});
```

📌 **검증 포인트**

- 상태 코드 (201)
- 응답 데이터 구조
- 실제 비즈니스 결과

---

## 4️⃣ 로그인(Login) 테스트 코드 리뷰

### 🔍 핵심 포인트

- 성공 케이스
- 실패 케이스 (비밀번호 오류 / 없는 유저)

```tsx
test('로그인 실패 - 잘못된 비밀번호',async () => {
const res =awaitrequest(app)
    .post('/users/login')
    .send({
email:'test@test.com',
password:'wrong-password'
    });

expect(res.status).toBe(401);
});
```

📌 **인증 테스트는 반드시 실패 시나리오 포함**

---

## 5️⃣ 리뷰 생성 (Create) 테스트 코드

### 🔍 흐름

1. 로그인 → 토큰 발급
2. Authorization 헤더 설정
3. POST 요청

```tsx
test('리뷰 생성 성공',async () => {
const res =awaitrequest(app)
    .post('/reviews')
    .set('Authorization',`Bearer ${token}`)
    .send({
content:'좋은 상품입니다'
    });

expect(res.status).toBe(201);
});
```

📌 **인증이 필요한 API 테스트의 기본 패턴**

---

## 6️⃣ 리뷰 조회 (Read) 테스트 코드

```tsx
test('리뷰 목록 조회',async () => {
const res =awaitrequest(app)
    .get('/reviews');

expect(res.status).toBe(200);
expect(Array.isArray(res.body)).toBe(true);
});
```

📌 조회 테스트는

- **데이터 존재 여부**
- **배열/객체 형태**
    
    만 검증하는 경우가 많음
    

---

## 7️⃣ 리뷰 수정 (Update) 테스트 코드

```tsx
test('리뷰 수정 성공',async () => {
const res =awaitrequest(app)
    .put('/reviews/1')
    .set('Authorization',`Bearer ${token}`)
    .send({
content:'수정된 리뷰'
    });

expect(res.status).toBe(200);
});
```

📌 **PUT / PATCH 테스트 핵심**

- 권한 체크
- 수정 결과 반영 여부

---

## 8️⃣ 리뷰 삭제 (Delete) 테스트 코드

```tsx
test('리뷰 삭제 성공',async () => {
const res =awaitrequest(app)
    .delete('/reviews/1')
    .set('Authorization',`Bearer ${token}`);

expect(res.status).toBe(204);
});
```

📌 삭제 테스트는

- **응답 데이터 없음**
- **상태 코드**가 핵심

### ❗ 헷갈렸던 점 / 문제 해결:

### ❓ 왜 서버를 실행하지 않지?

👉 Supertest는 **Express app 객체만으로 요청을 흉내냄**

### ❓ 왜 DB 결과를 직접 검증 안 할까?

👉 통합 테스트에서는

- “API 관점 결과”에 집중
- 내부 구현은 유닛 테스트에서 검증

### 💡 느낀 점 / 배운 점:

- 테스트 코드는 **또 다른 컨트롤러 문서**
- CRUD 테스트는 **패턴화 가능**
- 인증 테스트가 가장 중요하고 까다롭다
- app/server 분리는 **테스트를 위한 설계**

### 🏷️ 키워드 (태그):

`Jest` `Express` `Supertest` `API테스트` `CRUD테스트` `통합테스트` 

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-01-22 | Express 테스트 리뷰 | API 테스트 구조 이해 | 회원/리뷰 CRUD | 패턴 중요 | `Supertest` | ✅ |
