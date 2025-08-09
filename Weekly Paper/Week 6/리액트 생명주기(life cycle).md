## **React 생명주기(Lifecycle)란?**

> 리액트 컴포넌트가 **“생성 → 업데이트 → 제거”**되는 과정을 말합니다.
> 

---

## 🧩 **크게 3단계로 구분**

| 단계 | 설명 |
| --- | --- |
| ① 마운트(Mount) | 컴포넌트가 처음 생성되어 화면에 나타날 때 |
| ② 업데이트(Update) | props나 state가 바뀌어 다시 렌더링 될 때 |
| ③ 언마운트(Unmount) | 컴포넌트가 화면에서 사라질 때 (제거) |

---

## 🔁 **클래스 컴포넌트의 생명주기 메서드**

| 단계 | 메서드명 | 설명 |
| --- | --- | --- |
| 마운트 | `constructor()` | 초기값 설정 (state 등) |
| 마운트 | `componentDidMount()` | **처음 렌더링 후 1회 실행** (API 호출, 구독 등) |
| 업데이트 | `shouldComponentUpdate()` | 렌더링 여부 결정 (성능 최적화) |
| 업데이트 | `componentDidUpdate()` | 컴포넌트가 업데이트된 후 실행 |
| 언마운트 | `componentWillUnmount()` | 컴포넌트 제거 전 실행 (정리 작업 등) |

---

## ⚛️ **함수형 컴포넌트에서는? → `useEffect`로 처리**

```jsx
import { useEffect } from 'react';

function MyComponent() {
  useEffect(() => {
    console.log('마운트 또는 업데이트됨');

    return () => {
      console.log('언마운트됨');
    };
  }, []); // 의존성 배열에 따라 동작 방식 달라짐
}

```

| 의존성 배열 | 실행 시점 |
| --- | --- |
| 없음 `()` | 마운트 + 모든 업데이트 |
| 빈 배열 `[]` | 마운트 시 1회만 |
| `[value]` | `value`가 바뀔 때마다 실행 |

---

## ✅ **핵심 정리**

| 단계 | 클래스 컴포넌트 | 함수형 컴포넌트 |
| --- | --- | --- |
| 마운트 | `componentDidMount()` | `useEffect(() => {}, [])` |
| 업데이트 | `componentDidUpdate()` | `useEffect(() => {}, [deps])` |
| 언마운트 | `componentWillUnmount()` | `useEffect(() => { return () => {} }, [])` |
