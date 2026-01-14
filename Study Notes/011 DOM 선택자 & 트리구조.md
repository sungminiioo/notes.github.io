# JS DOM 선택자 & 이벤트 & 트리구조 이해하기

### 📅 날짜:

> 2025.07.11 (금)
> 

### 📘 오늘 공부한 주제:

> JavaScript로 HTML 태그 선택, DOM 트리 순회, 이벤트 핸들링, console 출력 방식 비교
> 

---

### 📝 핵심 개념 요약:

- **DOM (Document Object Model)**: HTML 문서를 객체처럼 구조화한 트리 구조
- **태그 선택 방법**: `getElementById`, `getElementsByClassName`, `querySelector` 등 다양한 방식 존재
- **유사 배열**: `HTMLCollection`, `NodeList`는 배열처럼 보이지만 배열 메소드는 사용 불가
- **이벤트(Event)**: 사용자 상호작용 (클릭, 스크롤 등) 발생 시 코드 실행
- **이벤트 핸들러(Event Handler)**: 이벤트 발생 시 동작을 정의한 코드
- **DOM 트리 탐색**: 부모, 자식, 형제 노드를 통해 구조적으로 접근 가능

### 📌 주요 개념 표 정리

### 🔎 태그 선택 메소드 비교

| 메소드 | 설명 | 결과 형태 |
| --- | --- | --- |
| `getElementById('id')` | ID로 요소 선택 | 단일 요소 |
| `getElementsByClassName('class')` | Class로 요소 선택 | HTMLCollection (유사 배열) |
| `getElementsByTagName('tag')` | 태그 이름으로 선택 | HTMLCollection |
| `querySelector('css')` | CSS 선택자 사용 (첫 번째 요소) | 단일 요소 |
| `querySelectorAll('css')` | CSS 선택자 사용 (전체 요소) | NodeList (유사 배열) |

### ⚙️ 이벤트 구성 요소

| 용어 | 설명 |
| --- | --- |
| 이벤트 | 사용자 동작 (ex. 클릭, 입력 등) |
| 이벤트 핸들링 | 이벤트 발생 시 처리하는 방식 |
| 이벤트 핸들러 | 이벤트 발생 시 실행되는 함수 |
| 전역 객체 `window` | 브라우저 전체를 대표하는 객체 |

### 🧭 DOM 트리 탐색 메소드 (요소 노드 전용)

| 메소드 | 설명 |
| --- | --- |
| `children` | 자식 요소들 |
| `firstElementChild` | 첫 번째 자식 요소 |
| `lastElementChild` | 마지막 자식 요소 |
| `parentElement` | 부모 요소 |
| `previousElementSibling` | 이전 형제 요소 |
| `nextElementSibling` | 다음 형제 요소 |

### 🌿 DOM 트리 탐색 메소드 (모든 노드)

| 메소드 | 설명 |
| --- | --- |
| `childNodes` | 모든 자식 노드 |
| `firstChild` | 첫 자식 노드 |
| `lastChild` | 마지막 자식 노드 |
| `parentNode` | 부모 노드 |
| `previousSibling` | 이전 형제 노드 |
| `nextSibling` | 다음 형제 노드 |

### 🧪 콘솔 출력 메소드 비교

| 메소드 | 특징 |
| --- | --- |
| `console.log()` | 값 자체를 출력 |
| `console.dir()` | 객체의 속성을 자세히 출력 (트리 형태) |

### 💻 실습 내용 정리:

- 다양한 방식으로 HTML 요소 선택 후 `console.log`로 확인
- 버튼 클릭 시 알림창 띄우는 `alert()`와 이벤트 핸들링 실습
- DOM 트리 탐색 메소드(`parentElement`, `children`, `nextElementSibling` 등)로 구조 확인

### ❗ 헷갈렸던 점 / 문제 해결:

- `getElementById()`는 존재하지 않는 ID를 선택하면 `null` 반환
- `getElementsByClassName()`은 결과가 유사 배열이라 배열 메소드는 사용 불가
- `querySelectorAll()`은 NodeList를 반환하며, 각 요소를 `[]` 인덱스로 접근 가능

### 💡 느낀 점 / 배운 점:

- DOM 트리 구조를 이해하고 콘솔로 직접 확인해보니 구조 이해가 빨라짐
- 이벤트 처리 흐름(정의 → 선택 → 실행)을 체계적으로 익힐 수 있었음
- `querySelector()`와 `querySelectorAll()` 사용법을 정확히 익힘

### 🏷️ 키워드 (태그):

`#DOM선택자` `#유사배열` `#이벤트핸들링` `#DOM탐색` `#콘솔출력` `#자바스크립트기초`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-07-11 | JS DOM 선택 & 이벤트 | DOM 선택자, 트리 구조, 이벤트 처리 | 태그 선택, 이벤트 바인딩, 트리 탐색 | 유사배열/console.dir 이해 | #DOM #이벤트 #기초JS | ✅ |
