# 박진규 | Backend Developer Portfolio

> Django / FastAPI 기반 백엔드 개발자입니다.  
> AI 추천 시스템, 데이터 정합성, 인증 보안, 비동기 아키텍처 등  
> 실제 서비스 수준의 문제를 고민하며 개발하고 있습니다.

---

## 🛠 기술 스택

**Backend** &nbsp;
![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![Django](https://img.shields.io/badge/Django-092E20?style=flat-square&logo=django&logoColor=white)
![DRF](https://img.shields.io/badge/DRF-A30000?style=flat-square&logo=django&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white)

**Desktop / MES** &nbsp;
![C#](https://img.shields.io/badge/C%23-239120?style=flat-square&logo=csharp&logoColor=white)
![DevExpress](https://img.shields.io/badge/DevExpress-FF7200?style=flat-square&logo=devexpress&logoColor=white)

> 아이앤지코리아 재직 당시 C# / DevExpress 기반 MES 윈도우 애플리케이션 개발 · 유지보수 (5년)

**DB / Cache** &nbsp;
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white)

**Infra / AI** &nbsp;
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![AWS S3](https://img.shields.io/badge/AWS_S3-569A31?style=flat-square&logo=amazons3&logoColor=white)
![AWS CloudFront](https://img.shields.io/badge/CloudFront-8C4FFF?style=flat-square&logo=amazonaws&logoColor=white)
![Gemini AI](https://img.shields.io/badge/Gemini_AI-4285F4?style=flat-square&logo=google&logoColor=white)

---

## 📂 프로젝트 목록

| # | 프로젝트 | 한 줄 소개 | 기간 | 역할 |
|---|----------|-----------|------|------|
| 1 | [FRAGMNT](#1-fragmnt---ai-향수-추천-플랫폼) | AI 기반 개인 맞춤 향수 추천 플랫폼 | 2026.04.02 ~ 2026.05.08 | 백엔드 리더 |
| 2 | [LMS 커뮤니티](#2-lms-커뮤니티---게시글-cud-담당) | Django 기반 LMS 커뮤니티 백엔드 | 2025.03.05 ~ 2025.03.27 | 백엔드 (게시글 CUD) |
| 3 | [사자사자 가계부](#3-사자사자-가계부---금융-교육-가계부-서비스) | 부모-자녀 함께 사용하는 금융 교육 가계부 | 2026.02.19 ~ 2026.02.27 | 백엔드 (finance / contents 도메인) |
| 4 | [J2S2](#4-j2s2---personal-insight--diary-platform) | 일기 · 명언 · 자아성찰 심리 케어 플랫폼 | 2026.01.20 ~ 2026.01.23 | 백엔드 (인프라 · 인증 · 스크래핑) |

---

## 1. FRAGMNT - AI 향수 추천 플랫폼

> 📁 [상세 README 보기](./one_piece_team/README.md)

설문, 키워드, 이미지, 챗봇 데이터를 기반으로 개인 맞춤 향수를 추천하는 AI 플랫폼입니다.  
단순 키워드 매칭이 아닌 사용자 취향 벡터화와 유사도 계산을 결합해 실제 서비스 형태로 구현했습니다.

**기술 스택**

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![Django](https://img.shields.io/badge/Django-092E20?style=flat-square&logo=django&logoColor=white)
![DRF](https://img.shields.io/badge/DRF-A30000?style=flat-square&logo=django&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white)
![Gemini AI](https://img.shields.io/badge/Gemini_AI-4285F4?style=flat-square&logo=google&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![AWS S3](https://img.shields.io/badge/AWS_S3-569A31?style=flat-square&logo=amazons3&logoColor=white)
![AWS CloudFront](https://img.shields.io/badge/CloudFront-8C4FFF?style=flat-square&logo=amazonaws&logoColor=white)

**핵심 기여**

- **유클리드 거리 기반 추천**: 전체 데이터를 AI에 넘기면 약 30초 지연이 발생해, DB 규모가 작은 상황에 적합한 유클리드 거리 계산으로 전환해 응답 속도를 개선
- **Top-3 랜덤 추천 전략**: 동일 입력에 항상 같은 결과가 나오는 문제를 해결하기 위해 유사도 상위 3개 중 랜덤 추출 방식 도입
- **Retry + Fallback 파이프라인**: 외부 AI 서버 장애 시 5초 간격 3회 재시도 → 보조 모델 전환 → 기본 추천 제공의 3단계 구조로 서비스 중단 방지
- **유클리드 거리 기반 후보 사전 필터링**: AI 입력 전 유사도 상위 후보만 선별해 토큰 사용량과 응답 시간 최적화 (30초 → 10초)
- **OG 공유 시스템**: Hashids 기반 ID 암호화 + 7일 TTL 만료 관리로 카카오톡/SNS 공유 지원
- **CloudFront + Presigned URL 이원화 설계**: 향수 이미지는 CloudFront로 Presigned URL 발급 없이 바로 출력, 개인 프로필 이미지는 Presigned URL로 발급하되 DB에는 키 값만 저장

---

## 2. LMS 커뮤니티 - 게시글 CUD 담당

> 📁 [상세 README 보기](./oz_externship_be_07/README.md)

Django 기반 LMS 커뮤니티 백엔드에서 게시글 CUD API와 S3 파일 라이프사이클 전반을 설계하고 구현했습니다.  
마크다운 본문 파싱 → S3 동기화 → DB 정합성 보장까지의 파일 처리 흐름을 책임졌습니다.

**기술 스택**

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![Django](https://img.shields.io/badge/Django-092E20?style=flat-square&logo=django&logoColor=white)
![DRF](https://img.shields.io/badge/DRF-A30000?style=flat-square&logo=django&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white)
![AWS S3](https://img.shields.io/badge/AWS_S3-569A31?style=flat-square&logo=amazons3&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)

**핵심 기여**

- **S3-DB 양방향 동기화**: 게시글 수정 시 본문 파싱 결과와 DB 파일 목록을 비교해 추가/삭제를 자동 동기화, 스토리지 누수 방지
- **삭제 순서 보장**: S3 삭제 → DB 삭제 순서를 강제해 고아 파일(orphan file) 발생 원천 차단
- **Presigned URL 자동 재발급**: 수정 요청마다 URL을 재발급해 만료로 인한 404 문제를 구조적으로 해결
- **정규식 4종 역할 분리**: 마크다운 링크 추출 / 쿼리스트링 제거 / 이미지 분기 / 첨부파일 분기를 각각 독립 정규식으로 설계해 변경 영향 범위 최소화
- **썸네일 조회 변환**: 본문 마크다운 파싱 결과를 재활용해 추가 저장 비용 없이 게시글 썸네일 제공
- **서비스 레이어 분리**: View는 요청/응답만 담당하고 파일 파싱·동기화 비즈니스 로직은 Service Layer로 분리

---

## 3. 사자사자 가계부 - 금융 교육 가계부 서비스

> 📁 [상세 README 보기](./oz_be_16_bank_rescue_team/README.md)

부모와 자녀가 함께 사용하는 금융 교육 기반 가계부 서비스입니다.  
자녀의 소비/저축 습관을 기록하고 부모가 안전하게 모니터링할 수 있도록 설계한 Django REST API 프로젝트입니다.

**기술 스택**

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![Django](https://img.shields.io/badge/Django-092E20?style=flat-square&logo=django&logoColor=white)
![DRF](https://img.shields.io/badge/DRF-A30000?style=flat-square&logo=django&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)

**핵심 기여**

- **Finance 도메인**: 거래내역 / 고정지출 CRUD 구현, 부모-자녀 조회 권한 분리, 자산 소유권 검증으로 타인 자산 연결 차단
- **Contents 도메인**: 랜덤 금융 명언 조회 API, `UniqueConstraint` 기반 DB 레벨 중복 스크랩 방지
- **권한 설계**: 부모는 본인+자녀 데이터 조회 가능, 자녀는 본인 데이터만 접근하는 계층적 권한 구조 설계
- **테스트**: 인증/권한/중복 방지/소유권 검증 등 핵심 기능 대상 팀 전체 68개 테스트 전체 통과

---

## 4. J2S2 - Personal Insight & Diary Platform

> 📁 [상세 README 보기](./j2s2/README.md)

일기, 자아 성찰 질문, 명언 서비스를 제공하는 심리 케어 플랫폼입니다.  
4일이라는 짧은 기간 내에 비동기 아키텍처를 활용한 인증 시스템과 데이터 수집 엔진을 구축했습니다.

**기술 스택**

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white)
![AWS EC2](https://img.shields.io/badge/AWS_EC2-FF9900?style=flat-square&logo=amazonec2&logoColor=white)

**핵심 기여**

- **인프라 구축**: EC2 배포 및 Nginx Reverse Proxy 설정, Tortoise-ORM 기반 비동기 DB 스키마 설계
- **인증 시스템**: `python-jose` JWT 발행/검증, `PBKDF2-SHA256` 비밀번호 암호화, OAuth2 기반 중앙 집중식 권한 관리
- **비동기 스크래핑 엔진**: `HTTPX` + `BeautifulSoup4`로 외부 명언 데이터 실시간 수집, `get_or_create` 방식 캐싱으로 응답 속도 최적화
- **북마크 시스템**: 등록/해제를 하나의 엔드포인트로 처리하는 토글 방식 설계, `prefetch_related`로 N+1 문제 해결

---

## 📬 Contact

- GitHub: [github.com/PJG8806](https://github.com/PJG8806)
