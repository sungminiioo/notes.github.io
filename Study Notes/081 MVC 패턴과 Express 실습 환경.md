# MVC 패턴과 Express 실습 환경

### 📅 날짜:

> 2025.10.28 (화)
> 

### 📘 오늘 공부한 주제:

> MVC 패턴 이해 및 Express 실습 환경 설정
> 

---

## 📝 핵심 개념 요약

- **MVC 패턴**: Model, View, Controller의 역할을 분리하여 애플리케이션을 구조화하는 패턴
- **Model**: 데이터와 비즈니스 로직 담당, DB CRUD, 유효성 검사, 트랜잭션
- **View**: 사용자에게 화면 표시, HTML/CSS/React 등을 통해 데이터 표현
- **Controller**: 사용자 요청 처리, 모델과 뷰 연결, 서비스 호출, 응답 반환
- **Service 계층**: 모델에서 비즈니스 로직 분리, 여러 모델에 걸친 작업 처리, 재사용 가능
- **Repository 계층**: DB 접근 전담, Prisma/ORM 사용, CRUD와 쿼리 최적화
- **Express 구조 실습 환경**: 계층별 폴더 구성, Prisma DB 연동, 트랜잭션 처리, 환경변수 설정, 외부 DB(Render.com) 연결

## 📊 핵심 요약 표

| 구분 | 내용 요약 |
| --- | --- |
| MVC 패턴 | Model-View-Controller로 관심사 분리 |
| Model | 데이터 관리, 비즈니스 로직, DB 처리 |
| View | 화면 구성 및 사용자에게 정보 표시 |
| Controller | 요청 수신, 모델 호출, 뷰 연결, 응답 반환 |
| Service | 복잡한 비즈니스 로직 처리, 트랜잭션, 재사용 |
| Repository | 데이터 접근 전담, ORM/Prisma 사용 |
| 장점 | 코드 구조화, 팀 협업 용이, 유지보수/테스트 쉬움 |

### 💻 실습 내용 정리

### 1️⃣ Express 프로젝트 구조

```
project-root/
├── src/
│   ├── controllers/  # 요청/응답 처리 및 서비스 연결
│   ├── services/     # 비즈니스 로직 처리
│   ├── repositories/ # DB 접근
│   ├── middlewares/  # 인증, 유효성 검사 등
│   ├── routes/       # API 라우트
│   ├── config/       # 환경 설정
│   └── app.js        # 앱 진입점
├── prisma/           # 스키마, 마이그레이션, 시드
├── .env              # 환경 변수
└── package.json
```

### 2️⃣ DB 설정 및 초기화

```bash
git clone https://github.com/wonee09/topic-express-user-system.git
cd topic-express-user-system
npm install
mv .env.sample .env
# DATABASE_URL에 Render.com PostgreSQL URL 삽입
npm run migrate
```

- Prisma ORM 사용
- 트랜잭션 처리 및 CRUD 예제 포함
- 서비스 계층에서 복잡한 비즈니스 로직 처리

### ❗ 헷갈렸던 점 / 문제 해결:

**문제:**

- 서비스 계층과 모델의 경계
- 컨트롤러에서 DB 직접 접근
- Prisma migrate dev 오류

**해결:**

- 모델은 CRUD, 서비스는 비즈니스 로직과 트랜잭션 처리
- 원칙적으로 금지, 서비스 호출 후 응답 처리만
- 배포용 migrate deploy 사용, 권한 문제 회피

### 💡 느낀 점 / 배운 점:

- MVC 패턴과 서비스/레포지토리 계층을 분리하면 대규모 애플리케이션 유지보수가 용이함
- 컨트롤러는 요청/응답 처리에 집중하고, 비즈니스 로직은 서비스에서 처리하는 것이 원칙
- Prisma ORM과 트랜잭션 개념을 이해하면 DB와 안전하게 연동 가능
- 외부 환경(Render.com)에서 테스트하면 실제 OAuth/세션 기능 검증에 유리

### 🏷️ 키워드 (태그):

`#MVC패턴` `#ExpressJS` `#ServiceLayer` `#Repository` `#Prisma` `#트랜잭션` `#웹개발기초`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-10-28 | MVC 패턴 및 Express 실습 환경 | MVC 패턴 이해, Service/Repository 계층 역할 정리 | Express 프로젝트 구조 설정, Prisma DB 연동, 트랜잭션 및 CRUD 실습 | 서비스/컨트롤러 역할 명확히 구분, Prisma migrate dev 문제 해결 | `#MVC패턴` `#ExpressJS` `#ServiceLayer` `#Repository` `#Prisma` | ✅ |
