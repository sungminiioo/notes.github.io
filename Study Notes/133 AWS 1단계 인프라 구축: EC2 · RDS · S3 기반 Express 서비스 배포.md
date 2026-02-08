# AWS 1단계 인프라 구축: EC2 · RDS · S3 기반 Express 서비스 배포

### 📅 날짜:

> 2025.01.13 (화)
> 

### 📘 오늘 공부한 주제: 

> **AWS 기본 인프라를 활용한 Express 백엔드 서비스 최소 구성 및 배포**
> 
> - EC2로 Express 서버 배포
> - RDS(PostgreSQL) 연동
> - S3 + multer-s3 이미지 업로드
> - Presigned URL을 통한 private 이미지 접근
> - PM2로 서버 백그라운드 실행 및 자동 재시작 설정

---

## 📝 핵심 개념 요약

- **“일단 돌아가는 서비스”를 목표로 최소 인프라 구성**
- AWS 프리티어 범위 내에서 EC2 · RDS · S3 핵심 서비스 사용
- SSH 터널링을 통해 로컬 ↔ RDS 안전하게 연결
- S3 퍼블릭/프라이빗 업로드 전략 분리
- PM2로 Express 서버를 안정적으로 운영

## 📊 핵심 요약 표

| 구성 요소 | 사용 서비스 | 핵심 역할 |
| --- | --- | --- |
| 서버 | EC2 (Amazon Linux 2023) | Express API 실행 |
| DB | RDS (PostgreSQL) | 데이터 영속성 관리 |
| 파일 저장 | S3 | 이미지 업로드/제공 |
| 업로드 | multer-s3 | Express ↔ S3 연동 |
| 보안 접근 | Presigned URL | private 이미지 임시 접근 |
| 프로세스 관리 | PM2 | 서버 백그라운드 실행 및 자동 재시작 |

### 💻 실습 내용 정리

### 1️⃣ EC2 인스턴스 생성

- 리전: **서울(ap-northeast-2)**
- 인스턴스 타입: `t2.micro` (프리티어)
- 키 페어 `.pem` 생성 및 **권한 설정(chmod 400)**
- 인바운드 규칙: TCP 3000 포트 오픈
- 탄력적 IP 연결 (연습 후 해제)

---

### 2️⃣ RDS(PostgreSQL) 생성

- 템플릿: 프리 티어
- EC2와 연결 설정 (보안 그룹 자동 구성)
- 자동 백업 비활성화 (비용 방지)
- EC2 ↔ RDS 간 5432 포트 통신 확인

---

### 3️⃣ DBeaver로 RDS 연결

- SSH 터널링 사용
- PostgreSQL DB 연결
- `CREATE DATABASE my_project;`
- Prisma migrate로 테이블 생성 확인

---

### 4️⃣ 로컬 개발 환경

- Express + Prisma 베이스 코드 클론
- `.env`에 SSH 터널 기반 `DATABASE_URL` 설정
- `npx prisma migrate dev`
- seed 데이터 삽입
- REST Client로 API 테스트

---

### 5️⃣ SSH 터널링 단축 명령

- `.env`에 EC2 / RDS 정보 등록
- `dotenv-cli` + npm script로 `npm run ssh` 구성

---

### 6️⃣ S3 버킷 생성

- 리전: 서울
- 퍼블릭 엑세스 차단 유지
- 버킷 정책으로 `public/` 폴더만 GetObject 허용

---

### 7️⃣ multer-s3 이미지 업로드

- IAM 정책 생성 (PutObject, GetObject)
- 전용 IAM 사용자 + Access Key 발급
- `multer-s3`로 S3 `public/` 업로드 구현
- 업로드 후 S3에서 이미지 확인

---

### 8️⃣ Presigned URL 적용

- private 이미지 업로드 시 `private/` 폴더 사용
- `@aws-sdk/s3-request-presigner` 활용
- 5분짜리 Presigned URL 생성

---

### 9️⃣ EC2 Express 배포 + PM2

- EC2에 Node.js, Git 설치
- 운영 환경용 `prisma migrate deploy`
- PM2로 Express 실행
- `pm2 startup` + `pm2 save`로 자동 실행 설정
- EC2 퍼블릭 IP:3000으로 최종 테스트

### ❗ 헷갈렸던 점 / 문제 해결:

- **SSH 키 권한 문제**
    - `.pem` 파일 권한이 잘못되면 SSH 접속 실패
    - `chmod 400` 필수
- **migrate vs migrate deploy**
    - 로컬: `migrate dev`
    - 운영: `migrate deploy`
- **S3 퍼블릭/프라이빗 정책 차이**
    - ACL ❌, 버킷 정책만 사용
- **포트 번호 노출**
    - 아직 ALB/Nginx 없음 → `:3000` 직접 접근 필요

### 💡 느낀 점 / 배운 점:

- “완벽한 구조”보다 **돌아가는 구조**가 먼저라는 걸 체감
- AWS 서비스들은 **보안 그룹과 정책 이해가 핵심**
- 프리티어라도 **백업, 탄력 IP 등은 비용 주의**
- Presigned URL로 **보안과 편의성의 균형**을 맞출 수 있음
- PM2 덕분에 “서버는 꺼지지 않는다”는 감각을 처음 가짐

### 🏷️ 키워드 (태그):

`AWS` `EC2` `RDS` `S3` `Express` `PostgreSQL` `Prisma` `PM2` `Multer-s3` `PresignedURL` `백엔드배포` `프리티어주의`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-01-13 | AWS 1단계 인프라 구축 | EC2·RDS·S3로 최소 서비스 구성 | Express 배포, 이미지 업로드, PM2 설정 | SSH 키 권한, migrate 구분 이해 | `AWS` `EC2` `RDS` `S3` `PM2` | ✅ |
