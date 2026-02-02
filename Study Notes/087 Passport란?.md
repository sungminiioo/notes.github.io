# Passport란?

### 📅 날짜:

> 2025.11.05 (수)
> 

### 📘 오늘 공부한 주제:

> Node.js의 인증 미들웨어 `Passport` 개념과 구조 이해
> 

---

## 📝 핵심 개념 요약

- **Passport**는 Node.js를 위한 **유연하고 확장 가능한 인증 미들웨어**입니다.
    
    이름처럼 “여권(passport)”이 사용자의 신원을 증명하듯,
    
    Passport는 사용자의 **신원을 확인하고 서비스 접근을 허용**하는 역할을 합니다.
    
    Passport는 **‘인증의 틀’만 제공**하고, 실제 인증 방식은 **Strategy(전략)** 을 통해 결정됩니다.
    
    즉, “**어떻게 로그인할 것인가?**”를 전략으로 분리해 구조화합니다.
    

## 📊 핵심 요약 표

| 개념 | 설명 |
| --- | --- |
| **Passport** | Node.js용 인증 미들웨어, 인증 절차의 공통 틀 제공 |
| **Strategy** | 로그인 방법(로컬, 구글, 페이스북, JWT 등)을 정의 |
| **LocalStrategy** | 서비스 자체 계정(ID/PW)으로 로그인 |
| **OAuth Strategy** | 구글, 카카오 등 제3자 인증으로 로그인 |
| **JWT Strategy** | 토큰 기반 인증 (세션 없이 인증 유지) |
| **done() 함수** | 인증 결과를 Express에 전달 (`done(err, user)`) |
| **req.user** | 인증 성공 시 사용자 정보가 담기는 객체 |
| **req.isAuthenticated()** | 사용자가 인증된 상태인지 판별하는 메서드 |

### 💻 실습 내용 정리

### ✅ 1. 설치

```bash
npm install passport passport-local
```

### ✅ 2. 폴더 구조

```
config/
 └── passport.js            # 전략 등록
middlewares/passport/
 ├── localStrategy.js       # 로컬 전략
 └── jwtStrategy.js         # JWT 전략
```

### ✅ 3. 로컬 전략 등록 (`localStrategy.js`)

```jsx
passport.use('local', new LocalStrategy(
  function(username, password, done) {
    User.findOne({ username }, (err, user) => {
      if (err) return done(err);
      if (!user) return done(null, false);
      if (!user.verifyPassword(password)) return done(null, false);
      return done(null, user);
    });
  }
));
```

### ✅ 4. Passport 초기화 (`app.js`)

```jsx
import passport from './config/passport.js';

app.use(passport.initialize());
app.use(passport.session());
```

### ✅ 5. 로그인 라우터 설정

```jsx
app.post('/login', passport.authenticate('local'), (req, res) => {
  res.status(200).json({ message: "로그인 성공", data: req.user });
});
```

### ✅ 6. 인가 미들웨어 (`auth.js`)

```jsx
function passportAuthenticate(req, res, next) {
  if (!req.isAuthenticated()) {
    return res.status(401).json({ message: 'Unauthorized' });
  }
  next();
}
```

### ❗ 헷갈렸던 점 / 문제 해결:

`done()`의 동작이 헷갈림

→ `done(null, false)`는 “사용자 없음”, `done(null, user)`는 “성공” 의미.

→ Express는 `req.user`에 해당 사용자 정보를 자동 저장.

 `req.isAuthenticated()`가 어디서 나왔는지 혼동

→ Passport가 세션과 통합될 때 자동으로 추가되는 메서드.

→ 세션을 사용할 경우 `passport.session()` 미들웨어 필수.

### 💡 느낀 점 / 배운 점:

- Passport는 인증 자체보다 **인증 구조를 표준화하는 역할**임을 깨달음.
- “Strategy” 개념이 로그인 방식을 추상화해 코드 재사용성을 극대화시킴.
- 단순 로그인에는 편리하지만, **커스터마이징이 필요한 복잡한 로직엔 구조 이해가 필수**.
- “인증 = 사용자의 신원을 증명하는 절차”임을 여권 비유로 직관적으로 이해함.

### 🏷️ 키워드 (태그):

`#Passport` `#Node.js` `#Express` `#인증` `#Authentication` `#Strategy` `#LocalStrategy` `#OAuth` `#JWT` `#미들웨어`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-11-05 | Passport.js 인증 | Node.js 인증 미들웨어, 전략 기반 인증 구조 | LocalStrategy 등록, authenticate 미들웨어 적용 | `done()` 역할 이해, req.isAuthenticated() 동작 파악 | `#Passport` `#Express` `#인증` `#Strategy` `#JWT` | ✅ |
