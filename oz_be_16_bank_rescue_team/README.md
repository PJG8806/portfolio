# 🦁 사자사자 가계부 - Backend

부모와 자녀가 함께 사용하는 금융 교육 기반 가계부 서비스입니다.  
자녀의 소비/저축 습관을 기록하고, 부모가 안전하게 모니터링할 수 있도록 설계된 Django REST API 프로젝트입니다.

> OZ Coding School Backend Team Project  
> 개발 기간: 2026.02.19 ~ 2026.02.27

---

# 👨‍💻 담당 역할 - 박진규

## 담당 도메인
- `finance`
- `contents`

## 구현 기능

### 📒 Finance
- 거래내역(Transaction) CRUD
- 고정지출(FixedExpense) CRUD
- 부모 → 자녀 거래 조회 권한 처리
- 작성자 본인 수정/삭제 권한 검증
- 자산 소유권 검증 로직 구현

### 💡 Contents
- 랜덤 금융 명언 조회 API
- 명언 스크랩 CRUD
- `(user, proverb)` Unique 제약 기반 중복 스크랩 방지
- 인증 사용자 권한 처리

---

# 🛠 Tech Stack

## Backend
- Python 3.13
- Django 6
- Django REST Framework
- PostgreSQL
- Simple JWT

## Infra / DevOps
- Docker
- Docker Compose
- GitHub

## Security
- JWT Authentication
- HttpOnly Cookie
- Fernet Encryption

---

# 📂 프로젝트 구조

```bash
oz-be-16-team1/
├── config/
├── users/
├── assets/
├── finance/
├── missions/
├── contents/
└── manage.py
```

---

# 📒 Finance 도메인 설계

## Transaction 핵심 기능

### ✅ 거래 CRUD
- 수입 / 지출 / 저축 카테고리 관리
- 자산(Account) 연결 가능
- 메모 및 중요도 저장 지원

### ✅ 권한 처리
- 부모:
  - 본인 + 자녀 거래 조회 가능
- 자녀:
  - 본인 데이터만 접근 가능
- 수정/삭제:
  - 작성자 본인만 가능

### ✅ 자산 소유권 검증

```python
Asset.objects.get(
    pk=asset_id,
    user=request.user
)
```

타인의 자산 연결을 차단하도록 구현했습니다.

---

## FixedExpense 핵심 기능

- 반복 지출 별도 관리
- `payment_day` 기반 정기지출 설계
- 사용자별 데이터 완전 분리

---

# 💡 Contents 도메인 설계

## 랜덤 명언 조회

```python
MoneyProverb.objects.order_by("?").first()
```

매 요청마다 다른 금융 명언을 제공하도록 구현했습니다.

---

## 명언 스크랩

### 중복 방지

```python
UniqueConstraint(
    fields=["user", "proverb"],
    name="unique_user_proverb"
)
```

동일 사용자의 중복 스크랩을 DB 레벨에서 차단했습니다.

### 구현 기능
- 내 스크랩 목록 조회
- 상세 조회
- 생성
- 삭제

---

# 🔐 권한 및 보안 처리

## Transaction 수정/삭제 권한

```python
if obj.user != request.user:
    raise PermissionDenied()
```

작성자 본인만 수정/삭제 가능하도록 처리했습니다.

---

## 인증 정책
- JWT 기반 인증
- Bearer Token 사용
- 인증 사용자만 금융 데이터 접근 가능

---

# 📌 주요 API

## Finance API

| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/finance/transaction/` | 거래 목록 조회 |
| POST | `/api/finance/transaction/` | 거래 생성 |
| PATCH | `/api/finance/transaction/{id}/` | 거래 수정 |
| DELETE | `/api/finance/transaction/{id}/` | 거래 삭제 |
| GET | `/api/finance/fixed/` | 고정지출 목록 |
| POST | `/api/finance/fixed/` | 고정지출 생성 |

---

## Contents API

| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/contents/` | 랜덤 명언 조회 |
| GET | `/api/contents/proverb_scrap/` | 내 스크랩 목록 |
| POST | `/api/contents/proverb_scrap/` | 스크랩 생성 |
| DELETE | `/api/contents/proverb_scrap/{id}/` | 스크랩 삭제 |

---

# 🧪 테스트 및 검증

## 테스트 중점
- 인증/권한 검증
- 부모-자녀 조회 범위
- 중복 스크랩 방지
- 작성자 권한 처리
- 자산 소유권 검증

## 결과

```bash
68 tests, OK
```

전체 테스트 통과로 핵심 기능 안정성을 검증했습니다.

---

# 📖 프로젝트를 통해 배운 점

- 금융 서비스는 단순 CRUD보다 권한/보안 설계가 중요하다는 점
- 도메인 규칙을 Serializer + DB Constraint로 이중 검증하는 방식
- 부모/자녀 관계 기반 조회 정책 설계 경험
- API 설계 단계(PRD/FlowChart/ERD)의 중요성

---

# 🚀 개선하고 싶은 부분

- Open Banking API 연동
- AI 소비 피드백 자동화
- Redis 기반 조회 캐싱
- 통계 대시보드 추가
- API 문서 자동화 고도화

---

# 🔗 GitHub

- [팀 GitHub Repository](https://github.com/oz-be-16-team1/oz-be-16-team1/tree/main/docs)
- [팀 GitHub docs](https://github.com/oz-be-16-team1/oz-be-16-team1/tree/main/docs)
