# ORM & Prisma 실무 활용 정리

### 📅 날짜:

> 2025.12.29 (월)
> 

### 📘 오늘 공부한 주제:

> ORM 개념 이해 및 Prisma 기반 DB 실무 작업
> 

---

## 📝 핵심 개념 요약

- ORM은 객체지향 코드와 SQL을 **자동 변환**해주는 번역기 역할
- Prisma는 Node.js에서 가장 널리 사용되는 ORM 중 하나
- Prisma는 **스키마 → SQL 변환 → DB 반영**의 구조로 동작
- CRUD, JOIN, 트랜잭션, 집계 등 SQL 대부분을 Prisma 방식으로 수행 가능
- ORM은 생산성이 높지만, SQL 대비 성능 이슈가 있을 수 있음
- Prisma 프로젝트 구성 → SSH 터널링 → 마이그레이션 → 데이터 생성까지 실습 진행

## 📊 핵심 요약 표

| 항목 | 요약 | 설명 |
| --- | --- | --- |
| ORM | 객체 ↔ DB 번역기 | 코드가 SQL로 자동 변환 |
| Prisma | Node.js ORM | schema.prisma 기반 모델 정의 |
| CRUD | 생성/조회/수정/삭제 | Prisma 메서드로 간결하게 처리 |
| JOIN | 관계 조회 | include 옵션으로 자동 JOIN |
| 트랜잭션 | 여러 작업 원자성 보장 | `$transaction()`으로 처리 |
| 집계 | GROUP BY 기능 | `groupBy` 메서드 제공 |
| Raw SQL | 고급 쿼리 직접 실행 | `$queryRaw` 사용 |
| 실습 흐름 | 프로젝트 → 스키마 → 마이그레이션 → 실행 | 전체 실무 파이프라인 경험 |

### 💻 실습 내용 정리

### 1) 프로젝트 초기 세팅

```bash
mkdir rds-orm-practice
cd rds-orm-practice
npm init -y
npm i @prisma/client
npm i -D prisma
```

### 2) Prisma 초기화

```bash
npx prisma init
```

### 3) schema.prisma에 Post 모델 추가

```
model Post {
  id        Int      @id @default(autoincrement())
  title     String
  content   String?
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}
```

### 4) app.js 작성

(포스트 생성 기능)

```jsx
const { PrismaClient } = require("@prisma/client");
const prisma = new PrismaClient();

async function createPost(title, content) {
  const post = await prisma.post.create({ data: { title, content } });
  console.log("생성된 포스트:", post);
  return post;
}

async function main() {
  await createPost("첫 번째 포스트", "이것은 테스트 포스트입니다.");
  await prisma.$disconnect();
}

main();
```

### 5) SSH 터널링 실행

```bash
ssh -i [key.pem] -L [local_port]:[rds_endpoint]:5432 ec2-user@[EC2_IP]
```

### 6) .env에 DB URL 입력

```
DATABASE_URL="postgresql://username:password@localhost:5432/my_project"
```

### 7) Prisma 마이그레이션 실행

```bash
npx prisma migrate dev
```

### 8) Node로 데이터 생성

```bash
node app.js
```

DBeaver에서 데이터 생성 확인 완료!

### ❗ 헷갈렸던 점 / 문제 해결:

### 헷갈렸던 점

- Prisma schema를 수정했는데 DB에 반영이 안 됨
- SSH 터널링을 먼저 연결하지 않으면 DB 접속이 안 됨
- Prisma가 자동 생성하는 SQL이 실제로 어떻게 동작하는지 이해가 필요함

### 문제 해결

- 스키마 변경 후 항상 `prisma migrate dev`로 DB 반영
- SSH 터널링 성공 후 로컬 포트 → RDS로 흐름 이해
- Prisma의 `previewFeatures` 옵션, 로그 출력으로 SQL 확인하며 이해도 증가

### 💡 느낀 점 / 배운 점:

- ORM은 SQL을 몰라도 시작할 수 있지만 **SQL을 알고 쓰면 훨씬 안정적**
- Prisma로 모델을 정의하면 실제 DB 스키마가 자동 관리되면서 생산성이 크게 올라감
- 트랜잭션, JOIN 등 복잡한 작업도 메서드 기반으로 매우 직관적
- 실무에서는 ORM + Raw SQL 병행이 가장 유연하다는 것도 배움

### 🏷️ 키워드 (태그):

`SSH터널링` `ORM` `Prisma` `CRUD` `ORM` `Prisma` `SQL` `NodeJS`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-12-29 | ORM & Prisma 실무 | ORM 개념 + Prisma로 CRUD·JOIN·마이그레이션 실습 | Prisma 초기화, schema 작성, migrate, SSH 터널링, 데이터 생성 | ORM–SQL 변환 이해, Prisma 실무 흐름 파악 | `#ORM` `#Prisma` `#SQL` `#NodeJS` | ✅ |
