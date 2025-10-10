# React 폼 다루기

### 📅 날짜:

> 2025.08.08 (금)
> 

### 📘 오늘 공부한 주제:

> 제어 컴포넌트와 비제어 컴포넌트의 차이
> 

---

### 📝 핵심 개념 요약:

- **제어 컴포넌트**
    - 인풋의 value 값을 **React state**로 제어
    - React에서 지정한 값과 실제 인풋 값이 항상 동일
    - 예측 가능하고 유지보수가 쉬움 → **권장 방식**
- **비제어 컴포넌트**
    - 인풋의 value 값을 React에서 지정하지 않음
    - DOM 직접 참조(FormData, e.target 활용)
    - 특정 상황(파일 업로드)에서만 주로 사용
- **HTML과의 차이점**
    - `onChange`: HTML의 `oninput`과 비슷하지만 React에선 입력 시 즉시 state 변경
    - `htmlFor`: HTML `for` 대신 React에서는 `htmlFor` 사용
- **폼 값 관리 패턴**
    - 개별 state 사용
    - 객체형 state로 name/value 기반 업데이트
- **기본 submit 동작 막기**
    - `e.preventDefault()`로 페이지 새로고침 방지

### 📌 핵심 요약 표

| 구분 | 제어 컴포넌트 | 비제어 컴포넌트 |
| --- | --- | --- |
| value 속성 | React에서 지정 | DOM 기본 값 사용 |
| 데이터 흐름 | 단방향 (React → UI) | 양방향 (UI ↔ DOM) |
| 장점 | 예측 가능, 상태 공유 쉬움 | 구현 간단, 특정 상황에 유리 |
| 단점 | 코드 길어질 수 있음 | 데이터 관리 복잡 |
| 사용 예시 | 일반 입력 폼 | 파일 업로드, 단순 입력 |

### 💻 실습 내용 정리

### 1. 개별 state로 폼 값 관리 (제어 컴포넌트)

```jsx
function TripSearchForm() {
  const [location, setLocation] = useState('Seoul');
  const [checkIn, setCheckIn] = useState('2022-01-01');
  const [checkOut, setCheckOut] = useState('2022-01-02');

  return (
    <form>
      <label htmlFor="location">위치</label>
      <input id="location" value={location} onChange={e => setLocation(e.target.value)} />
      <label htmlFor="checkIn">체크인</label>
      <input id="checkIn" type="date" value={checkIn} onChange={e => setCheckIn(e.target.value)} />
      <label htmlFor="checkOut">체크아웃</label>
      <input id="checkOut" type="date" value={checkOut} onChange={e => setCheckOut(e.target.value)} />
    </form>
  );
}
```

---

### 2. 객체형 state로 폼 값 관리 (제어 컴포넌트)

```jsx
function TripSearchForm() {
  const [values, setValues] = useState({
    location: 'Seoul',
    checkIn: '2022-01-01',
    checkOut: '2022-01-02',
  });

  const handleChange = e => {
    const { name, value } = e.target;
    setValues(prev => ({ ...prev, [name]: value }));
  };

  return (
    <form>
      <label htmlFor="location">위치</label>
      <input id="location" name="location" value={values.location} onChange={handleChange} />
      <label htmlFor="checkIn">체크인</label>
      <input id="checkIn" type="date" name="checkIn" value={values.checkIn} onChange={handleChange} />
      <label htmlFor="checkOut">체크아웃</label>
      <input id="checkOut" type="date" name="checkOut" value={values.checkOut} onChange={handleChange} />
    </form>
  );
}
```

---

### 3. 비제어 컴포넌트 예시

```jsx
function TripSearchForm({ onSubmit }) {
  const handleSubmit = e => {
    e.preventDefault();
    const formData = new FormData(e.target);
    console.log(Object.fromEntries(formData.entries()));
  };

  return (
    <form onSubmit={handleSubmit}>
      <label htmlFor="location">위치</label>
      <input id="location" name="location" placeholder="어디로 여행가세요?" />
      <label htmlFor="checkIn">체크인</label>
      <input id="checkIn" type="date" name="checkIn" />
      <label htmlFor="checkOut">체크아웃</label>
      <input id="checkOut" type="date" name="checkOut" />
      <button type="submit">검색</button>
    </form>
  );
}
```

### ❗ 헷갈렸던 점 / 문제 해결:

- **문제:** 처음에는 `value`를 안 써도 되는 줄 알았는데, React에서는 value를 지정하지 않으면 비제어 컴포넌트가 되어 상태 관리가 어려워짐.
- **해결:** value와 onChange를 반드시 같이 사용해 제어 컴포넌트로 구현 → 예측 가능해짐.

### 💡 느낀 점 / 배운 점:

- 제어 컴포넌트를 쓰면 상태와 UI의 싱크가 보장되어 유지보수성이 높아짐
- 객체형 state를 활용하면 코드가 간결해지고 확장성이 좋음
- 비제어 컴포넌트는 특별한 경우(파일 업로드) 외에는 잘 안 쓰는 게 낫다

### 🏷️ 키워드 (태그):

`#React` `#폼` `#제어컴포넌트` `#비제어컴포넌` `#useState` `#FormData` `#onChange` `#htmlFor` 

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-08-08 | React 폼 다루기 | 제어 vs 비제어, 객체형 state, onChange, htmlFor | 개별 state/객체형 state 관리, 비제어 폼 구현 | 제어 컴포넌트로 상태-UI 싱크 보장, 비제어는 특수 상황만 |  `#React` `#useState` `#폼` `#제어컴포넌트` `#비제어컴포넌트`   | ✅ |
