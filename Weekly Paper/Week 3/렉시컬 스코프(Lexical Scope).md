# 렉시컬 스코프(Lexical Scope)

### 📘 정의:

> 렉시컬 스코프란 "코드가 작성된 위치(문법 구조)"에 따라 변수의 유효 범위(Scope)가 결정되는 방식입니다.
> 
> 
> 즉, **"어디서 선언되었느냐"**에 따라 접근 가능한 범위가 정해집니다.
> 

### 🔍 주요 특징:

| 특성 | 설명 |
| --- | --- |
| 선언 위치 기준 | 변수를 사용할 수 있는 범위는 **함수가 정의된 위치 기준**으로 결정됨 |
| 실행 시점 무관 | 호출 위치나 실행 시점과는 무관 |
| 중첩 함수 가능 | 내부 함수는 외부 함수의 변수에 접근 가능 |

### 💡 예제 코드:

```jsx
function outer() {
  const a = 10;
  function inner() {
    console.log(a); // 10 - 외부 변수 접근 가능
  }
  inner();
}
outer();

```

# **모범 답안**

### **렉시컬 스코프란?**

렉시컬 스코프는 **변수와 블록 스코프가 코드가 작성된 구조에 따라 결정되는 스코프**의 한 형태입니다.

함수나 블록이 **선언된 시점의 외부 환경**을 기준으로 스코프가 결정되며, 이는 **실행 시점이 아닌 선언 시점**에 결정된다는 의미입니다.

---

### **렉시컬 스코프의 작동 방식**

- **스코프 결정 시점**: 함수가 **정의된 위치**를 기준으로 스코프가 결정됩니다.
- **스코프 체인**: 변수를 참조할 때 내부 스코프 → 외부 스코프로 순차적으로 탐색합니다.
- **상위 스코프 참조**: 내부 함수는 **자신이 정의된 외부 환경**을 참조할 수 있으며, 호출 위치와 관계없이 일관된 스코프를 유지합니다.

---

### **렉시컬 스코프 예시**

```jsx
const outerVar = 'I am outside!';

function outerFunction() {
    const innerVar = 'I am inside!';

    function innerFunction() {
        console.log(outerVar); // 'I am outside!'
        console.log(innerVar); // 'I am inside!'
    }

    innerFunction();
}

outerFunction();

```

- `outerFunction`은 `outerVar`를 참조할 수 있습니다.
- `innerFunction`은 `outerVar`와 `innerVar` 모두를 참조할 수 있습니다.
- 이유: `innerFunction`이 `outerFunction` 내부에서 **선언**되었기 때문에 상위 스코프를 기억합니다.

---

### **클로저와 렉시컬 스코프**

렉시컬 스코프는 **클로저(Closure)**와 밀접한 관련이 있습니다.

클로저는 **함수와 그 함수가 선언될 당시의 렉시컬 환경의 조합**으로, 외부 함수의 변수를 내부 함수에서 접근할 수 있도록 합니다.

```jsx
function makeCounter() {
    let count = 0;

    return function() {
        count++;
        console.log(count);
    }
}

const counter = makeCounter();
counter(); // 1
counter(); // 2

```

- `makeCounter`는 `count` 변수를 갖고 있으며,
- 반환된 함수는 `makeCounter`의 **렉시컬 환경**을 기억해 `count`를 계속 증가시킬 수 있습니다.
- 이러한 특성이 바로 **클로저**입니다.

---

### **결론**

렉시컬 스코프는 함수나 블록이 **선언된 시점**의 외부 환경을 기준으로 스코프를 결정하는 개념입니다.

이 덕분에 자바스크립트에서는 **클로저**와 같은 강력한 기능을 사용할 수 있습니다.
