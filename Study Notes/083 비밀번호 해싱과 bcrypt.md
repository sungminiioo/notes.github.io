# 비밀번호 해싱과 bcrypt

### 📅 날짜:

> 2025.10.30 (목)
> 

### 📘 오늘 공부한 주제:

> 비밀번호 안전 저장 – 해싱, 솔트, 키 파생 함수, bcrypt
> 

---

## 📝 핵심 개념 요약

- **암호화 vs 해싱**
    - 암호화: 양방향, 원본 복원 가능
    - 해싱: 단방향, 원본 복원 불가 → 비밀번호 저장에 사용
- **해싱의 문제점**
    - **사전 공격(Dictionary Attack)**: 흔한 비밀번호와 해시값 비교
    - **무차별 대입 공격(Brute Force)**: 가능한 모든 조합 시도
- **해결책**
    - **솔트(Salt)**: 사용자별 랜덤 문자열 추가 → Rainbow Table 공격 방지
    - **슬로우 해싱(Slow Hashing)**: 의도적으로 계산 느리게 → 공격 비용 증가
    - **강력한 비밀번호 정책, 로그인 제한, 2FA 등**
- **키 파생 함수 (Key Derivation Function)**
    - 해시 반복, 메모리 사용 증가, 병렬 공격 방지
    - 주요 예시: PBKDF2, bcrypt, scrypt, Argon2
- **bcrypt**
    - salt + 해싱을 동시에 처리 가능
    - saltRounds 설정으로 반복 횟수 제어 (보안과 성능 조절)
    - createUser 시 비밀번호 해싱, getUser 시 해시 비교 적용

## 📊 핵심 요약 표

| 개념 | 특징 | 적용/예시 |
| --- | --- | --- |
| 해싱 | 단방향, 고정 길이, 원본 복원 불가 | 비밀번호 저장 |
| 솔트 | 사용자별 랜덤 문자열 | Rainbow Table 공격 방지 |
| 슬로우 해싱 | 계산 의도적으로 느리게 | Brute Force 공격 방지 |
| 키 파생 함수 | 반복 해싱, 메모리 사용 증가 | PBKDF2, bcrypt, scrypt, Argon2 |
| bcrypt | salt + 반복 해싱, 안전성 높음 | createUser → 해싱, getUser → 비교 |

### 💻 실습 내용 정리

### 1️⃣ 회원가입(createUser) – bcrypt 적용

```jsx
import bcrypt from 'bcrypt';
import * as userRepository from '../repositories/userRepository.js';

export async function createUser({ email, password, name }) {
  // 비밀번호 해싱
  const saltRounds = 10;
  const hashedPassword = await bcrypt.hash(password, saltRounds);

  // DB 저장
  const newUser = await userRepository.save({ email, password: hashedPassword, name });
  return newUser;
}
```

- Prisma Studio에서 기존 유저 삭제 후 테스트
- 해싱된 비밀번호 확인 → 원본은 저장되지 않음

### 2️⃣ 로그인(getUser) – bcrypt 비교 적용

```jsx
import bcrypt from 'bcrypt';
import * as userRepository from '../repositories/userRepository.js';

export async function getUser({ email, password }) {
  const user = await userRepository.findUserByEmail(email);
  if (!user) throw new Error('Invalid credentials');

  const isMatch = await bcrypt.compare(password, user.password);
  if (!isMatch) throw new Error('Invalid credentials');

  return user;
}
```

- Prisma Studio에서 기존 유저 삭제 후 테스트
- 입력 비밀번호와 해시 비교 → 로그인 성공/실패 확인

### ❗ 헷갈렸던 점 / 문제 해결:

**문제:**

- 단순 해시와 솔트의 차이
- bcrypt 사용법
- 반복 해싱과 속도 조절

**해결:**

- 솔트는 해시 입력값에 랜덤 문자열 추가 → 동일 비밀번호라도 다른 해시 생성
- genSalt + hash 또는 hash(password, saltRounds) 모두 가능
- saltRounds / timeCost / memoryCost 설정으로 보안 vs 성능 균형 맞춤

### 💡 느낀 점 / 배운 점:

- 단순 해싱만으로는 사전 공격이나 무차별 공격에 취약 → 솔트와 슬로우 해싱 필요
- bcrypt는 솔트 생성과 해싱을 동시에 처리해 간단하고 안전
- 키 파생 함수와 반복 해싱(Key Stretching)으로 공격 비용 증가
- 실습을 통해 createUser/getUser에 보안 로직 적용 흐름 이해

### 🏷️ 키워드 (태그):

`#해싱` `#비밀번호보안` `#bcrypt` `#솔트` `#슬로우해싱` `#키파생함수` `#PBKDF2` `#scrypt` `#Argon2` `#ExpressJS` `#Prisma`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-10-30 | 해싱과 bcrypt 적용 | 단방향 해싱, 솔트, 슬로우 해싱, 키 파생 함수, bcrypt | createUser → bcrypt 해싱, getUser → bcrypt 비교, Prisma Studio 테스트 | 솔트/슬로우 해싱 개념 이해, bcrypt 적용 경험 | `#해싱` `#bcrypt` `#솔트` `#키파생함수` `#ExpressJS` `#Prisma` | ✅ |
