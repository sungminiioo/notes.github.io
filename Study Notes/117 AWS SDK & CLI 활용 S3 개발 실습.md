# AWS SDK & CLI 활용 S3 개발 실습

### 📅 날짜:

> 2025.12.22 (월)
> 

### 📘 오늘 공부한 주제:

> AWS SDK를 이용한 S3 이미지 업로드, IAM 사용자 및 권한 관리, AWS CLI를 통한 S3 정적 웹사이트 배포 및 업데이트
> 

---

## 📝 핵심 개념 요약

### 1. AWS SDK

| 개념 | 설명 |
| --- | --- |
| **AWS SDK** | Software Development Kit, 코드로 AWS 서비스를 제어/자동화 가능 |
| **REST API Wrapper** | AWS 서비스 API를 쉽게 호출할 수 있는 라이브러리 |
| **주요 사용 사례** | S3 파일 업로드/다운로드, EC2 인스턴스 관리, RDS 제어 등 |

### 2. AWS CLI

| 개념 | 설명 |
| --- | --- |
| **AWS CLI** | Command Line Interface, 터미널에서 AWS 서비스 관리 가능 |
| **특징** | 웹 콘솔 없이 작업 가능, 자동화 스크립트 작성 가능, 빠른 작업 처리 |

## 📊 핵심 요약 표

| 개념 | 설명 |
| --- | --- |
| AWS SDK | Node.js, Python 등 코드로 AWS 서비스 제어, REST API를 쉽게 사용 |
| AWS CLI | 터미널에서 AWS 서비스 관리, 자동화 스크립트 작성 가능 |
| IAM 정책 | S3 파일 업로드 권한 부여, ARN 기반 리소스 접근 제어 |
| S3 업로드 | PutObjectCommand 사용, 파일 경로(Key) 지정 필요 |
| 환경변수 관리 | AWS Key는 로컬 `.env` 또는 CI/CD 환경에 안전하게 저장 |

### 💻 실습 내용 정리

| 단계 | 내용 |
| --- | --- |
| 1 | 프로젝트 폴더 생성 및 Node.js 초기화 (`npm init -y`) |
| 2 | AWS SDK 설치 (`npm i @aws-sdk/client-s3`) |
| 3 | `app.js` 작성 → 로컬 이미지 파일을 S3에 업로드하는 코드 구현 |
| 4 | IAM 사용자 생성 및 S3 PutObject 권한 부여 (ARN 지정) |
| 5 | IAM 사용자 엑세스 키 생성 → `.env`에 AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY 저장 |
| 6 | Node.js 실행 → S3에 이미지 업로드 확인 |
| 7 | AWS CLI 설치 (Windows/MacOS) 및 설치 확인 (`aws --version`) |
| 8 | `aws configure` 실행 → IAM 사용자 자격 증명 등록 |
| 9 | `aws s3 cp` 명령어로 정적 웹사이트 파일 재배포 및 업데이트 |

### ❗ 헷갈렸던 점 / 문제 해결:

| 문제 | 해결 |
| --- | --- |
| 브라우저에서 직접 AWS Key 사용 → 보안 문제 | Node.js 서버에서 SDK/Pre-signed URL 사용, Key는 절대 클라이언트에 노출하지 않음 |
| IAM 정책 설정 오류 → 권한 부족 | S3 버킷 ARN 정확히 지정, PutObject 권한 연결 후 해결 |
| CLI 배포 시 파일 덮어쓰기 | `aws s3 cp` 사용 시 대상 경로 정확히 지정 |

### 💡 느낀 점 / 배운 점:

- AWS SDK를 통해 코드에서 S3 파일 업로드, 다운로드, 권한 제어까지 자동화 가능
- AWS CLI로 정적 웹사이트 재배포가 훨씬 빠르고 효율적
- IAM 정책과 Key 관리는 보안 관점에서 매우 중요, Key 절대 노출 금지
- SDK와 CLI를 활용하면 서버/배포 자동화가 가능, 학습 및 실무 적용 시 유용

### 🏷️ 키워드 (태그):

`#AWS` `#S3` `#AWS_SDK` `#AWS_CLI` `#NodeJS` `#IAM` `#권한관리` `#자동화` `#정적웹사이트` `#업로드`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-12-22 | AWS SDK & CLI 개발 활용 | SDK로 코드에서 S3 제어, CLI로 터미널에서 S3 관리 | Node.js S3 업로드, IAM 권한 설정, CLI로 파일 재배포 | Key 노출 문제 해결, IAM 정책 오류 수정, CLI 재배포 효율 경험 | `AWS` `S3` `SDK` `CLI` `NodeJS` `IAM` `자동화` `정적웹사이트` `업로드` | ✅ |
