# Git 흐름 한눈에 보기


### 📅 날짜:
> 2025.07.08 (화)

### 📘 오늘 공부한 주제:
> Git의 기본 개념과 명령어 요약 (버전 관리, 브랜치, 커밋, 충돌, stash 등)

---

### 📝 핵심 개념 요약:
- **Git**: 코드 버전 관리 도구, 이전 버전으로 복원 가능, 작업 히스토리 추적
- **Repository**: 변경 기록과 커밋이 저장되는 프로젝트 공간
- **Commit**: 특정 시점의 변경 기록, `git commit -m "메시지"`
- **Add / Staging Area**: `git add`로 커밋 대상으로 지정
- **Branch**: 코드 흐름을 나눠 개발 (ex: dev, main)
- **Merge & Conflict**: 브랜치 병합 / 충돌 해결 후 커밋 필요
- **Reset / Revert / Checkout**: 버전 되돌리기, 이동, 최신 커밋 수정
- **Push / Pull / Fetch**: 로컬과 리모트 간 동기화
- **Tag / Blame / Log / Diff / Alias**: 커밋 관리, 기록 확인, 별칭 설정
- **Stash**: 작업 임시 저장(stack 구조), apply/pop으로 적용/삭제
- **Rebase vs Merge**: 히스토리 유지 vs 병합 정보 유지

### 📊 Git 상태 & 명령어 핵심 표

#### 🔹 파일 상태 흐름
| 상태 | 설명 |
| --- | --- |
| untracked | Git이 추적하지 않는 파일 |
| modified | 수정된 파일 |
| staged | 커밋 대상 대기 상태 |
| committed | 저장소에 저장된 상태 |

#### 🔹 Reset 옵션 요약
| 옵션 | Working Directory | Staging Area | 설명 |
| --- | --- | --- | --- |
| --soft | 유지됨 | 유지됨 | HEAD만 이동 |
| --mixed | 유지됨 | 취소됨 | 스테이징 취소 |
| --hard | 초기화됨 | 초기화됨 | 강제 되돌리기 |

### 💻 실습 내용 정리:
- `git init`, `add`, `commit`, `log`, `status` 기본 실습
- 브랜치 생성/이동, `merge`, `conflict` 상황 재현 후 수동 해결
- `git stash`, `apply`, `pop`, `drop` 순서대로 실습 및 복원
- `reset`, `revert`, `checkout`, `cherry-pick` 기능 비교 실습

### ❗ 헷갈렸던 점 / 문제 해결:
- `reset` 옵션 구분 혼동 → 표로 정리해서 명확히 이해
- `stash`로 저장 후 올바른 브랜치에서 `apply` 잊어서 초기화됨 → 순서 숙지 필요
- `rebase`와 `merge`의 차이 구분 어려움 → 기록 히스토리 목적에 따라 선택

### 💡 느낀 점 / 배운 점:
- Git은 단순 저장이 아니라 작업 흐름을 체계적으로 관리하게 해줌
- `stash`, `revert`, `rebase`는 실무에서 매우 유용하게 사용될 기능들
- 명령어들은 의미와 흐름을 구조화하면 복잡하지 않게 정리 가능함

### 🏷️ 키워드 (태그):
`#Git` `#버전관리` `#stash` `#브랜치` `#커밋관리` `#rebase` `#reset`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-07-08 | Git 버전 관리 기초 | Git 기본 명령어, reset/stash 등 실습 | commit, merge, stash 등 다양한 흐름 연습 | reset 옵션, stash 적용 시기 혼동 해결 | #Git #stash #브랜치 | ✅ |
