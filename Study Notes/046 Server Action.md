# Server Action

### 📅 날짜:

> 2025.09.09 (화)
> 

### 📘 오늘 공부한 주제:

> Next.js에서 서버 환경에서 실행되는 async 함수(Server Action) 이해 및 활용
> 

---

## 📝 핵심 개념 요약

- **Server Action 정의**
- 서버 환경에서 실행되는 비동기 함수
- `'use server'` 지시어 사용
- 폼 제출, 데이터 변경, DB 조작 등 서버 로직 처리에 적합
- **Server Action 장점**
    1. 보안 강화: 클라이언트에 민감 정보 노출 없음
    2. 개발자 경험 개선: 별도 API 엔드포인트 관리 불필요
    3. 번들 최적화: 서버액션 코드는 클라이언트 JS 번들에 포함되지 않음
- **Server Action 단점**
    1. 실시간 유효성 검사 필요 시 적합하지 않을 수 있음
    2. 중간 서버 경유로 API 요청 지연 발생 가능 (UX 저하)

## 📊 핵심 요약 표

| 구분 | 정의 | 사용 사례 | 장점 | 단점 |
| --- | --- | --- | --- | --- |
| Server Action | 서버에서 실행되는 async 함수 | 폼 제출, DB 조작, 서버 데이터 처리 | 보안 강화, 별도 API 불필요, 번들 최적화 | 실시간 유효성 검사 어려움, API 레이턴시 증가 가능 |

### 💻 실습 내용 정리

- **폼 제출용 Server Action**
    
    ```jsx
    export default function Page() {
      async function createInvoice(formData) {
        'use server'
    
        const rawFormData = {
          customerId: formData.get('customerId'),
          amount: formData.get('amount'),
          status: formData.get('status'),
        }
    
        await db.invoice.create({ data: rawFormData })
    
        revalidatePath('/invoices') // 캐시 재검증
        // 또는 revalidateTag('invoice-list')
      }
    
      return <form action={createInvoice}>...</form>
    }
    ```
    
- **서버 액션 모듈 예시**
    
    ```jsx
    'use server'; // 파일 최상단에 선언
    
    export async function createUser(formData) {
      const name = formData.get('name');
      await db.user.create({ data: { name } });
      return { success: true };
    }
    
    export async function updateUser(formData) {
      const id = formData.get('id');
      const name = formData.get('name');
      await db.user.update({ where: { id }, data: { name } });
      return { success: true };
    }
    ```
    

### ❗ 헷갈렸던 점 / 문제 해결:

- ❌ “서버 액션도 클라이언트에서 직접 호출 가능한가?”
- ✅ 해결: 서버 액션은 클라이언트 컴포넌트에서 `form action` 또는 Next.js 폼 제출 방식으로만 실행 가능

### 💡 느낀 점 / 배운 점:

- 서버 액션은 기존 API 라우트보다 훨씬 간편하게 서버 로직을 처리할 수 있음
- 하지만 모든 폼/데이터 처리에 적합한 것은 아니므로 UX와 보안 요구사항에 따라 선택적으로 사용

### 🏷️ 키워드 (태그):

`#Nextjs` `#ServerAction` `#FormSubmit` `#useServer` `#AsyncFunction` `#DB` `#Revalidate`

| 날짜 | 주제 | 핵심 요약 | 실습 내용 | 문제 해결 / 느낀 점 | 키워드 태그 | 복습 필요 |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-09-09 | Server Action | 서버 환경 async 함수, 폼/DB 처리 적합 | 폼 제출 Server Action 코드, actions.js 모듈 | 클라이언트 직접 호출 불가, UX 고려 | `#Nextjs` `#ServerAction` `#useServer` `#FormSubmit` | ✅ |
