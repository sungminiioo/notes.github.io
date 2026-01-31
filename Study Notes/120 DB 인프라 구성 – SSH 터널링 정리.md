# DB 인프라 구성 – SSH 터널링 정리

### 📅 날짜:

> 2025.12.25 (목)
> 

### 📘 오늘 공부한 주제:

> AWS DB 인프라 구성 — SSH 터널링으로 RDS 안전하게 접속하기
> 

---

## 📝 핵심 개념 요약

- SSH 터널링은 **EC2를 중간 다리(Jump Server)**로 활용해 **로컬 PC → EC2 → RDS**로 안전하게 연결하는 방식
- RDS를 Public Access = YES로 열면 **보안 리스크 증가 + 보안 그룹 관리 번거로움 + 퍼블릭 IP 비용 발생**
- RDS는 **Private Subnet**에 두고, 오직 **EC2에서만 접근 허용**하는 것이 정석
- SSH 명령어를 이용해 **Local Port Forwarding** 구성 → 로컬에서 DB가 있는 것처럼 사용 가능
- 개발 편의성과 보안 모두 잡는 방식

## 📊 핵심 요약 표

| 구분 | 핵심 내용 |
| --- | --- |
| SSH 터널링 정의 | EC2를 거쳐 RDS에 안전하게 접속하는 기술 |
| 필요성 | RDS Public Access 보안 위험 제거 + 퍼블릭 IP 비용 절감 |
| 구성 방식 | 내 PC → SSH로 EC2 접속 → EC2가 RDS로 내부망 전달 |
| 장점 | 고보안(비노출), 암호화 채널, 최소 보안그룹 규칙, 개발 환경에서 편리 |
| 핵심 명령어 | `ssh -i key.pem -L 5432:rds-endpoint:5432 ec2-user@EC2_IP` |
| 실습 핵심 | EC2 생성 → RDS 생성 → SG 연결 → 터널링 구성 |

### 💻 실습 내용 정리

### 1) SSH 터널링 개념 이해

- EC2는 퍼블릭/프라이빗 네트워크를 모두 연결하는 **점프 서버** 역할
- SSH 터널 사용하면 로컬 PC 5432 → RDS 5432로 투명하게 전달됨

### 2) RDS 인스턴스 생성

- 엔진: PostgreSQL
- 템플릿: 프리티어
- EC2 컴퓨팅 리소스 연결 선택
- 자동 백업 비활성화
- 생성 후 보안 그룹에서 inbound 5432 규칙 확인
- Outbound 규칙에서 EC2 → RDS 연결 허용 확인

### 3) SSH 터널 명령 실행

```bash
ssh -i key.pem -L 5432:rds-endpoint.amazonaws.com:5432 ec2-user@EC2_PUBLIC_IP
```

### 4) 로컬에서 DB 접속

```bash
psql -h localhost -p 5432 -U postgres
```

### ❗ 헷갈렸던 점 / 문제 해결:

### 헷갈렸던 점

- 왜 RDS Public Access=YES로 두면 안 되는지 명확하지 않았음
- SSH 명령어 옵션 `L` 의미가 헷갈림
- EC2 SG와 RDS SG 관계가 헷갈렸음

### 문제 해결

- RDS Public Access는 **해커 포트 스캔 + IP 관리 번거로움 + 비용 부과** 문제 → Private Subnet 활용이 정석
- `L`은 **로컬 포트 포워딩 옵션**이라는 개념을 구조도와 함께 이해
- RDS SG inbound를 “EC2 SG만 허용”함으로써 최소 권한 구조 완성

### 💡 느낀 점 / 배운 점:

- RDS를 외부에 노출하지 않고도 개발환경에서 완벽히 접근 가능한 구조라는 점이 인상적
- SSH 터널링 구조가 “보안·편의성·비용” 모두 잡는다는 것을 체감
- AWS 인프라는 결국 **네트워크 설계 + 보안 구조** 이해가 핵심이라는 점을 깨달음
- 실무에서 왜 대부분 **EC2 → RDS 구조**를 사용하는지 명확해짐

### 🏷️ 키워드 (태그):

`AWS` `RDS` `EC2` `SSH 터널링` `보안` `Private Subnet` `포트 포워딩` `DB 인프라` `VPC`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-12-25 | DB 인프라 구성 — SSH 터널링 | SSH를 이용해 EC2를 경유하여 RDS Private Subnet에 안전하게 접속하는 방식. 보안·비용·편의성 모두 개선함. | EC2 생성 → RDS 생성 → SG 연결 → 터널링 명령 실행 → 로컬에서 DB 접속 | Public Access의 위험 이해, -L 옵션 구조 파악, RDS SG inbound 최소화 구조 학습 | `AWS` `RDS` `EC2` `SSH` `포트포워딩` `Private` `Subnet` `보안` | ✅ |
