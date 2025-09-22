# React에서 컴포넌트란?

### **정의**

> 컴포넌트는 UI를 독립적이고 재사용 가능한 단위로 나눈 리액트의 기본 구성 요소입니다.
> 
> 
> 하나의 컴포넌트는 **상태(state)**와 **속성(props)**을 받아 특정 UI를 렌더링합니다.
> 

### **특징**

- **재사용성**: 한 번 만든 컴포넌트를 여러 곳에서 사용 가능
- **유지보수성**: 코드가 분리되어 관리가 쉬움
- **조합 가능성**: 작은 컴포넌트를 모아 복잡한 UI를 구성

---

## **함수형 컴포넌트 vs 클래스 컴포넌트**

| 구분 | **함수형 컴포넌트** | **클래스 컴포넌트** |
| --- | --- | --- |
| **작성 방식** | 함수로 정의 | `class` 키워드로 정의 |
| **상태 관리** | `useState`, `useEffect` 등 훅(Hooks) 사용 | `this.state`와 `setState` 사용 |
| **라이프사이클** | 훅으로 대체 (`useEffect` 등) | `componentDidMount`, `componentDidUpdate` 등 메서드 사용 |
| **코드 길이** | 짧고 간결 | 상대적으로 길고 복잡 |
| **권장 여부** | 현재 리액트에서 권장 | 구식 방식, 점점 사용 줄어듦 |

**핵심 정리**

> 함수형 컴포넌트가 현대 리액트의 표준.
> 
> 
> 코드가 간결하고, Hooks로 상태와 라이프사이클 관리가 편리함.
>

# 모범 답안

## React에서 컴포넌트란 무엇인가?

React에서 **컴포넌트**는 UI를 구성하는 독립적이고 재사용 가능한 부품입니다. 컴포넌트는 애플리케이션의 인터페이스를 작은 조각으로 나누어 관리할 수 있게 해주며, 각각의 컴포넌트는 자신의 상태와 UI를 관리합니다.

React에서 컴포넌트는 두 가지 유형으로 나뉩니다:

- **함수형 컴포넌트 (Functional Component)**
- **클래스 컴포넌트 (Class Component)**

---

## 함수형 컴포넌트와 클래스 컴포넌트의 차이점

### 1. 함수형 컴포넌트

- **정의**: 함수형 컴포넌트는 단순한 JavaScript 함수로 정의됩니다.
- **특징**:
    - 초기에는 상태나 생명주기 메서드를 가지지 않는 단순한 UI 컴포넌트로 사용되었습니다.
    - React 16.8에서 **Hooks**가 도입되면서 상태 관리(`useState`), 생명주기 관리(`useEffect`) 등이 가능해졌습니다.
    - Hooks를 사용함으로써 상태와 생명주기 메서드가 관리 가능해지며, 함수형 컴포넌트는 강력해졌습니다.

```jsx
function MyComponent(props) {
    const [greeting, setGreeting] = useState('Hello');

    useEffect(() => {
        console.log('Component mounted');
        return () => console.log('Component will unmount');
    }, []);

    return <div>{greeting}, {props.name}</div>;
}

```

---

### 2. 클래스 컴포넌트

- **정의**: 클래스 컴포넌트는 ES6 클래스 문법을 사용하여 정의됩니다.
- **특징**:
    - React에서 상태 관리와 생명주기 메서드를 제공하는 원래 방식이었습니다.
    - `this.state`를 사용하여 상태를 관리하고, 생명주기 메서드(`componentDidMount`, `componentDidUpdate`, `componentWillUnmount`)를 사용하여 컴포넌트의 생명 주기를 관리합니다.
    - React Hooks 도입 이후, 대부분의 생명주기 메서드는 함수형 컴포넌트로 대체 가능해졌습니다.

```jsx
class MyComponent extends React.Component {
    constructor(props) {
        super(props);
        this.state = { greeting: 'Hello' };
    }

    componentDidMount() {
        // 컴포넌트가 마운트된 후에 실행됩니다.
    }

    componentDidUpdate(prevProps, prevState) {
        // 컴포넌트가 업데이트된 후에 실행됩니다.
    }

    componentWillUnmount() {
        // 컴포넌트가 언마운트되기 전에 실행됩니다.
    }

    render() {
        return <div>{this.state.greeting}, {this.props.name}</div>;
    }
}

```

---

## 주요 차이점

| **구분** | **함수형 컴포넌트** | **클래스 컴포넌트** |
| --- | --- | --- |
| **정의** | JavaScript 함수로 정의 | ES6 클래스 문법으로 정의 |
| **상태 관리** | Hooks (`useState`) 사용 | `this.state`로 상태 관리 |
| **생명주기 관리** | Hooks (`useEffect`) 사용 | `componentDidMount`, `componentWillUnmount` 등 생명주기 메서드 사용 |
| **코드 길이** | 더 간결하고 이해하기 쉬움 | 코드가 길고 복잡함 |
| **현재 사용 여부** | 최신 React 개발에서 주로 사용 | 레거시 코드나 특수한 경우에 사용 |

---

## 결론

React에서 **컴포넌트**는 UI를 구성하는 독립적이고 재사용 가능한 부품입니다. 최신 React 개발에서는 **함수형 컴포넌트**와 **Hooks**가 주로 사용되며, 이 방식이 더 간결하고 효율적입니다. 함수형 컴포넌트는 Hooks를 활용하여 상태와 생명주기를 관리할 수 있어 코드의 가독성과 재사용성이 높아집니다.

반면, **클래스 컴포넌트**는 이제 잘 사용되지 않으며, 주로 레거시 코드나 특정한 경우에만 사용됩니다. 따라서, 새로운 React 프로젝트에서는 함수형 컴포넌트와 Hooks를 사용하는 것이 권장됩니다.
