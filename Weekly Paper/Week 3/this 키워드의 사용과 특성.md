# this 키워드의 사용과 특성

### 📘 정의:

> 자바스크립트에서 this는 **함수가 호출될 때 결정되는 실행 컨텍스트(context)의 주체(=소유자)**를 가리키는 키워드입니다.
> 

즉, **"누가 호출했는지"** 에 따라 `this`가 참조하는 대상이 달라집니다

### 🔍 `this`의 주요 특징:

| 상황 | `this`가 가리키는 대상 |
| --- | --- |
| 일반 함수 호출 | 전역 객체 (`window` or `global`) |
| 객체의 메서드 호출 | 해당 객체 |
| 생성자 함수 | 새로 생성된 인스턴스 |
| 화살표 함수 | 외부(상위) 스코프의 `this` (자신의 `this`를 가지지 않음) |
| 이벤트 핸들러 (브라우저) | 해당 DOM 요소 |
| `call`, `apply`, `bind` 사용 | 지정된 객체 |

### 💡 예제 코드:

```jsx
// 일반 함수
function show() {
  console.log(this); // window (브라우저 환경)
}
show();

// 객체 메서드
const obj = {
  name: 'Alice',
  greet() {
    console.log(this.name); // Alice
  }
};
obj.greet();

// 화살표 함수
const arrowObj = {
  name: 'Bob',
  greet: () => {
    console.log(this.name); // undefined (외부 this → 전역)
  }
};
arrowObj.greet();

```

### ⚠️ 주의:

- `this`는 **선언 시점이 아니라 호출 시점**에 결정됩니다.
- 화살표 함수는 `this`가 **렉시컬하게 결정**됩니다 → 자신만의 `this`를 갖지 않음.

# **모범 답안**

### **자바스크립트에서 `this` 키워드의 사용과 특성**

자바스크립트에서 `this` 키워드는 매우 중요하면서도 혼란스러운 개념 중 하나입니다.

`this`는 **실행 컨텍스트**에 따라 그 값이 결정되며, 현재 실행 중인 함수 또는 메서드의 **"소유자"**를 가리킵니다.

---

### **1. 전역 컨텍스트에서의 `this`**

전역 실행 컨텍스트에서 `this`는 전역 객체를 가리킵니다.

브라우저 환경에서는 `window`, Node.js 환경에서는 `global`이 됩니다.

```jsx
console.log(this); // 브라우저에서는 window 객체 출력

```

---

### **2. 함수 내에서의 `this`**

- **일반 함수**: 기본적으로 전역 객체를 가리킴.
- **엄격 모드('use strict')**: `undefined`.

```jsx
function regularFunction() {
    console.log(this); // window 또는 global 객체
}

function strictFunction() {
    'use strict';
    console.log(this); // undefined
}

regularFunction();
strictFunction();

```

---

### **3. 객체 메서드 내에서의 `this`**

객체의 메서드로서 함수가 호출될 때, `this`는 해당 메서드를 호출한 객체에 바인딩됩니다.

```jsx
const obj = {
    name: 'JavaScript',
    getName: function() {
        console.log(this.name);
    }
};

obj.getName(); // 'JavaScript'

```

---

### **4. 생성자 함수에서의 `this`**

생성자 함수에서 `this`는 새로 생성되는 객체를 가리킵니다.

```jsx
function Person(name) {
    this.name = name;
}

const person = new Person('John');
console.log(person.name); // 'John'

```

---

### **5. 이벤트 핸들러에서의 `this`**

이벤트 핸들러에서 `this`는 이벤트를 받는 요소를 가리킵니다.

```jsx
document.getElementById('myButton').addEventListener('click', function() {
    console.log(this); // 클릭된 버튼 요소
});

```

---

### **6. 화살표 함수에서의 `this`**

화살표 함수는 자체적으로 `this`를 바인딩하지 않고, 생성된 **시점의 실행 컨텍스트**의 `this`를 "상속"받습니다.

```jsx
const obj = {
    name: 'JavaScript',
    regularFunction: function() {
        setTimeout(function() {
            console.log(this.name); // undefined (전역 객체의 name 속성)
        }, 100);
    },
    arrowFunction: function() {
        setTimeout(() => {
            console.log(this.name); // 'JavaScript'
        }, 100);
    }
};

obj.regularFunction();
obj.arrowFunction();

```

위 예시에서 `regularFunction`의 `setTimeout` 콜백은 전역 객체의 `this`를 가리키지만,

`arrowFunction`의 콜백은 **`obj`의 this를 상속**받아 `'JavaScript'`를 출력합니다.
