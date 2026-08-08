# PPYURIND — Emotion & Conflict Record Analysis

**Role:** AI Backend / LLM Application · **Period:** 2026.06.29–2026.07.05 (siaSim PR 기록 기준) · **Team Project**

> This was a team project. This case study describes only my verified contributions.
>
> The original PPYURIND source is private. This document contains generalized explanations and links to verifiable GitHub records only.

## Overview

PPYURIND는 감정·갈등 기록을 분석하고, 대화 표현을 바꾸며, 일반 상담·법률 정보·안전 안내를 구분하는 AI Backend 프로젝트입니다. 저는 Azure OpenAI 응답 계약 확장, 개인정보 보호, Safety Policy, RAG routing과 법률 정보 범위, 라벨·저장 신뢰성 개선을 담당했습니다.

> **Image placeholder — Overview / Team Service UI**
>
> 기존 프로젝트 캡처가 준비되면 이 위치에 대표 화면을 추가합니다. 화면은 팀 UI에서 Backend/AI 결과가 제공되는 방식을 보여주는 용도입니다.
>
> *Caption: Service UI — team implementation. The case study focuses on my verified AI/Backend contributions behind this flow.*

## Problem & Goal

감정 기록 서비스의 입력은 개인식별정보와 민감한 관계 맥락을 포함할 수 있고, 하나의 채팅 흐름 안에서도 일반 관계 고민·법률 정보·즉시 안전 위험이 섞일 수 있습니다. 자유 형식 LLM 응답만으로 처리하면 개인정보가 분석·저장 경계를 넘거나, 법률 판단과 안전 안내가 잘못된 순서로 제공될 위험이 있었습니다.

목표와 제약은 다음과 같았습니다.

- Azure OpenAI 응답을 기존 API와 호환되는 Structured Output으로 연결
- 분석·저장·공유 전에 PII를 보호하고 Azure 장애 시 fallback 유지
- 법률 RAG는 법률성 맥락에만 연결하고, 근거가 약한 주제는 상담 준비 수준으로 제한
- 긴급 상황은 법률 설명보다 안전 안내가 우선되도록 정책화
- 기존 프론트·DB 계약을 깨지 않으면서 라벨, 저장, 테스트 정합성 개선

## Architecture

현재 코드와 PR에서 확인되는 Backend 논리 구조는 다음과 같습니다.

- **Input protection:** 기록·분석·미디어·커뮤니티 입력을 공통 PII masking 경로로 통과시킵니다.
- **Analysis layer:** Emotion Analysis와 Tone Conversion이 Azure OpenAI Structured Output 또는 명시된 backend fallback을 사용합니다.
- **Policy layer:** Content Safety 결과와 AI Safety Policy가 위험도·긴급 정책·법률 표현 제한을 결정합니다.
- **Routing layer:** 일반 상담, 법률 RAG, 안전 안내를 현재 질문과 대화 이력에 따라 분리합니다.
- **Persistence layer:** 분석 결과, 저장 상태, label taxonomy와 응답 필드를 기존 계약에 맞춰 정규화합니다.

이는 최종 배포 Architecture나 Frontend 전체 구현을 확정하는 설명이 아니라, 현재 개인 기여를 검증할 수 있는 Backend 경계의 요약입니다.

> **Architecture placeholder**
>
> Architecture diagram will be added after project architecture material is provided.

## My Contribution

### 1. Structured LLM Analysis & Response Contracts

