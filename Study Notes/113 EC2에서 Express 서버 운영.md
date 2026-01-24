# EC2에서 Express 서버 운영

### 📅 날짜:

> 2025.12.16 (화)
> 

### 📘 오늘 공부한 주제:

> EC2 인스턴스에서 Express 서버 구축 및 FileZilla를 통한 파일 전송
> 

---

## 📝 핵심 개념 요약

### 1. Express 서버 구축 과정

1. **SSH 연결**: EC2 인스턴스 접속
2. **Node.js 설치**: nvm 사용, 최신 LTS 설치
3. **Express 환경 구성**: `npm init -y`, `npm i express`
4. **서버 파일 작성**: `server.js` → `/hello` 라우트에서 “Hello World” 반환
5. **보안 그룹 설정**: TCP 3000 포트 열기
6. **서버 실행**: `node server.js`
7. **브라우저 테스트**: `http://퍼블릭IP:3000/hello`

### 2. EC2 파일 전송(FileZilla)

- **SFTP**: SSH 기반 안전한 파일 전송
- **권한 문제 해결**: 서버 폴더 소유권 변경 (`sudo chown -R ec2-user:ec2-user /home/ec2-user/server`)

## 📊 핵심 요약 표

| 개념 | 설명 |
| --- | --- |
| nvm | Node.js 버전 관리 도구 |
| Express | Node.js 기반 웹 서버 프레임워크 |
| server.js | Express 서버 코드 파일 |
| 보안 그룹 | EC2 접근 포트/규칙 설정 |
| SFTP(FileZilla) | SSH 기반 안전한 파일 전송, GUI 방식 |
| 권한 설정 | EC2 파일 소유권 변경 → ec2-user 접근 가능 |

### 💻 실습 내용 정리

| 단계 | 내용 |
| --- | --- |
| 1 | SSH로 EC2 인스턴스 접속 |
| 2 | nvm 설치 후 Node.js LTS 버전 설치 |
| 3 | Node.js 및 npm 버전 확인 |
| 4 | 서버 디렉터리 생성 (`mkdir server`) → 이동 (`cd server`) |
| 5 | Express 설치 (`npm init -y`, `npm i express`) |
| 6 | server.js 생성 → Express 서버 작성 |
| 7 | 보안 그룹 TCP 3000 포트 열기 |
| 8 | 서버 실행 (`node server.js`) → 브라우저 접속 확인 |
| 9 | FileZilla 설치 및 SFTP 연결 |
| 10 | 로컬 파일 → EC2 서버로 업로드 → 권한 문제 발생 시 `chown`으로 해결 |

### ❗ 헷갈렸던 점 / 문제 해결:

| 문제 | 해결 |
| --- | --- |
| server.js 덮어쓰기 실패 | 파일 소유권이 root로 되어 있어 ec2-user 접근 불가 → `sudo chown -R ec2-user:ec2-user /home/ec2-user/server` |
| 브라우저 접근 불가 | TCP 3000 포트 미허용 → 보안 그룹 인바운드 규칙 추가 |

### 💡 느낀 점 / 배운 점:

- Express 서버 구축과 실행 과정 이해 → Node.js 환경 필수
- EC2에서 직접 파일 전송 시 권한 문제를 반드시 확인
- 보안 그룹 포트 설정 없으면 외부 접근 불가 → 운영 서버 환경 필수 확인

### 🏷️ 키워드 (태그):

`#AWS` `#EC2` `#Express` `#NodeJS` `#SFTP` `#FileZilla` `#보안그룹` `#서버운영`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-12-16 | EC2 Express 서버 구축 & 파일 전송 | Node.js + Express 설치 → server.js 작성 → TCP 3000 포트 오픈 → 브라우저 테스트 | SSH 접속 → nvm 설치 → Node.js 설치 → Express 설치 → server.js 작성 → 보안 그룹 포트 열기 → 서버 실행 → FileZilla로 파일 업로드 | server.js 소유권 변경 필요 → ec2-user 권한 확보 → 브라우저 접근 문제 해결 | `AWS` `EC2` `Express` `NodeJS` `SFTP` `FileZilla` `보안그룹` `서버운영` | ✅ |
