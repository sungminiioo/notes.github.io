# PostgreSQL 실행계획 읽기 실전 정리

### 📅 날짜:

> 2025.01.29 (목)
> 

### 📘 오늘 공부한 주제:

> EXPLAIN ANALYZE를 활용한 PostgreSQL 실행계획 실제 분석 방법
> 

---

## 📝 핵심 개념 요약

- **EXPLAIN vs EXPLAIN ANALYZE**
    - `EXPLAIN`: DB가 예상한 실행 경로만 확인 (실행 ❌)
    - `EXPLAIN ANALYZE`: **실제로 쿼리 실행 후** 단계별 소요 시간 측정 (실행 ⭕)
- **실행계획은 안쪽 → 바깥쪽 순서로 읽는다**
    - 가장 깊게 들여쓰기 된 노드부터 시작
- **예상(Cost)과 실제(actual time)는 다를 수 있다**
    - 통계 정보가 오래됐거나 데이터 분포가 달라지면 차이 발생
- **Filter가 많으면 인덱스 부족 신호**
    - Seq Scan + Filter 조합은 성능 개선 포인트

## 📊 핵심 요약 표

| 구분 | 핵심 내용 |
| --- | --- |
| EXPLAIN | 실행 없이 예상 실행계획만 확인 |
| EXPLAIN ANALYZE | 실제 실행 + 단계별 실제 시간 측정 |
| actual time | 실제 걸린 시간(ms) |
| loops | 해당 작업이 반복된 횟수 |
| 실행 순서 | 안쪽 → 바깥쪽 (들여쓰기 기준) |
| Filter | 인덱스 미활용 가능성 신호 |

### 💻 실습 내용 정리

```jsx
EXPLAIN ANALYZE
SELECT * FROM users WHERE age < 80;
```

- 실제 쿼리를 실행하고 각 단계의:
    - `actual time`
    - `rows`
    - `loops`
        
        를 확인
        
- 예상 Cost와 실제 수행 시간이 얼마나 차이나는지 비교
- `Seq Scan + Filter` vs `Index Scan / Index Only Scan` 여부 확인

### ❗ 헷갈렸던 점 / 문제 해결:

- ❓ **Cost가 낮은데 왜 느릴까?**
    - Cost는 **예상값**, 실제 성능은 `actual time` 기준으로 판단해야 함
- ❓ **Seq Scan은 무조건 나쁜가?**
    - ❌ 아님
        
        → 데이터가 적거나 대부분의 데이터를 읽을 땐 더 빠를 수 있음
        
- ❗ `EXPLAIN ANALYZE`는 실제 실행됨
    
    → `DELETE`, `UPDATE`에 사용 시 주의
    

### 💡 느낀 점 / 배운 점:

- 실행계획은 **DB의 속마음**
- 인덱스 설계가 성능을 거의 결정함
- “느리다”는 감이 아니라 **숫자로 확인하는 습관**이 중요
- Cost보다 **actual time을 믿어야 실전 대응 가능**

### 🏷️ 키워드 (태그):

`PostgreSQL` `ExecutionPlan` `EXPLAIN_ANALYZE` `DB성능`

`IndexScan` `SeqScan` `QueryTuning` `Backend`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-01-29 | 실행계획 실전 분석 | EXPLAIN ANALYZE로 예상과 실제 비교 | actual time·loops 확인 | Cost보다 실제 시간이 중요 | `EXPLAIN_ANALYZE``DB성능` | ✅ |
