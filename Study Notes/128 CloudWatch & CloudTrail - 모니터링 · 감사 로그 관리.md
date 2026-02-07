# CloudWatch & CloudTrail - 모니터링 · 감사 로그 관리

### 📅 날짜:

> 2025.01.06 (화)
> 

### 📘 오늘 공부한 주제:

> AWS CloudWatch 모니터링 · 로그 관리 + CloudTrail 감사 및 활동 추적
> 

---

## 📝 핵심 개념 요약

- **CloudWatch**: AWS 리소스의 상태를 모니터링(지표·알람·로그·대시보드)하는 시스템
- **CloudTrail**: AWS 계정 내부에서 발생하는 모든 API 호출과 활동을 기록하는 감사 시스템
- CloudWatch = 실시간 **건강 검진**
- CloudTrail = 리소스 활동을 기록하는 **CCTV/블랙박스**
- 실습: **CPU 알람 생성**, IAM 권한 부족 문제 해결
- CloudTrail로 리소스 변경·데이터 액세스 이벤트 추적 가능

## 📊 핵심 요약 표

| 항목 | 내용 |
| --- | --- |
| CloudWatch 역할 | AWS 리소스 상태 모니터링 & 로그 수집 |
| 주요 기능 | 메트릭 / 알람 / 로그 / 대시보드 |
| CloudTrail 역할 | 모든 AWS 활동 기록 (감사·보안·추적) |
| 이벤트 종류 | Management Events / Data Events |
| CloudWatch vs CloudTrail | 상태 모니터링 vs 활동 추적 |
| 실습 핵심 | CPU 알람 생성 + SNS/IAM 권한 문제 해결 |

### 💻 실습 내용 정리

### **1️⃣ CloudWatch – CPU 알람 생성**

- CloudWatch → 경보 → 새 경보 생성
- 지표 선택: `EC2 → CPUUtilization`
- 임계값 70 입력
- SNS 주제 생성 시도 → IAM 권한 부족으로 실패
- IAM에서 `AmazonSNSFullAccess` 정책 연결
- 다시 SNS 주제 생성 성공
- 경보 이름 입력 후 생성 완료

---

## 🔍 CloudWatch 주요 개념 요약

- CPU/메모리/네트워크 같은 지표 확인
- 특정 조건 이상 시 메일·SMS 알람
- 로그 그룹을 통해 서버 로그 중앙 관리
- 대시보드로 시각화
- 비용·지연·보존기간 고려 필요

---

## 🕵️ CloudTrail 주요 개념 요약

- 누가/언제/무엇을/어디서 했는지 기록
- 리소스 생성, 삭제, 보안 설정 변경 등을 모두 남김
- 보안 사고 분석·문제 해결·비용 증가 원인 파악에 필수

### ❗ 헷갈렸던 점 / 문제 해결:

- **SNS 주제 생성 실패 원인** → IAM 계정에 권한 없음
- **해결** → `AmazonSNSFullAccess` 정책 추가
- CloudWatch 임계값 입력 시 `%` 단위 없이 숫자만 넣어야 함
- CloudWatch는 “상태 모니터링”, CloudTrail은 “행동 기록”이라는 역할 차이를 이번에 확실히 이해

### 💡 느낀 점 / 배운 점:

- AWS 운영에서 CloudWatch + CloudTrail은 필수 조합
- 문제 발생 전에 CloudWatch가 감지하고,
    
    문제 발생 후엔 CloudTrail로 원인을 찾는 흐름이 이해되었다
    
- IAM 권한이 얼마나 중요한지 다시 확인
- 실제 운영 환경과 매우 유사한 실습이라 큰 도움이 됨

### 🏷️ 키워드 (태그):

`AWS` `CloudWatch` `CloudTrail` `Monitoring` `Logging` `Audit` `Alarm` `SNS` `IAM` `API` `Logs` `EC2` `CPUUtilization` `Security`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-01-06 | CloudWatch & CloudTrail | 모니터링 시스템 + 감사 로그 시스템 이해 | CPU 알람 생성 / SNS 주제 생성 / IAM 권한 설정 | SNS 권한 오류 해결, CloudWatch·CloudTrail 개념 완전히 정리됨 | `AWS` `CloudWatch` `CloudTrail` `Monitoring` `Audit` | ✅ |
