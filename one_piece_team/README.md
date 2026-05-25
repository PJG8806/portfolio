# FRAGMNT - AI 향수 추천 플랫폼

> ⚠️ 본 레포지토리는 팀 프로젝트 중 제가 담당한 기능을 중심으로 정리한 개인 포트폴리오입니다.  
> 백엔드 관점에서 AI 추천 시스템의 안정성, 성능 최적화, 운영 환경 대응을 고민하며 개발한 프로젝트입니다.

## 🔗 Team Repository

- https://github.com/space-one-piece/be_one_piece

AI 기반 향수 추천 플랫폼 **FRAGMNT**에서  
백엔드 리더로 참여하며 추천 시스템 설계, AI 안정화 아키텍처 구축,  
추천 성능 최적화 및 공유 시스템 개발을 담당했습니다.

---

## 📋 목차

1. [프로젝트 소개](#-프로젝트-소개)
2. [주요 기여](#-주요-기여)
3. [Tech Stack](#-tech-stack)
4. [핵심 기능](#-내가-구현한-핵심-기능)
   - [AI 추천 엔진 설계](#1-ai-추천-엔진-설계)
   - [유클리드 거리 기반 추천 시스템](#2-유클리드-거리-기반-추천-시스템)
   - [정확도 + 다양성 추천 전략](#3-정확도--다양성-추천-전략)
   - [Retry + Fallback Pipeline](#4-retry--fallback-pipeline)
   - [유클리드 거리 기반 후보 사전 필터링](#5-유클리드-거리-기반-후보-사전-필터링-pre-filtering)
   - [Social Share & OG Crawler System](#6-social-share--og-crawler-system)
5. [문제 해결 경험](#-문제-해결-경험)
6. [프로젝트를 통해 배운 점](#-프로젝트를-통해-배운-점)
7. [주요 키워드](#-주요-키워드)
8. [프로젝트 회고](#-프로젝트-회고)
9. [프로젝트 구조](#-프로젝트-구조)

---

# 📌 프로젝트 소개

FRAGMNT는 설문, 키워드, 이미지, 챗봇 데이터를 기반으로  
개인 맞춤 향수를 추천하는 AI 기반 추천 플랫폼입니다.

단순 키워드 매칭이 아닌:

- 사용자 취향 벡터화
- AI 기반 추천
- 유사도 계산
- 안정적인 AI 응답 처리
- SNS 공유 시스템

을 결합하여 실제 서비스 형태로 구현했습니다.

---

# 👨‍💻 주요 기여

## Backend Leader

### 담당 영역

- AI 추천 시스템 설계 및 구현
- Gemini API 기반 추천 파이프라인 구축
- Retry / Fallback 기반 AI 안정성 아키텍처 구현
- 유클리드 거리 기반 후보 사전 필터링 구조 설계
- 유클리드 거리 기반 추천 알고리즘 구현
- OG(Open Graph) 기반 SNS 공유 시스템 개발
- 프론트엔드 및 인프라 협업

---

# 🛠 Tech Stack

## Backend

<p>
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white"/>
  <img src="https://img.shields.io/badge/Django-092E20?style=for-the-badge&logo=django&logoColor=white"/>
  <img src="https://img.shields.io/badge/DRF-A30000?style=for-the-badge&logo=django&logoColor=white"/>
  <img src="https://img.shields.io/badge/Redis-DC382D?style=for-the-badge&logo=redis&logoColor=white"/>
  <img src="https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white"/>
</p>

## AI / Infra

<p>
  <img src="https://img.shields.io/badge/Gemini_AI-4285F4?style=for-the-badge&logo=google&logoColor=white"/>
  <img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white"/>
  <img src="https://img.shields.io/badge/Nginx-009639?style=for-the-badge&logo=nginx&logoColor=white"/>
  <img src="https://img.shields.io/badge/AWS_S3-569A31?style=for-the-badge&logo=amazons3&logoColor=white"/>
</p>

---

# 🚀 내가 구현한 핵심 기능

# 1. AI 추천 엔진 설계

사용자 설문 데이터를 기반으로  
5차원 취향 벡터를 생성하고 향수 프로필과 매칭하는 추천 엔진을 구현했습니다.

## 핵심 아이디어

1. 설문 답변 정량화
2. 사용자 취향 벡터 생성
3. 향수 프로필 벡터와 거리 계산
4. 가장 유사한 향수 추천

---

# 2. 유클리드 거리 기반 추천 시스템

사용자 취향 벡터와 향수 프로필 간의  
유클리드 거리(Euclidean Distance)를 계산하여 추천 정확도를 향상시켰습니다.

## 배경

초기에는 사용자 선택 데이터와 향 데이터를 그대로 Gemini AI에 전달하여 추천 결과를 받는 방식을 사용했습니다.  
하지만 모든 데이터를 AI에 넘기다 보니 응답까지 **약 30초의 지연**이 발생했고, 실서비스에서 사용하기 어려운 수준이었습니다.

## 선택한 이유

여러 대안을 검토한 결과, 향수 DB 규모가 크지 않은 상황에서는  
**데이터가 적을 때 성능이 좋은 유클리드 거리 계산**이 적합하다고 판단했습니다.  
AI 없이도 벡터 간 거리만으로 빠르게 유사도를 측정할 수 있어 응답 지연 문제를 해결할 수 있었습니다.

```python
def distance(p1, p2):
    return math.sqrt(sum((p1[k] - p2[k])**2 for k in p1))
```

## 구현 포인트

- 주관적인 설문 데이터를 5차원 수치 벡터로 정량화
- 텍스트 매칭이 아닌 벡터 거리 기반 유사도 측정
- AI 의존 없이 자체 계산으로 추천 처리

---

# 3. 정확도 + 다양성 추천 전략

Top-3 후보군 중 랜덤 추천 방식을 도입하여  
추천 정확도와 다양성을 동시에 확보했습니다.

## 배경

유클리드 거리 기반 추천을 적용한 이후, 새로운 문제가 발생했습니다.  
**비슷한 취향 데이터가 입력되면 항상 동일한 향수가 추천**되어 사용자가 반복적인 경험을 느끼는 현상이었습니다.

## 해결 방식

거리 계산 결과를 완전히 버리지 않으면서도 다양성을 확보하기 위해,  
**유사도 상위 3개 향수를 선별한 뒤 그 중 하나를 랜덤으로 출력**하는 방식을 선택했습니다.  
단순히 1위만 고정 추천하는 것보다 추천 신뢰도를 유지하면서도 결과의 다양성을 확보할 수 있었습니다.

```python
return random.choice(sorted_scents[:3])
```

## 결과

- 유사도 기반 신뢰성은 유지하면서 반복 추천 문제 해결
- 동일한 입력에도 자연스럽게 다른 향수가 추천되어 사용자 경험 다양성 확보

---

# 🧠 AI 안정성 아키텍처

# 4. Retry + Fallback Pipeline

Gemini API 응답 실패 및 Timeout 상황 대응을 위해  
3-Step Retry & Fallback 구조를 설계했습니다.

## 배경

Gemini API를 사용하면서 **외부 서버 상태에 따라 응답 실패 및 Timeout이 자주 발생**했습니다.  
이 경우 추천 기능 전체가 실패하여 사용자가 아무런 결과도 받지 못하는 상황이 생겼습니다.

## 선택한 이유와 구조

단순히 에러를 반환하는 것보다, **재시도 → 모델 전환 → 기본 추천** 순의 단계적 대응이 서비스 안정성에 더 적합하다고 판단했습니다.

- **1~3차 Retry**: 5초 간격으로 최대 3회 재시도
- **Sub 모델 Fallback**: 3회 모두 실패 시 보조 모델로 전환 시도
- **기본 추천 제공**: 최종 실패 시에도 사전 정의된 기본 추천 데이터를 반환하여 서비스 전체 실패 방지

## 결과

- 일시적인 외부 API 장애에도 서비스 중단 없이 추천 결과 제공
- 최악의 경우에도 사용자가 빈 화면을 보는 상황을 방지

---

# ⚡ 추천 성능 최적화

# 5. 유클리드 거리 기반 후보 사전 필터링 (Pre-filtering)

Gemini AI 입력 전, 유클리드 거리로 유사도 상위 후보군을 먼저 추려  
AI에 전달하는 데이터를 최소화했습니다.

## 배경

모든 향수 데이터를 AI에 전달하면 불필요한 토큰이 낭비되고,  
AI 응답 지연 및 Timeout 발생 가능성이 높아졌습니다.

## 구현 방식

1. 전체 향수 DB를 대상으로 유클리드 거리 계산
2. 거리 기준 상위 N개 후보군 선별
3. 선별된 후보군만 Gemini AI에 전달하여 최종 추천 생성

```python
candidates = sorted(scents, key=lambda s: distance(user_vector, s.profile_vector))[:N]
```

## 해결한 문제

- AI 응답 지연 (30초 → 10초)
- Timeout 감소
- 불필요한 토큰 사용 절감
- API 비용 최적화

---

# 🌐 OG 공유 시스템

# 6. Social Share & OG Crawler System

카카오톡 및 SNS 공유를 위한  
OG(Open Graph) 메타데이터 제공 시스템을 구현했습니다.

```python
question_id = encode_id(result_id)
expires_at = timezone.now() + datetime.timedelta(days=7)
```

## 구현 기능

- 공유 URL 생성
- Hashids 기반 ID 암호화
- 7일 TTL 만료 관리
- 동적 OG Metadata 제공
- 소셜 미리보기 대응

## 해결한 문제

기존 DB Primary Key가 외부에 그대로 노출될 수 있었고,  
공유 데이터의 만료 관리도 필요했습니다.

## 개선 방식

- Hashids 기반 공유 ID 생성
- TTL 기반 만료 시스템 구현
- 만료 시 410 Gone 처리
- SNS 크롤러 전용 OG 메타데이터 제공

## 결과

- 내부 DB ID 보호
- 만료 링크 관리 가능
- 카카오톡 / SNS 공유 최적화

---

# 🧩 문제 해결 경험

# AI 응답 지연 문제

## 문제

Gemini AI 응답 생성 과정에서  
응답 시간이 길어지며 사용자 경험이 저하되었습니다.

## 해결

- 유클리드 거리 기반 후보 사전 필터링 도입
- 후보군 선별 구조 설계
- AI 입력 데이터 최소화

## 결과

- 응답 시간 개선 (30초 → 10초)
- Timeout 감소

---

# AI 장애 대응

## 문제

외부 AI 서버 상태에 따라  
추천 기능 전체 실패가 발생했습니다.

## 해결

- Retry + Fallback 구조 구현
- 장애 발생 시 기본 추천 제공

## 결과

- 무중단 추천 서비스 구현
- 안정적인 사용자 경험 유지

---

# 📚 프로젝트를 통해 배운 점

이번 프로젝트를 통해:

- AI는 항상 예측 가능하게 동작하지 않는다는 점
- 장애 상황을 고려한 설계의 중요성
- 추천 알고리즘 최적화
- 실제 서비스 운영 환경에서의 안정성 확보
- 프론트엔드 / 인프라 협업 경험

을 깊이 경험할 수 있었습니다.

특히 단순 CRUD 구현보다:

- 장애 대응
- 안정성
- 성능 최적화
- 사용자 경험 유지
- 운영 환경 대응

을 고려한 백엔드 설계 역량을 키울 수 있었던 프로젝트였습니다.

---

# 🔑 주요 키워드

- Recommendation System
- AI Integration
- Retry / Fallback Architecture
- Euclidean Distance Pre-filtering
- Recommendation Optimization
- OG Crawler System
- API Stability
- Backend Architecture

---

# 📝 프로젝트 회고

초기 설계 단계에서:

- 에러 포맷 규격화
- API 응답 구조 표준화
- 추천 로직 정확도 개선

에 더 많은 시간을 투자했다면  
협업 및 유지보수 효율을 더욱 높일 수 있었을 것이라 생각합니다.

하지만 실제 서비스 수준의:

- AI 장애 대응
- 추천 엔진 설계
- 공유 시스템 구현
- 운영 안정성 확보
- 성능 최적화

를 직접 경험하며  
단순 CRUD를 넘어선 백엔드 아키텍처 경험을 얻을 수 있었습니다.

---

# 📂 프로젝트 구조

```bash
FRAGMNT
├── apps
│   ├── analysis
│   ├── chatbot
│   ├── core
│   ├── question
│   ├── scent
│   └── users
├── config
├── docker
├── nginx
├── requirements
└── manage.py
```
