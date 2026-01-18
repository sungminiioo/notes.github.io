# React 파일 인풋과 useEffect 사이드 이펙트 정리

### 📅 날짜:

> 2025.08.11 (월)
> 

### 📘 오늘 공부한 주제:

> React 파일 인풋 다루기 (파일 선택, ref 사용, 초기화)
> 

---

### 📝 핵심 개념 요약:

| 주제 | 핵심 내용 |
| --- | --- |
| 파일 인풋 기본 | `<input type="file" />`로 파일 선택 가능, `accept="image/*"`로 확장자 제한, 선택 파일은 `e.target.files[0]`로 접근 |
| 파일 인풋 ref 사용 | `useRef`로 파일 인풋 DOM 직접 참조, 초기화 시 `ref.current.value = ''` 로 초기화 가능 |
| 이미지 미리보기 | `URL.createObjectURL(file)`로 선택한 이미지 임시 URL 생성 → `<img src={url} />`로 미리보기 |
| 사이드 이펙트 (Side Effect) | 함수 외부 상태 변경, DOM 조작, 네트워크 호출 등 부수 작용을 의미 |
| useEffect 활용 | 사이드 이펙트 안전 관리, 컴포넌트 렌더링과 분리하여 예측 가능 코드 작성, 의존성 배열로 동기화 대상 제어 |
| 정리 함수 (Cleanup) | useEffect 내부에서 리턴하는 함수로 타이머 정리, 메모리 해제 등 부수 효과 뒷정리 담당 |

### 💻 실습 내용 정리

---

### 1. 파일 인풋에서 선택한 파일 저장 및 accept 제한

```jsx
const [file, setFile] = useState(null);

<inputtype="file"
  accept="image/*"
  onChange={e => setFile(e.target.files[0])}
/>
```

---

### 2. 파일 인풋 ref로 직접 DOM 접근 및 초기화

```jsx
const inputRef = useRef(null);

<input type="file" ref={inputRef} />

// 파일 초기화
const resetFileInput = () => {
  if (inputRef.current) {
    inputRef.current.value = '';
  }
};
```

---

### 3. 이미지 파일 미리보기

```jsx
const [preview, setPreview] = useState(null);

useEffect(() => {
  if (!file) return;
  const url = URL.createObjectURL(file);
  setPreview(url);

  return () => URL.revokeObjectURL(url); // 메모리 해제
}, [file]);

return (
  <>
    {preview && <img src={preview} alt="preview" />}
  </>
);
```

---

### 4. 사이드 이펙트 예시 - 타이머 및 useEffect 정리 함수

```jsx
useEffect(() => {
  const timerId = setInterval(() => {
    console.log('타이머 실행중...');
  }, 1000);

  return () => {
    clearInterval(timerId);
    console.log('타이머 정리 완료');
  };
}, []);
```

### ❗ 헷갈렸던 점 / 문제 해결:

- `ref.current` 값이 `null`일 수도 있어 반드시 조건 검사 후 접근 필요
- `URL.createObjectURL` 사용 시 메모리 누수 방지를 위해 `URL.revokeObjectURL`을 꼭 호출해야 함
- useEffect에서 사이드 이펙트를 잘못 다루면 예기치 않은 동작이나 메모리 누수가 발생할 수 있음

### 💡 느낀 점 / 배운 점:

- React에서 DOM 직접 조작이 필요한 경우 `useRef`가 매우 유용함
- 이미지 미리보기 구현 시 메모리 관리가 중요함을 깨달음
- useEffect는 사이드 이펙트를 분리해 코드의 예측 가능성과 안정성을 높여줌
- 정리 함수는 특히 타이머, 이벤트 리스너 등 리소스 해제에 필수적임

### 🏷️ 키워드 (태그):

`#React` `#파일인풋` `#useRef` `#이미지미리보기` `#useEffect`  `#사이드이팩트`  `#정리함수`  `#Cleanup`  `#타이머` 

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-08-11 | React 파일 인풋 및 사이드 이펙트 관리 | 파일 선택, ref 초기화, 이미지 미리보기, useEffect 정리 함수 | 파일 인풋, ref, 이미지 미리보기, 타이머 정리 코드 | ref null 체크, 메모리 해제, useEffect 안정적 사용법 |  `#React` `#useRef` `#useEffect` | ✅ |
