# React 기초 다지기


### 📅 날짜:

> 2025.07.31 (목)
> 

### 📘 오늘 공부한 주제:

> React - children, state, 컴포넌트의 장점, 렌더링 방식, 스타일 적용
> 

---

### 📝 핵심 개념 요약:

- **children**

부모 컴포넌트가 자식 컴포넌트에게 전달하는 내용. `props.children`을 통해 렌더링.

- **state**
    
    컴포넌트 내부에서 사용하는 데이터 저장소. 값이 바뀌면 자동으로 렌더링이 다시 이루어짐.
    
- **참조형 state**
    
    배열, 객체와 같은 복잡한 구조의 데이터. 직접 수정 시 React가 감지하지 못하므로 새로운 객체로 갱신해야 함.
    
- **컴포넌트 장점**
    1. 재사용성 높음
    2. 모듈화 용이
    3. 유지 보수 간편
    4. 오류 수정 쉬움
    5. 협업에 유리
- **React 렌더링 방식**
    1. state나 props가 바뀌면 컴포넌트가 다시 렌더링됨
    2. Virtual DOM 사용
    3. 변경된 부분만 실제 DOM에 반영 (Reconciliation)
    4. 렌더링 최적화 가능 (React.memo, shouldComponentUpdate)
- **스타일 적용 방식**
    - 인라인 스타일: `style` 객체 사용
    - 클래스 이름: 외부 CSS 파일을 불러와 className으로 지정

### 📌 핵심 요약 표

| 항목 | 내용 |
| --- | --- |
| children | 부모 → 자식으로 JSX 내용 전달 |
| state | 컴포넌트의 동적인 데이터 관리 |
| 참조형 state | 배열/객체는 새로 만들어야 변경 감지 |
| 컴포넌트 장점 | 재사용성, 모듈화, 유지 보수 용이 |
| 렌더링 방식 | Virtual DOM, 변경 감지 및 최소 업데이트 |
| 스타일 방식 | 인라인 스타일 vs CSS 파일(className) |

### 💻 실습 내용 정리

- **children 사용 예시**
    
    ```jsx
    function Parent() {
      return (
        <Child>
          <p>자식 컴포넌트 내용</p>
        </Child>
      );
    }
    
    function Child(props) {
      return (
        <div>
          <h1>이곳이 자식 컴포넌트</h1>
          <div>{props.children}</div>
        </div>
      );
    }
    ```
    
- **참조형 state 갱신**
    
    ```jsx
    const [items, setItems] = useState([]);
    
    // ❌ 이렇게 하면 React가 감지 못함
    items.push("새 항목");
    setItems(items);
    
    // ✅ 새로운 배열로 갱신해야 함
    setItems([...items, "새 항목"]);
    ```
    
- **인라인 스타일**
    
    ```jsx
    const style = {
      color: 'red',
      backgroundColor: 'lightgray'
    };
    
    return <div style={style}>Hello</div>;
    ```
    
- **외부 CSS 클래스 적용**
    
    ```css
    /* style.css */
    .container {
      background-color: blue;
    }
    ```
    
    ```jsx
    import './style.css';
    
    return <div className="container">Box</div>;
    ```
    

### ❗ 헷갈렸던 점 / 문제 해결:

- 참조형 state를 직접 수정했을 때 화면이 갱신되지 않음

→ spread 문법([...])으로 새로운 배열을 만들어 해결

- children은 JSX로 넘겨주는 구조라는 개념 이해에 시간 소요
    
    → props.children으로 처리하는 구조 연습
    

### 💡 느낀 점 / 배운 점:

- `children`은 매우 유연한 방식으로 부모가 자식에게 내용을 전달할 수 있음
- React는 참조형 데이터를 처리할 때 항상 새로운 객체를 만들어야 변화를 감지함
- 컴포넌트를 잘게 나누면 유지 보수가 쉬워지고 협업에도 효과적임
- JSX와 스타일을 함께 관리하는 방법의 장단점을 알게 됨

### 🏷️ 키워드 (태그):

`#React` `#props` `#children` `#state` `#참조형State` `#컴포넌트장점` `#렌더링` `#가상DOM` `#인라인스타일` `#CSS클래스`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-07-31 | children, state, 컴포넌트의 장점, 렌더링 방식 | props.children, state 개념, 컴포넌트의 장점, React 렌더링 흐름 및 스타일 방식 정리 | children 구조 사용, 참조형 state 업데이트 실습, 인라인/클래스 스타일 적용 | 참조형 상태 직접 수정 시 문제 해결, children 구조 이해 |  `#React` `#props` `#children` `#state`             `#렌더링`           `#컴포넌트장점`   `#스타일링`  | ✅ |
