# Jest 핵심 API와 Matchers 정리

### 📅 날짜:

> 2025.01.20 (화)
> 

### 📘 오늘 공부한 주제:

> Jest 핵심 API 구조 (`expect`, `matcher`)
> 
> 
> `toBe`, `not` (원시값 비교)
> 
> `toStrictEqual`, `toMatchObject` (객체/배열 비교)
> 
> 자주 사용하는 Matcher 모음
> 
> 실무에서 Matcher 선택 기준
> 

---

## 📝 핵심 개념 요약

- **Jest 테스트의 핵심은 `expect(값).matcher(기대값)` 구조**
- Matcher는 **“어떻게 검증할 것인가”를 정의하는 도구**
- `toBe`는 **원시값 + 엄격 비교**
- 객체/배열은 **반드시 `toStrictEqual` 또는 `toMatchObject` 사용**
- `toMatchObject`는 **API 응답 테스트에 최적**
- 불필요한 전체 비교보다 **의미 있는 부분 검증이 중요**

## 📊 핵심 요약 표

| 구분 | 설명 | 사용 예 |
| --- | --- | --- |
| toBe | 원시값 엄격 비교 (`===`) | 숫자, 문자열 |
| not | 부정 검증 | 같지 않음 |
| toStrictEqual | 객체/배열 전체 비교 | 유닛 테스트 |
| toMatchObject | 객체 부분 비교 | API 응답 |
| Truthy/Falsy | 참/거짓 검증 | 조건 결과 |
| toContain | 포함 여부 | 배열/문자열 |
| toThrow | 에러 검증 | 예외 처리 |

### 💻 실습 내용 정리

### 1️⃣ expect와 toBe 구조 이해

```tsx
expect(value).toBe(expectedValue);
```

- expect: 테스트 대상 값
- matcher: 검증 방식
- toBe: `===` 비교

---

### 2️⃣ toBe / not 실습 (원시값)

```tsx
expect(add(1,2)).toBe(3);
expect(add(1,2)).not.toBe(4);
```

✔ number / string / boolean 에만 사용

❌ 객체, 배열에는 사용 불가

---

### 3️⃣ 할인 계산기 Matcher 실습

### 테스트 포인트

1. 정상 계산
2. 엣지 케이스 (0%, 100%)
3. 음수 값
4. 잘못된 타입

```tsx
expect(calculateDiscount(1000,10)).toBe(900);
expect(calculateDiscount(-1000,10)).toBe(0);
expect(calculateDiscount(1000,150)).toBe(0);
```

👉 **여러 케이스를 나눠 테스트하는 습관이 중요**

---

### 4️⃣ toStrictEqual (객체 깊은 비교)

```tsx
expect(formatUserData(rawData)).toStrictEqual(expected);
```

- 모든 키/값/구조가 동일해야 통과
- 데이터 변환 함수 테스트에 적합
- 배열, 중첩 객체도 정확히 비교 가능

---

### 5️⃣ toMatchObject (부분 비교)

```tsx
expect(response).toMatchObject({
id:1,
name:'John'
});
```

- 실제 객체가 더 커도 통과
- timestamp, metadata 무시 가능
- **API 응답 테스트의 핵심 Matcher**

---

### 6️⃣ 기타 자주 쓰는 Matcher 실습

| Matcher | 용도 |
| --- | --- |
| toBeTruthy / toBeFalsy | 조건 결과 |
| toContain | 배열/문자열 포함 |
| toMatch | 문자열 패턴 |
| toHaveLength | 길이 검증 |
| toHaveProperty | 객체 속성 |
| toThrow | 에러 발생 |
| toBeGreaterThan | 숫자 비교 |

### ❗ 헷갈렸던 점 / 문제 해결:

### ❓ 객체 비교에 toBe를 쓰면 왜 실패할까?

- 객체는 **메모리 주소(reference)** 비교
- 내용이 같아도 다른 객체 → 실패
- 해결: `toStrictEqual`, `toMatchObject`

### ❓ 언제 toMatchObject를 써야 할까?

- 응답에 **동적 값(timestamp)** 이 포함될 때
- 전체 구조보다 **핵심 필드만 중요할 때**

### 💡 느낀 점 / 배운 점:

- Matcher 선택이 테스트 품질을 결정한다
- “모두 비교”보다 “의미 있는 검증”이 더 중요
- `toMatchObject`는 실무 API 테스트의 필수 무기
- 테스트는 **검증이자 문서**라는 느낌을 받음

### 🏷️ 키워드 (태그):

`Jest` `Matcher` `toBe` `toStrictEqual` `toMatchObject` `UniTest` `API테스트`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-01-20 | Jest Matcher | 값·객체·부분 비교 전략 이해 | 할인 계산기, 객체 비교 | Matcher 선택 중요 | `Jest` `Matcher` | ✅ |
