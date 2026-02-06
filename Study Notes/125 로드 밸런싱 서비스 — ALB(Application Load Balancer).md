# 로드 밸런싱 서비스 — ALB(Application Load Balancer)

### 📅 날짜:

> 2025.01.01 (목)
> 

### 📘 오늘 공부한 주제:

> AWS 로드밸런싱(ALB), Target Group, Listener, Health Check, 로드밸런싱 실습
> 

---

## 📝 핵심 개념 요약

- 로드 밸런서는 **트래픽을 여러 서버로 균등하게 분배**하는 장치
- ALB는 **HTTP/HTTPS 전용** 로드밸런서
- 하나의 도메인으로 **여러 서비스 경로 기반 라우팅 가능**
- 헬스 체크를 통해 **상태가 정상인 서버만** 트래픽 수신
- Target Group = 요청을 실제 처리하는 서버 묶음
- Listener = ALB에서 요청 받는 포트(80/443)
- 실습에서 ALB → 3000번 포트 express 서버에 연결

## 📊 핵심 요약 표

| 구분 | 설명 |
| --- | --- |
| 로드 밸런서 역할 | 사용자 요청을 여러 서버로 분산하여 안정성과 성능 확보 |
| ALB 특징 | HTTP/HTTPS 전용, 경로 기반 라우팅, SSL 관리 |
| Target Group | ALB가 전달하는 실제 서버들의 묶음 |
| Listener | ALB가 요청을 받는 포트(80/443) |
| Health Check | 정상 응답하는 서버만 트래픽 전달 |
| 장점 | 고가용성, 확장성, 성능 향상 |
| 실습 목적 | ALB 생성 → Target Group 연동 → 2개 서버로 로드밸런싱 테스트 |

### 💻 실습 내용 정리

### 1) ALB 생성

- EC2 콘솔 → 로드 밸런서 생성
- ALB 선택
- 모든 가용영역/서브넷 선택

### 2) 보안 그룹 구성

- HTTP/HTTPS 허용 SG 생성
- ALB에 기존 SG 제거 후 새 SG 적용

### 3) Target Group 생성

- 타입: 인스턴스
- 포트: express 서버 포트에 맞게 → **3000**
- Health check 경로: `/health`
- 서버 코드에 `/health` 엔드포인트 추가:

```jsx
app.get("/health", (req, res) => {
  res.send("Health Check Success");
});
```

### 4) Target Group에 EC2 등록

- “보류 중 → 포함” 클릭

### 5) ALB Listener에 Target Group 연결

- 리스너 80 → 생성한 Target Group 선택

### 6) 로드밸런서 생성 후 테스트

- ALB DNS 주소로 접속 → express "Hello World" 확인

### 7) EC2 한 대 더 생성하여 로드밸런싱 동작 확인

- 새 인스턴스 ssh 접속 → nodejs 설치 → git clone → 서버 실행
- 새로고침할 때마다
    - 서버1: "Hello World"
    - 서버2: "Hello World from Server-2"
        
        번갈아 나오면 성공
        

### ❗ 헷갈렸던 점 / 문제 해결:

| 헷갈렸던 점 | 해결 방법 |
| --- | --- |
| Target Group 포트 80으로 두면 왜 안 됨? | express 서버가 3000에서 동작하므로 Target Group 포트도 반드시 3000으로 변경 |
| Health Check가 실패하는 문제 | `/health` 엔드포인트 서버에 직접 작성 필요 |
| ALB 생성 후 접속 안됨 | 보안그룹에서 HTTP/HTTPS 허용했는지 다시 확인 |
| 두 서버 응답이 번갈아 나오지 않음 | target group의 두 EC2 인스턴스 상태가 "Healthy"인지 체크 |

### 💡 느낀 점 / 배운 점:

- ALB는 단순 트래픽 분산뿐 아니라 **경로 기반 라우팅**, **SSL 종료**, **헬스 체크** 등 웹 서비스 운영에 필수 기능을 제공한다는 것을 알게 됨
- 직접 EC2 두 대를 띄워 로드밸런싱을 실습하니 트래픽이 서버별로 분배되는 구조가 확실히 이해됨
- Health Check 하나만 올바르게 구성해도 서비스 안정성이 크게 향상되는 것을 실감함
- AWS 서비스들이 서로 긴밀하게 연동되기 때문에 아키텍처 흐름을 이해하는 것이 중요함

### 🏷️ 키워드 (태그):

`AWS` `ALB` `로드밸런서` `TargetGroup` `Listener` `HealthCheck` `EC2` `LoadBalancing` `HTTP` `HTTPS` `BackendInfra`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-01-01 | ALB (Application Load Balancer) | 트래픽을 서버 여러 대로 분산하여 고가용성과 확장성 확보. Target Group, Listener, Health Check 활용. | ALB 생성 → Target Group 구성 → Health Check → 2대 EC2 로드밸런싱 테스트 | Target Group 포트, Health Check 설정 오류 해결. 두 서버 응답이 번갈아 나오는 구조 직접 확인하며 이해 깊어짐. | `AWS` `ALB` `LoadBalancer` `EC2` `TargetGroup` | ✅ |
