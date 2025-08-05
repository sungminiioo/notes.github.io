# DOM 조작 및 이벤트 흐름

### 📅 날짜:

> 2025.07.14 (월)
> 

### 📘 오늘 공부한 주제:

> 요소 노드 프로퍼티 / DOM 추가·삭제 / 클래스 & 스타일 / 이벤트 등록 & 흐름
> 

---

### 📝 핵심 개념 요약:

- `innerHTML`, `outerHTML`, `textContent`는 요소 내용을 다루는 기본 프로퍼티
- DOM 요소를 **생성/삽입/삭제**하고, **이벤트 핸들러**로 인터랙션 구현
- `classList`를 통해 클래스를 효율적으로 추가/삭제/토글
- DOM 이벤트는 **캡쳐링 → 타깃 → 버블링** 흐름을 따름

### 📌 주요 프로퍼티 & 메서드 정리

| 항목 | 설명 | 사용 예 |
| --- | --- | --- |
| `innerHTML` | 요소 안의 HTML 반환/설정 | `el.innerHTML = '<p>안녕</p>'` |
| `outerHTML` | 요소 전체 HTML 반환/설정 | `el.outerHTML = '<div>교체</div>'` |
| `textContent` | 텍스트만 반환/설정 | `el.textContent = '텍스트만'` |
| `createElement()` | 새 요소 만들기 | `document.createElement('li')` |
| `append`, `prepend` | 요소 추가 | `parent.append(child)` |
| `before`, `after` | 요소 앞/뒤 추가 | `el.before(newEl)` |
| `remove()` | 요소 제거 | `el.remove()` |
| `setAttribute()` | 속성 설정 | `el.setAttribute('id', 'new')` |
| `removeAttribute()` | 속성 제거 | `el.removeAttribute('class')` |
| `classList.add()` | 클래스 추가 | `el.classList.add('done')` |
| `classList.remove()` | 클래스 제거 | `el.classList.remove('done')` |
| `classList.toggle()` | 토글 | `el.classList.toggle('done')` |

### 🔧 이벤트 핸들러 주요 속성

| 속성 | 설명 |
| --- | --- |
| `type` | 이벤트 타입 (예: `'click'`) |
| `target` | 이벤트가 발생한 대상 |
| `currentTarget` | 이벤트 리스너가 등록된 요소 |
| `timeStamp` | 이벤트 발생 시간 |
| `bubbles` | 버블링 여부 (true/false) |

### 🧩 DOM 이벤트 흐름 요약표

| 단계 | 설명 | 예시 상황 |
| --- | --- | --- |
| 캡쳐링 | 상위 → 하위로 이벤트 전달 | `div → ul → li` 순 |
| 타깃 | 이벤트가 발생한 실제 요소 | `li`에서 클릭 발생 |
| 버블링 | 하위 → 상위로 이벤트 전파 | `li → ul → div` 순 |

### 💻 실습 내용 정리:

- 요소 생성 및 추가
- `classList`를 통한 스타일 제어
- `setAttribute`, `removeAttribute`로 HTML 속성 제어
- `addEventListener`로 클릭 이벤트 등록
- DOM 트리 구조를 따라 이벤트 버블링 확인

### ❗ 헷갈렸던 점 / 문제 해결:

- `outerHTML`은 **요소 자체를 교체**, `innerHTML`은 **내부만 수정**
- `append`/`prepend`는 요소를 옮기는 개념이라 **기존 위치에서 제거됨**
- `className`으로 전체 클래스 교체하면 **기존 클래스 사라짐**
- 이벤트 버블링이 예상치 못한 동작을 유발할 수 있음 → `stopPropagation()` 필요 시 사용

### 💡 느낀 점 / 배운 점:

- DOM을 직접 조작하면 HTML 구조를 자바스크립트로 자유롭게 다룰 수 있음
- 이벤트 흐름 구조 이해는 **복잡한 인터페이스** 구현 시 매우 중요
- 클래스 제어는 `classList`를 사용하는 것이 가장 직관적이고 안전

### 🏷️ 키워드 (태그):

`#DOM조작` `#innerHTML` `#outerHTML` `#classList` `#이벤트버블링` `#캡쳐링단계` `#addEventListener`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-07-14 | DOM 조작 및 이벤트 흐름 | 요소 내용 조작, DOM 생성/삭제, 이벤트 흐름 | `createElement`, `classList`, `addEventListener` | `outerHTML` 주의, 버블링 이해 | `#DOM조작`, `#이벤트버블링`, `#innerHTML` | ✅ |
