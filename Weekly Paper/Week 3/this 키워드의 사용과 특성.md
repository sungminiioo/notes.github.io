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

# 💡 `this` 키워드 예제 코드

```javascript
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


### ⚠️ 주의:

- `this`는 **선언 시점이 아니라 호출 시점**에 결정됩니다.
- 화살표 함수는 `this`가 **렉시컬하게 결정**됩니다 → 자신만의 `this`를 갖지 않음.
