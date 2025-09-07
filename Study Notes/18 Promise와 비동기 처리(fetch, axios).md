# Promise와 비동기 처리

### 📅 날짜:

> 2025.07.22 (화)
> 

### 📘 오늘 공부한 주제:

> 자바스크립트 **Promise와 비동기 처리(fetch, axios)**
> 

---

### 📝 핵심 개념 요약:

- **Promise 3가지 상태**
- `pending`: 비동기 작업 진행 중
- `fulfilled`: 성공적으로 완료되어 값 반환
- `rejected`: 실패하거나 오류 발생
- **then(), catch()**
    - `then`: fulfilled 시 실행할 코드 등록
    - `catch`: rejected 시 실행할 코드 등록
    - 체이닝으로 여러 `.then()` 연결 가능
- **Promise.all()**
    - 여러 Promise를 동시에 실행하고 **모두 완료되면** 결과 배열 반환
- **fetch**
    - 표준 Promise 기반 HTTP 요청
    - `.then()` 체인 또는 `async/await`로 사용 가능
    - `POST` 시 `method`, `body`, `headers` 지정 필요
    - `res.ok`로 성공 여부 확인, 실패 시 `throw new Error()`
- **axios**
    - fetch와 비슷하지만 **더 간단한 문법**과 실무에 유용한 기능 제공
    - `axios.get()`, `axios.post()` 지원
    - `async/await`와 try/catch로 오류 처리 간단화

### 📌 핵심 요약 표

| 개념 | 설명 | 예시 코드 |
| --- | --- | --- |
| Promise 상태 | `pending` → `fulfilled` / `rejected` | - |
| then/catch | 성공 시 `.then()`, 실패 시 `.catch()` | `fetch(url).then(...).catch(...)` |
| Promise.all | 여러 비동기 작업을 병렬 실행 | `Promise.all([p1, p2]).then(...)` |
| fetch | 표준 HTTP 요청 (Promise 기반) | `await fetch(url)` |
| axios | 간결하고 실무 친화적인 HTTP 요청 | `await axios.post(url, data)` |

### 💻 실습 내용 정리:

- `fetch`로 GET/POST 요청 실습 (`then/catch`, `async/await` 두 가지 방식)
- `Promise.all()`로 두 API를 병렬 호출 후 결과 배열 확인
- `axios`를 사용한 GET/POST 요청과 try/catch 오류 처리 연습
- `res.ok` 체크 후 실패 시 에러 던지기 실습

### ❗ 헷갈렸던 점 / 문제 해결:

- `Promise.all`은 하나라도 실패하면 전체가 `rejected` 된다는 점을 재확인
- `fetch`와 `axios`의 오류 처리 차이를 반복 연습하여 이해 (axios는 자동으로 `throw`)

### 💡 느낀 점 / 배운 점:

- `async/await`가 `.then()`보다 가독성이 좋아 실무에서 더 많이 사용될 것 같음
- `axios`는 기본 설정과 응답 처리 방식이 fetch보다 직관적이라 대규모 프로젝트에 유리

### 🏷️ 키워드 (태그):

`#Promise` `#fetch` `#axios` `#비동기` `#then` `#asyncAwait` `#PromiseAll`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-07-22 | Promise와 비동기 처리 | Promise 상태, then/catch, fetch, axios | fetch와 axios로 GET/POST 요청 실습 | Promise.all 동작 원리와 fetch/axios 차이 이해 | `#Promise` `#fetch` `#axios` `#asyncAwait`   | ✅ |
