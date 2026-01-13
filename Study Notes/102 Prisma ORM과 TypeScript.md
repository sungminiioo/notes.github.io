# Prisma ORM과 TypeScript

### 📅 날짜:

> 2025.12.01 (월)
> 

### 📘 오늘 공부한 주제:

> Prisma ORM 프로젝트 초기화, 스키마 정의, 타입 안전한 DB 작업 및 TypeScript 적용
> 

---

## 📝 핵심 개념 요약

- **Prisma ORM**: TypeScript 친화적인 ORM, 자동 타입 생성, 타입 안전성, IDE 자동완성 지원
- **프로젝트 초기화**: `npx prisma init`, `prisma.schema` 정의, `PrismaClient` 생성
- **자동 타입 생성**: Prisma Client 생성 시 스키마 기반 타입 자동 생성 (`Post`, `User`)
- **타입 안전한 CRUD**: `create`, `findMany`, `findUnique`, `update` 등 모든 DB 작업에 타입 안전 제공
- **부분 타입 활용**: `Prisma.PostCreateInput`, `Prisma.PostUpdateInput` 등을 사용하여 필요한 필드만 처리 가능
- **서비스 레이어 타입 적용**: API 핸들러, 서비스 함수에서 Prisma 타입 활용 → 타입 안전 + IDE 지원

## 📊 핵심 요약 표

| 개념 | 설명 |
| --- | --- |
| PrismaClient | DB 접근용 클라이언트, 자동 타입 생성 |
| 스키마 정의 | `schema.prisma`에서 모델 정의, 데이터베이스 구조 지정 |
| 자동 타입 생성 | `npx prisma generate` 시 TypeScript 타입 자동 생성 |
| 타입 안전 CRUD | `create`, `findMany`, `findUnique`, `update` 등 모든 작업 타입 안전 |
| 부분 타입 | `Prisma.PostCreateInput`, `Prisma.PostUpdateInput` 등 활용 가능 |
| 서비스/핸들러 적용 | API 레이어에서 Prisma 타입 사용 시 타입 안전 및 IDE 지원 |

### 💻 실습 내용 정리

### ✔ 프로젝트 초기화

```bash
mkdir my-app
cd my-app
npm init -y
npm i -D typescript ts-node @types/node
npx tsc --init
```

- tsconfig.json 설정

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "strict": true,
    "esModuleInterop": true,
    "outDir": "dist",
    "rootDir": "src"
  },
  "include": ["src"]

```

### ✔ Prisma 설치 및 초기화

```bash
npm i -D prisma
npm i @prisma/client
npx prisma init
npm install dotenv
```

### ✔ 스키마 정의 (Post 모델)

```
model Post {
  id        String   @id @default(uuid())
  title     String
  content   String
  author    String
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}
```

### ✔ Prisma Client 및 CRUD

```tsx
import { PrismaClient } from '@prisma/client';
const prisma = new PrismaClient();

// 생성
const post = await prisma.post.create({
  data: { title: "TS + Prisma", content: "ORM example", author: "John" }
});

// 조회
const posts = await prisma.post.findMany();

// 업데이트
await prisma.post.update({ where: { id: "some-id" }, data: { title: "Updated" } });
```

### ✔ 타입 활용 예시

```tsx
import { Post, Prisma } from "@prisma/client";

// 함수 매개변수 타입
async function createPost(postData: Prisma.PostCreateInput): Promise<Post> {
  return await prisma.post.create({ data: postData });
}

// 부분 타입 활용
type PostUpdateInput = Prisma.PostUpdateInput;
async function updatePost(id: string, data: PostUpdateInput): Promise<Post> {
  return await prisma.post.update({ where: { id }, data });
}
```

### ✔ 서비스 레이어 적용

```tsx
async function getPostsService(): Promise<Post[]> {
  return await prisma.post.findMany({
    include: { author: true, _count: { select: { comments: true } } },
    orderBy: { createdAt: 'desc' },
    take: 10
  });
}
```

### ❗ 헷갈렸던 점 / 문제 해결:

| 문제 | 해결 |
| --- | --- |
| Prisma가 자동 생성하는 타입 이해 필요 | `npx prisma generate` 후 `@prisma/client` 타입 import |
| 부분 타입(PostCreateInput, PostUpdateInput) 혼동 | Prisma 제공 타입 활용, 필요한 필드만 지정 가능 |
| findUnique 결과 null 처리 | 타입 `Post |
| 스키마 변경 후 타입 업데이트 | `npx prisma generate` 재실행 |

### 💡 느낀 점 / 배운 점:

- Prisma는 TypeScript와 완벽한 연계, DB 작업의 타입 안전성 극대화
- IDE에서 자동완성 지원으로 개발 생산성 ↑
- 스키마 중심 설계 → 타입 생성 자동 → 오류 가능성 최소화
- 서비스 레이어에서 타입을 활용하면 API 안정성과 코드 유지보수 용이

### 🏷️ 키워드 (태그):

`#Prisma` `#TypeScript` `#ORM` `#PrismaClient` `#자동타입` `#타입안전` `#CRUD` `#서비스레이어`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-12-01 | Prisma ORM과 TypeScript | Prisma + TS: 자동 타입 생성, 타입 안전성, IDE 지원, CRUD, 부분 타입 활용 | 프로젝트 초기화, schema.prisma 정의, PrismaClient 생성, CRUD, 타입 활용, 서비스 레이어 적용 | Prisma 타입 자동 생성 이해, 부분 타입 활용, null 처리, 스키마 변경 후 타입 업데이트 | `Prisma` `TypeScript` `ORM` `PrismaClient` `자동타입` `타입안전` `CRUD` `서비스레이어` | ✅ |