기존 감정 분석 응답을 단순 감정 목록에서 사실·해석·느낀 감정·균형 관점과 키워드별 sentiment로 확장했습니다. Azure OpenAI `EmotionAnalysisResult` 계약에 새 구조를 추가하고, 점수 범위와 label을 정규화해 저장 응답과 API schema가 같은 기준을 사용하도록 했습니다. [PR #30](https://github.com/AIStreetFighter/ppyurind-backend/pull/30) · [Commit 6142601](https://github.com/AIStreetFighter/ppyurind-backend/commit/614260199198a1ba957b0a472dec5a3b187d6d83)

Tone Conversion에서는 Azure 응답과 backend mock fallback을 `source`로 구분하고, 설정 누락·빈 응답·호출 예외가 명시된 fallback으로 내려가도록 응답 계약을 보강했습니다. [PR #20](https://github.com/AIStreetFighter/ppyurind-backend/pull/20)

초기 Azure 기반 감정 분석과 기본 Tone Conversion 구현 자체는 팀원의 PR에서 시작된 범위이며, 이 문서에서는 제가 작성한 후속 계약 확장과 fallback 정비만 개인 기여로 설명합니다.

### 2. PII Protection & Safety Policy

Azure AI Language PII Detection을 공통 masking 경로에 연결하고, Azure 설정이 없거나 실패하면 기존 정규식 fallback을 사용하도록 구성했습니다. 동기 Azure 호출을 async wrapper로 감싸 기록·감정·미디어·커뮤니티·직접 AI 분석 경로에서 동일한 정책을 적용했습니다. [PR #28](https://github.com/AIStreetFighter/ppyurind-backend/pull/28)

또한 기존 `normal/caution/danger` 호환성을 보존하면서 `emergency`를 optional meta/safety 정보로 전달하고, DB 저장 시에는 legacy 값으로 안전하게 정규화했습니다. 긴급 연락처와 법률·상담 표현 제한을 별도 Policy로 분리해 알 수 없는 category에도 안전 기본값을 반환하도록 했습니다. [PR #26](https://github.com/AIStreetFighter/ppyurind-backend/pull/26)

Content Safety의 초기 분석·저장 통합은 팀 구현이었고, 저는 이후 Chat routing에서 Content Safety 결과와 실제 위험 표현을 결합하는 안전 분기와 테스트를 보강했습니다. [PR #31](https://github.com/AIStreetFighter/ppyurind-backend/pull/31)

### 3. RAG Chat Routing & Legal Information Boundaries

일반 관계 고민에도 법률 Search가 연결될 수 있던 경계를 현재 질문과 최근 사용자 history의 법률성 맥락을 기준으로 분리했습니다. 법률성일 때만 Azure Search data source를 사용하고, 즉시 위험이면 RAG보다 Safety Card를 우선하도록 했습니다. 부정문, 인용 문맥, 과거 피해 진술과 실제 현재 위험 표현을 구분하는 테스트도 추가했습니다. [PR #31](https://github.com/AIStreetFighter/ppyurind-backend/pull/31)

RAG 근거가 상대적으로 약한 불륜·도박·접근금지 같은 주제는 승소 가능성이나 신청 요건을 단정하지 않고, 날짜별 사실관계·적법한 기록 정리·전문가 상담 권장 수준으로 제한했습니다. 양육·재산·가사소송 등 주제별 guidance를 분리해 같은 포괄 문구를 반복하지 않도록 했습니다. [PR #32](https://github.com/AIStreetFighter/ppyurind-backend/pull/32)

### 4. Taxonomy & Backend Reliability

감정·갈등 주제·숨은 욕구·관계 패턴·Safety·RAG 신뢰도·AI Tone 등의 라벨을 공통 taxonomy로 모으고, helper를 Schema·Prompt·Test에서 재사용할 수 있게 했습니다. 기존 `realistic` 입력을 호환하면서 canonical label은 `honest`로 정규화하고, `label_schema_version`을 optional metadata로 관리했습니다. [PR #27](https://github.com/AIStreetFighter/ppyurind-backend/pull/27)

분석 저장에서는 빈 입력을 거절하고, 동일 사용자·최근 시간 범위·유사도 기준으로 중복 기록을 update/create로 구분했습니다. 리포트 기간의 종료일 포함 문제와 기록 시각 갱신도 보완했습니다. [PR #22](https://github.com/AIStreetFighter/ppyurind-backend/pull/22) · [PR #24](https://github.com/AIStreetFighter/ppyurind-backend/pull/24) · [PR #33](https://github.com/AIStreetFighter/ppyurind-backend/pull/33)

## Technical Decisions

- **계약 우선:** LLM 출력은 Pydantic 기반 구조와 정규화 단계를 거친 뒤 API·저장 모델로 전달
- **보호 경계 우선:** 분석보다 먼저 PII masking을 적용하고, Azure 실패 시에도 명시된 fallback 경로 사용
- **라우팅 보수성:** 일반 상담에 법률 RAG를 자동 연결하지 않고, 법률·안전 맥락을 별도 판별
- **호환성 유지:** 기존 필수 필드와 DB 제약을 보존하면서 `meta`, `safety`, `source`, `save_action` 같은 optional/확장 필드 사용

## Service Flow

아래 화면은 팀 UI에서 제 Backend/AI 결과가 사용자에게 나타나는 형태를 보여주는 용도로만 사용합니다. Frontend 전체 구현을 제 기여로 주장하지 않습니다.

> **Image placeholder — Emotion Analysis / Structured Result**
>
> 감정 기록 입력과 사실·해석·감정·균형 관점 결과가 함께 보이는 실제 캡처를 추후 추가합니다.
>
> *Caption: Service UI — team implementation. The case study focuses on my verified AI/Backend contributions behind this flow.*

> **Image placeholder — RAG Chat / Safety Routing**
>
> 일반 상담, 법률 정보, 안전 안내가 구분되어 표시되는 실제 캡처를 추후 추가합니다.
>
> *Caption: Service UI — team implementation. The case study focuses on my verified AI/Backend contributions behind this flow.*

> **Image placeholder — PII Masking / Stored Analysis**
>
> 개인정보가 보호된 분석·저장 결과와 사용자 노출 응답을 보여주는 실제 캡처를 추후 추가합니다.
>
> *Caption: Service UI — team implementation. The case study focuses on my verified AI/Backend contributions behind this flow.*

## Reliability & Safety

- Azure PII Detection 실패·미설정 시 regex fallback으로 전환하고, 내부 오류 상세를 응답·로그에 그대로 노출하지 않음
- Content Safety와 텍스트 위험 신호를 함께 사용하되 단순 법률 키워드만으로 emergency를 판정하지 않음
- `emergency`와 legacy `danger`를 분리해 기존 API·DB 계약을 보호
- 법률 RAG는 법률성 맥락에만 연결하고, 약한 근거 주제는 결과 예측이 아닌 상담 준비로 제한
- Label과 저장 결과를 정규화해 알 수 없는 값·중복 입력·기간 경계 오류를 안전하게 처리

## Result & Validation

### Software Tests

PR과 Commit에서 확인 가능한 Backend 검증 결과는 다음과 같습니다.

- [PR #20](https://github.com/AIStreetFighter/ppyurind-backend/pull/20): Tone Conversion의 Azure 성공·설정 누락·호출 실패 fallback 및 `source` 구분 테스트 보강
- [Commit `fe02d51`](https://github.com/AIStreetFighter/ppyurind-backend/commit/fe02d51bb5933faf30a6d053329ea4f642f5c39a): Safety Policy 집중 테스트 15 passed
- [Commit `22c688b`](https://github.com/AIStreetFighter/ppyurind-backend/commit/22c688b4f61de564cada8913b66a12e48859b479): Label taxonomy 테스트 21 passed
- [PR #24](https://github.com/AIStreetFighter/ppyurind-backend/pull/24): Report 기간 경계 테스트 결과 69 passed가 Commit 기록에 남아 있음
- [PR #33](https://github.com/AIStreetFighter/ppyurind-backend/pull/33): 감정 기록 중복 저장·시각 갱신 테스트 7 passed

PR #28, #30, #31, #32에는 관련 테스트 파일과 검증 시나리오가 추가되어 있지만, 공개 PR 기록에서 일관된 전체 통과 수치가 확인되지 않는 항목은 숫자로 확대하지 않았습니다. PR #30 자체도 일부 검증 항목을 미완료로 표시하므로 완료된 LLM 평가로 표현하지 않습니다.

### LLM Quality Evaluation Boundary

Repository에서 확인되는 것은 schema·routing·fallback·policy에 대한 소프트웨어 테스트입니다. 정확도, Recall, F1, PII masking 정확도, 사용자 수, latency 또는 별도 평가셋 기반 품질 향상 수치는 확인되지 않아 주장하지 않습니다.

## Evidence

대표 근거는 아래 PR에 연결했습니다. 전체 근거 장부와 개인/팀 경계는 [PPYURIND Evidence](../evidence/ppyurind.md)에서 확인할 수 있습니다.

- [PR #20 — Azure OpenAI response source and fallback](https://github.com/AIStreetFighter/ppyurind-backend/pull/20)
- [PR #26 — AI Safety Policy Layer](https://github.com/AIStreetFighter/ppyurind-backend/pull/26)
- [PR #27 — Label taxonomy](https://github.com/AIStreetFighter/ppyurind-backend/pull/27)
- [PR #28 — Azure AI Language PII masking](https://github.com/AIStreetFighter/ppyurind-backend/pull/28)
- [PR #30 — Structured emotion analysis extension](https://github.com/AIStreetFighter/ppyurind-backend/pull/30)
- [PR #31 / #32 — RAG routing and legal scope](https://github.com/AIStreetFighter/ppyurind-backend/pull/31) · [#32](https://github.com/AIStreetFighter/ppyurind-backend/pull/32)

Private Repository 링크는 외부 채용 담당자에게 보이지 않을 수 있습니다. 원본 소스, PR diff, 내부 Prompt 전문, API 계약 전문, Secret, Azure Resource 정보, RAG Corpus는 이 Repository에 복사하지 않습니다.

## Lessons Learned

- LLM 기능은 모델 호출보다 응답 계약·정규화·fallback을 먼저 설계해야 기존 서비스와 안전하게 연결됩니다.
- PII, Content Safety, 법률 RAG는 하나의 “안전 기능”이 아니라 서로 다른 경계로 분리해 검증해야 합니다.
- 민감한 도메인에서는 더 구체적인 답변보다, 근거 범위와 불확실성을 보존하는 라우팅이 신뢰성에 중요합니다.
