# Mongoose & MongoDB CRUD 정리

### 📅 날짜:

> 2025.08.19 (화)
> 

### 📘 오늘 공부한 주제:

> Mongoose 스키마, 모델, CRUD 연산, 데이터 조회/수정/삭제, 비동기 오류 처리
> 

---

## 📝 핵심 개념 요약

- **Mongoose 스키마(Schema)**: 컬렉션의 구조를 정의. 필드 타입, 필수 여부, 기본값, 유효성 검사 설정 가능
- **모델(Model)**: 스키마 기반으로 DB와 상호작용하는 인터페이스(`mongoose.model`)
- **CRUD 연산**: Create(.create), Read(.find, .findById), Update(.findByIdAndUpdate, .save), Delete(.findByIdAndDelete)
- **비동기 처리**: 모든 Mongoose 메서드는 async/await 필요, try/catch로 오류 처리 필수
- **유효성 검사**: required, enum, min/max, match, 커스텀 validator 사용 가능
- **정렬/제한**: .sort(), .limit()로 조회 결과 정렬 및 제한 가능
- **시드 데이터**: 초기 데이터를 DB에 삽입하여 테스트 용도로 활용 가능

## 📊 핵심 요약 표

| 구분 | 내용 | 예시 |
| --- | --- | --- |
| 스키마 정의 | 컬렉션 구조, 필드 타입, 기본값, 유효성 | `title: { type: String, required: true }` |
| 모델 생성 | 스키마 기반 DB 인터페이스 | `const Task = mongoose.model('Task', TaskSchema)` |
| 시드 데이터 | 초기 DB 데이터 삽입 | `Task.insertMany(data)` |
| 조회(Read) | 전체, 조건, 단일, 첫 문서 | `.find() / .find({isComplete:true}) / .findById(id) / .findOne({title:'공부'})` |
| 정렬/제한 | .sort(), .limit() 체이닝 | `.find().sort({createdAt:-1}).limit(5)` |
| 생성(Create) | 새 도큐먼트 생성 및 저장 | `const task = new Task(req.body); await task.save()` |
| 수정(Update) | findByIdAndUpdate / save | `{new:true, runValidators:true}` |
| 삭제(Delete) | findByIdAndDelete | `res.sendStatus(204)` |
| 오류 처리 | try/catch 필수 | asyncHandler(), res.status(404) |

### 💻 실습 내용 정리

1. **스키마 정의**

```jsx
import mongoose from 'mongoose';

const TaskSchema = new mongoose.Schema({
  title: { type: String, required: true, maxLength: 30 },
  description: { type: String },
  isComplete: { type: Boolean, default: false, required: true }
}, { timestamps: true });

const Task = mongoose.model('Task', TaskSchema);
export default Task;
```

1. **시드 데이터 넣기**

```jsx
const tasks = [
  { title: '공부', description: 'React 공부', isComplete: false },
  { title: '운동', description: '조깅 30분', isComplete: true },
];
await Task.insertMany(tasks);
console.log('Seed data inserted');
```

1. **데이터 조회**

```jsx
const allTasks = await Task.find();
const doneTasks = await Task.find({ isComplete: true });
const task = await Task.findById(id);
const firstTask = await Task.findOne({ title: '공부' });
const recentTasks = await Task.find().sort({ createdAt: -1 }).limit(5)
```

1. **데이터 생성 & 유효성 검사**

```jsx
try {
  const task = new Task(req.body);
  await task.save();
} catch(err) {
  res.status(400).send(err);
}
```

1. **데이터 수정**

```jsx
const updatedTask = await Task.findByIdAndUpdate(
  id,
  req.body,
  { new: true, runValidators: true }
);
if(!updatedTask) return res.status(404).send({ message: 'Cannot find given id.' });
res.send(updatedTask);
```

1. **데이터 삭제**

```jsx
const task = await Task.findByIdAndDelete(id);
if(task) res.sendStatus(204);
else res.status(404).send({ message: 'Cannot find given id.' });
```

1. **비동기 오류 처리**

```jsx
function asyncHandler(handler) {
  return async (req, res) => {
    try {
      await handler(req, res);
    } catch(e) {
      res.status(500).send({ message: e.message });
    }
  };
}
```

### ❗ 헷갈렸던 점 / 문제 해결:

- findByIdAndUpdate, findByIdAndDelete 모두 **비동기**이므로 반드시 await와 try/catch 필요
- Validation 옵션(runValidators) 설정 안 하면 일부 필드 유효성 검사가 적용되지 않음
- 없는 ID 조회 시 404 처리 필수

### 💡 느낀 점 / 배운 점:

- Mongoose는 스키마 기반이라 DB 구조를 코드로 관리할 수 있어 안정적
- CRUD 메서드와 async/await, try/catch 활용법을 반드시 숙지해야 서버 오류 방지 가능
- 시드 데이터를 활용하면 테스트와 초기 DB 설정이 편리

### 🏷️ 키워드 (태그):

`#MongoDB` `#Mongoose` `#CRUD` `#Schema` `#Model` `#Validation` `#AsyncAwait` `#NodeJS` 

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-08-19 | Mongoose & MongoDB CRUD | 스키마, 모델, CRUD, 유효성 검사, 비동기 처리 | 스키마 정의, 시드 데이터 삽입, 조회, 생성, 수정, 삭제, asyncHandler | runValidators 옵션 확인, 없는 ID 404 처리 | `#MongoDB` `#Mongoose` `#CRUD` `#Schema` 
  | ✅ |
