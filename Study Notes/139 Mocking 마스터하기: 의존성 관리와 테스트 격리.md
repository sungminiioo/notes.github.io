# Mocking 마스터하기: 의존성 관리와 테스트 격리

### 📅 날짜:

> 2025.01.21 (수)
> 

### 📘 오늘 공부한 주제:

> Mock의 개념과 필요성
> 
> 
> `jest.fn()` / `jest.spyOn()`
> 
> 호출 검증 Matcher (`toHaveBeenCalled`)
> 
> Mock 반환값 설정 (`mockReturnValue`, `mockImplementation`)
> 
> 비동기 테스트 (`async/await`, `resolves`, `rejects`)
> 
> 에러 테스트 (`toThrow`, 비동기 에러)
> 
> Mock 정리 (`mockClear`, `mockReset`, `mockRestore`)
> 
> 테스트 라이프사이클 훅
> 
> 4단계 테스트 패턴 (Setup → Exercise → Assertion → Teardown)
> 

---

## 📝 핵심 개념 요약

- **Mock = 테스트를 위해 원본 대신 사용하는 가짜 구현**
- Mock의 목적은 **격리(Isolation)** 와 **호출 검증**
- `jest.fn()` → **완전한 가짜 함수**
- `jest.spyOn()` → **기존 함수 감시 + 필요 시 가짜로 변경**
- 외부 의존성(API, DB, 네트워크)은 **반드시 Mock**
- 비동기 테스트는 **async/await + Mock** 이 기본
- 테스트는 항상 **독립적**이어야 하므로 Mock 정리가 필수
- 좋은 테스트는 **구조가 보인다 (4단계 패턴)**

## 📊 핵심 요약 표

| 구분 | 설명 | 언제 사용 |
| --- | --- | --- |
| jest.fn | 가짜 함수 생성 | 의존성 완전 제거 |
| jest.spyOn | 기존 함수 감시 | 순수 로직 호출 검증 |
| toHaveBeenCalled | 호출 여부 | 로직 흐름 확인 |
| mockReturnValue | 고정 값 반환 | API/DB 결과 |
| mockImplementation | 로직 구현 | 조건 분기 테스트 |
| mockResolvedValue | Promise 성공 | 비동기 성공 |
| mockRejectedValue | Promise 실패 | 비동기 실패 |

### 💻 실습 내용 정리

### 1️⃣ Mock 함수란?

> 가짜 함수를 만들어 호출 여부·인자·횟수를 추적
> 

✔ 외부 의존성 제거

✔ 테스트 격리

✔ 로직 흐름 검증

---

### 2️⃣ jest.fn() 기본 사용

```tsx
const mockFn = jest.fn();

mockFn('hello');
mockFn('world');

expect(mockFn).toHaveBeenCalled();
expect(mockFn).toHaveBeenCalledTimes(2);
expect(mockFn).toHaveBeenCalledWith('hello');
```

📌 **원본 구현이 필요 없을 때 사용**

---

### 3️⃣ 호출 검증 Matcher

| Matcher | 의미 |
| --- | --- |
| toHaveBeenCalled | 한 번 이상 호출 |
| toHaveBeenCalledTimes | 호출 횟수 |
| toHaveBeenCalledWith | 인자 확인 |
| toHaveBeenLastCalledWith | 마지막 호출 |

---

### 4️⃣ jest.spyOn() – 기존 함수 감시

```tsx
const spy = jest.spyOn(emailService,'sendEmail');

emailService.sendEmail('user@test.com','Welcome');

expect(spy).toHaveBeenCalledWith('user@test.com','Welcome');

spy.mockRestore();
```

📌 **기존 로직을 실행하면서 호출만 검증**

---

### 5️⃣ jest.fn vs jest.spyOn 기준

| 상황 | 선택 |
| --- | --- |
| API / DB / 네트워크 | jest.fn |
| 순수 함수 | jest.spyOn |
| 구현 로직 필요 없음 | jest.fn |
| 실제 결과도 검증 | jest.spyOn |

---

### 6️⃣ Mock 반환값 설정

### mockReturnValue (고정값)

```tsx
jest.fn().mockReturnValue(true);
```

✔ 항상 같은 결과

✔ API 응답, DB 조회

---

### mockImplementation (로직)

```tsx
jest.fn().mockImplementation((age) => age >=20)
```

✔ 조건 분기

✔ 복잡한 시나리오

---

### 여러 번 다른 결과

```tsx
jest.fn()
  .mockReturnValueOnce('첫 번째')
  .mockReturnValueOnce('두 번째')
  .mockReturnValue('기본값');
```

---

### 7️⃣ 비동기 Mock & 테스트

```tsx
const mockApi = jest.fn().mockResolvedValue({id:1 });

const result =awaitmockApi();

expect(result.id).toBe(1);
```

📌 **비동기 테스트 기본은 async/await**

---

### 8️⃣ Promise 검증 Matcher

```tsx
awaitexpect(fetchUser()).resolves.toBeTruthy();
awaitexpect(fetchUserFail()).rejects.toThrow();
```

---

### 9️⃣ 에러 테스트

### 동기 에러

```tsx
expect(() =>divide(10,0)).toThrow('0으로 나눌 수 없습니다')
```

⚠ 반드시 **함수 래핑**

---

### 비동기 에러

```tsx
awaitexpect(fetchUser(-1)).rejects.toThrow('유효하지 않은 사용자');
```

---

## 🧹 Mock 정리 전략

### mockClear

- 호출 기록만 제거
- 설정 유지

### mockReset

- 호출 기록 + 설정 제거

### mockRestore

- spyOn 전용
- 원본 함수 복구

---

## 🔄 테스트 라이프사이클 훅

| 훅 | 실행 시점 | 용도 |
| --- | --- | --- |
| beforeAll | 그룹 시작 전 | 서버, DB 연결 |
| beforeEach | 테스트 전 | Mock 초기화 |
| afterEach | 테스트 후 | Mock 정리 |
| afterAll | 그룹 종료 | 리소스 해제 |

---

## 🎯 4단계 테스트 패턴

```tsx
test('주문 처리 테스트',() => {
// 1️⃣ Setup
const mockPayment = jest.fn().mockReturnValue({success:true });

// 2️⃣ Exercise
const result =processOrder(order, mockPayment);

// 3️⃣ Assertion
expect(result.status).toBe('completed');

// 4️⃣ Teardown
  mockPayment.mockClear();
});
```

📌 **모든 테스트는 이 구조를 유지하는 것이 이상적**

### ❗ 헷갈렸던 점 / 문제 해결:

### ❓ 왜 실제 API 호출하면 안 될까?

- 느림
- 네트워크 실패
- 비결정적 테스트

👉 **Mock으로 로직만 검증**

### ❓ jest.fn과 spyOn이 헷갈림

- “**원본이 필요하면 spyOn**”
- “**완전 대체면 jest.fn**”

### 💡 느낀 점 / 배운 점:

- Mock은 “가짜”가 아니라 **테스트 안정성의 핵심**
- 호출 검증은 로직 흐름을 문서화한다
- 비동기 테스트는 **Mock 없이는 거의 의미 없음**
- 좋은 테스트는 **읽기만 해도 동작이 보인다**

### 🏷️ 키워드 (태그):

`Jest` `Mocking` `jest.fn` `jest.spyOn` `비동기테스트` `에러테스트` `테스트격리`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-01-21 | Jest Mock | 의존성 제거 & 호출 검증 | fn, spy, async | 테스트 안정성 | `Mock` | ✅ |
