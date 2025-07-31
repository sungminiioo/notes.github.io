# Virtual DOM이란?

## **정의**

> Virtual DOM은 실제 DOM을 메모리에 가볍게 복사한 가상 트리입니다.
> 
> 
> 리액트는 UI를 직접 수정하지 않고 Virtual DOM에서 먼저 변경 사항을 계산한 뒤, 필요한 부분만 실제 DOM에 반영합니다.
> 

---

## **왜 사용할까?**

1. **빠른 성능**
    - 실제 DOM을 직접 조작하면 느림 → Virtual DOM으로 미리 계산 후 필요한 부분만 반영.
2. **효율적인 렌더링**
    - 이전 Virtual DOM과 새로운 Virtual DOM을 **비교(diffing)**해서 바뀐 부분만 업데이트.
3. **개발 편의성**
    - 상태(state)만 바꾸면 리액트가 알아서 화면을 최신 상태로 유지.
4. **예측 가능한 UI**
    - 상태 기반으로 렌더링되어 코드가 간결하고 버그가 줄어듦.

---

## **핵심 포인트**

- **"가볍고 빠른 가상 화면 설계도"**
- **변경된 부분만 실제 DOM에 반영** → 성능 최적화
- **상태 기반 렌더링**으로 **개발 효율** 상승

# **모범 답안**

### **리액트에서 Virtual DOM이 무엇인지, 이를 사용하는 이유**

리액트(React)는 **UI 라이브러리**로, **Virtual DOM**을 활용하여 효율적인 렌더링을 수행합니다.

---

### **Virtual DOM의 개념**

- *Virtual DOM(VDOM)**은 UI의 가상 표현을 메모리에 저장하고, ReactDOM과 같은 라이브러리를 통해 **실제 DOM과 동기화**하는 개념입니다.
- **실제 DOM의 가벼운 복사본**으로, 상태가 변경될 때마다 새로운 Virtual DOM을 생성하고 이전 Virtual DOM과 비교해 실제 DOM에 반영합니다.

---

### **Virtual DOM의 작동 원리**

1. **초기 렌더링**:
    - 컴포넌트가 처음 렌더링될 때 JSX가 Virtual DOM으로 변환되어 메모리에 저장됩니다.
2. **상태 변화**:
    - state나 props가 변경되면 새로운 Virtual DOM 트리가 생성됩니다.
3. **Diffing 알고리즘**:
    - 이전 Virtual DOM과 새로운 Virtual DOM을 비교하여 변경된 부분만 식별합니다.
4. **패치 과정**:
    - 필요한 부분만 실제 DOM에 업데이트하여 **불필요한 연산을 줄이고 성능을 최적화**합니다.

---

### **리액트에서 Virtual DOM을 사용하는 이유**

### **1. 성능 최적화**

- **Reflow와 Repaint 최소화**: 실제 DOM 조작은 연산 비용이 크기 때문에 Virtual DOM으로 이를 줄입니다.
- **배치 업데이트**: 여러 변경 사항을 한 번에 DOM에 반영하여 효율을 높입니다.

### **2. 개발 편의성**

- **선언적 프로그래밍**: 개발자는 UI의 현재 상태를 선언적으로 정의하고, 리액트가 DOM을 자동으로 업데이트합니다.
- **가독성 및 유지보수 향상**: 복잡한 DOM 조작 로직이 줄어들어 코드가 더 명확해집니다.

---

### **예제: Virtual DOM 작동 원리**

```jsx
class Counter extends React.Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 };
  }

  increment = () => {
    this.setState({ count: this.state.count + 1 });
  };

  render() {
    return (
      <div>
        <p>{this.state.count}</p>
        <button onClick={this.increment}>Increment</button>
      </div>
    );
  }
}

ReactDOM.render(<Counter />, document.getElementById('root'));

```

- `increment()` 실행 시 `setState()`가 호출되어 **새 Virtual DOM 생성**.
- 이전 Virtual DOM과 비교(diff) 후 **변경된 부분만 실제 DOM에 반영**.

---

### **결론**

Virtual DOM은 리액트가 **효율적인 UI 렌더링**을 위해 사용하는 핵심 개념입니다.

메모리에서 UI의 가상 표현을 먼저 업데이트한 뒤, 필요한 최소한의 변경만 실제 DOM에 반영하여 **성능을 최적화**하고, **선언적 프로그래밍**을 통해 개발 경험을 개선합니다.
