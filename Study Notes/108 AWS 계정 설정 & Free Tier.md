# AWS 계정 설정 & Free Tier

### 📅 날짜:

> 2025.12.09 (화)
> 

### 📘 오늘 공부한 주제:

> AWS 회원가입 및 Free Tier 서비스
> 

---

## 📝 핵심 개념 요약

### 1. AWS 회원가입

- **가입 링크:** [AWS Free Tier](https://aws.amazon.com/ko/free/)
- **주요 단계**
    1. 무료 계정 생성 → 이메일/계정명 입력 → 인증 코드 확인
    2. 비밀번호 설정 → Free Plan 선택
    3. 연락처 정보 입력 → 개인 사용 선택
    4. 결제 정보 입력 → 카드 인증 ($1 결제 후 환불)
    5. 휴대전화 인증 → 인증번호 확인
    6. Support 플랜 선택 → 기본 지원(무료)
    7. 회원가입 완료 후 콘솔 로그인 → 루트 사용자 로그인

### 2. AWS Free Tier

- **목적:** 신규 사용자가 AWS 서비스를 무료로 체험하고 학습 가능
- **유형**
    1. **6개월 무료 (6 Months Free)**
        - 매달 일정량 무료 사용(또는 크레딧 소진 시까지)
        - 대표 서비스:
            - EC2: 월 750시간 (t2.micro/t3.micro)
            - S3: 5GB 저장 + 20,000 GET + 2,000 PUT
            - RDS: 월 750시간 (db.t2.micro/db.t3.micro)
            - Lambda: 월 100만 요청 + 400,000GB-초
            - CloudFront: 50GB 데이터 전송
    2. **영구 무료 (Always Free)**
        - 계정 생성 후 지속적으로 무료 제공
        - 대표 서비스:
            - Lambda: 월 100만 요청
            - DynamoDB: 25GB 저장 + 200만 요청
            - SNS: 100만 게시 요청
            - SES: 월 62,000 이메일 발송
            - CloudWatch: 10개 알람
    3. **단기 체험 (Trials)**
        - 특정 기간 무료 제공
        - 대표 서비스:
            - SageMaker: 2개월 무료
            - Redshift: 2개월 무료
            - Lightsail: 1개월 무료
            - WorkSpaces: 40시간 무료

## 📊 핵심 요약 표

| 항목 | 내용 |
| --- | --- |
| 가입 링크 | [AWS Free Tier](https://aws.amazon.com/ko/free/) |
| 가입 단계 | 이메일 인증 → 비밀번호 설정 → Free Plan → 연락처/결제 → 전화 인증 → Support 선택 → 콘솔 로그인 |
| Free Tier 유형 | 6개월 무료, 영구 무료, 단기 체험 |
| 대표 서비스 | EC2, S3, RDS, Lambda, CloudFront, DynamoDB, SNS, SES, CloudWatch 등 |

### 💻 실습 내용 정리

- AWS 무료 계정 생성 및 루트 사용자 로그인
- Free Tier 서비스 제공량 확인
- 주요 서비스(EC2, S3, Lambda 등) 무료 범위 실습 계획

### ❗ 헷갈렸던 점 / 문제 해결:

| 문제 | 해결 |
| --- | --- |
| 카드 정보 입력 필요성 | $1 결제 후 환불, 추가 인증 가능 |
| Free Tier 기간 및 서비스 종류 | 6개월 무료, 영구 무료, Trials로 구분 → 각 서비스별 용량 확인 |
| 루트 계정과 IAM 계정 차이 | 루트 계정 → 전체 권한, 실습용 IAM 계정 추천 |

### 💡 느낀 점 / 배운 점:

- Free Tier 활용하면 실제 AWS 서비스를 비용 부담 없이 학습 가능
- 계정 생성 후 바로 콘솔 접근 가능 → 실습 환경 구축 용이
- 각 서비스별 무료 범위를 숙지해야 과금 발생 방지 가능

### 🏷️ 키워드 (태그):

`#AWS` `#회원가입` `#FreeTier` `#6개월무료` `#영구무료` `#Trials` `#EC2` `#S3` `#RDS` `#Lambda` `#CloudFront` `#DynamoDB` `#SNS` `#SES` `#CloudWatch` `#클라우드학습`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-12-09 | AWS 계정 설정 & Free Tier | AWS 회원가입 단계, Free Tier 유형 및 서비스 | 계정 생성, 루트 로그인, Free Tier 서비스 확인 | 카드 인증 이해, Free Tier 범위 숙지, 루트/IAM 계정 차이 이해 | `AWS` `회원가입` `FreeTier` `EC2` `S3` `RDS` `Lambda` `CloudFront` | ✅ |
