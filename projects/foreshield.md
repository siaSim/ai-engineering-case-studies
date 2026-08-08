# ForeShield — Climate Disaster Intelligence

> This was a team project. This case study describes only my verified contributions.
>
> The original ForeShield source is private. This document contains generalized explanations and links to verifiable GitHub records only.

## Overview

ForeShield는 기후재난 관련 질문에 대해 지역·재난 유형·시간 범위와 공식 문서 근거를 함께 다루는 AI Backend 프로젝트입니다. 제가 담당한 범위는 LLM Agent 실행 기반, Context Interpretation, RAG Provider 연결, Azure OpenAI 응답 계약, Tool orchestration, 안전 정책과 테스트입니다.

> **Image placeholder — Overview / Service UI**
>
> 실제 ForeShield 서비스 화면은 추후 제공 예정입니다.
>
> *Caption: Service UI — team implementation. The case study focuses on my verified AI/Backend contributions behind this flow.*

## Problem & Goal

기후재난 질문은 자연어로 입력되기 때문에 지역, 재난 유형, 리드타임이 누락되거나 모호할 수 있습니다. 이 상태에서 Agent가 임의의 지역을 선택하거나 잘못된 Tool을 실행하면, 검색 결과와 답변의 신뢰성이 함께 떨어질 수 있습니다.

초기 단계에서는 실제 Azure·PostgreSQL·외부 API를 연결하기 전에 Agent 실행 흐름을 반복 검증할 수 있는 결정론적 기반도 필요했습니다. 또한 Provider 결과가 ERROR, NO_DATA, PARTIAL, STALE인 상황을 단순 성공 응답으로 축약하지 않고, 답변 계약과 사용자에게 전달되는 상태를 분리해야 했습니다.

목표는 다음과 같았습니다.

- 외부 Provider와 분리된 Contract-based Agent Engine 구성
- 자연어 Context를 검증 가능한 AgentPlan으로 변환
- 필요한 입력이 없을 때 Tool 실행 대신 명확화로 종료
- Azure OpenAI 응답과 RAG 결과를 Structured Output과 Provenance로 연결
- Prompt Injection, Training/Live 경계, 잘못된 UI Action을 fail-closed로 처리
- 각 판단을 테스트와 CI로 검증

## Architecture

현재 ForeShield의 최종 아키텍처는 아직 확정되지 않았습니다. 아래는 최종 배포 구조가 아니라, 현재 코드와 PR에서 확인 가능한 논리적 구성 요소를 요약한 것입니다.

1. AgentRunRequest가 사용자 질문과 화면·대화 Context를 Agent 실행 계약으로 전달합니다.
2. Context Interpretation이 intent, region query, hazard, lead, time range, RAG 필요 여부를 구조화합니다.
3. AgentPlan과 Intent-based Tool Router가 필요한 입력을 확인하고 Tool sequence, clarification, 병렬화 후보를 결정합니다.
4. Tool Provider와 RAG Provider가 결과 상태, 오류, 경고, 출처와 버전 정보를 반환합니다.
5. ToolResultLedger와 orchestration 계층이 여러 Provider 결과를 Agent Event와 최종 응답 Metadata로 모읍니다.
6. Azure OpenAI Provider가 최종 응답을 Structured Output 계약으로 생성하고, 실패 시 안전한 오류 경로로 종료합니다.

이 설명은 현재 검증 가능한 Backend 구성 요소에 한정하며, 최종 서비스 Architecture를 확정하거나 Frontend 전체 구조를 본인 구현으로 주장하지 않습니다.

> **Architecture placeholder**
>
> Architecture diagram will be added after project architecture is finalized.

## My Contribution

### 계약 기반 Agent 실행 기반

실제 Azure와 DB에 의존하지 않고 Agent 실행 흐름을 검증하기 위해 AgentRunRequest Fixture, Mock ToolProvider, Mock RagProvider, MockAgentEngine의 이벤트 스트림과 정책 테스트를 구현했습니다.

Issue #15–#19를 Context Fixture → Tool Provider → RAG Provider → AgentEngine 실행 → 계약·정책 테스트로 나누고, PR #36에서 연결했습니다. 이 순서를 통해 Provider 구현이 완성되기 전에도 이벤트 순서, 상태 값, 근거 부족 처리와 live/training 경계를 반복 검증할 수 있었습니다.

### Context Interpretation과 AgentPlan

자연어 요청을 바로 Tool 호출로 연결하지 않고, Context Interpretation을 거쳐 AgentPlan으로 변환하도록 구현했습니다. 지역·재난·lead가 불명확하면 임의 값을 선택하지 않고 clarification 또는 오류 상태로 전환하도록 계약 검증을 추가했습니다.

Azure 응답에 대해서는 Pydantic 계약과 Structured Output 검증을 적용했습니다. 계약에 맞지 않는 응답은 성공 응답이나 임의 fallback으로 바꾸지 않고 fail-closed로 처리했습니다.

실제 HTTP/SSE 경로에 ConversationContext를 주입한 PR #218은 팀원 Nyahong이 구현했습니다. 따라서 이 Case Study에서는 제가 구현한 Agent 계약·해석·검증 계층과 해당 연결 지점에 대한 협업만 제 기여로 설명합니다.

### Intent-based Tool Router와 Tool Policy

불완전한 자연어 요청이 바로 실행으로 이어질 가능성을 줄이기 위해 intent별 Tool 선택, 필수 입력 확인, clarification, 병렬화 후보를 분리한 Tool Router 정책을 구현했습니다.

Live/Training 모드에서는 허용 Tool을 분리하고, Training에서 외부 영향을 만들 수 있는 Tool을 차단했습니다. 사용자의 확인이 필요한 Action은 승인 전 deferred 상태로 남기고, 확인 없이 세션 생성이 진행되지 않도록 정책을 적용했습니다.

### Azure OpenAI와 RAG Backend 연결

Azure OpenAI Response Provider와 실제 RAG Provider를 기존 계약에 연결했습니다. RAG 경로에서는 지역·재난·출처·발행일 필터와 Provider 선택 경계를 유지하고, 로컬 Mock 경로와 Azure Search 경로를 설정으로 분리했습니다.

다만 PR에서 명시된 것처럼 Azure Search smoke test와 실제 운영 E2E는 자격증명·배포 환경에서 별도로 확인해야 하므로, 이 Case Study에서는 Production 운영 완료로 표현하지 않습니다.

### 결과 Orchestration과 Provenance

여러 Tool과 RAG 결과가 합쳐질 때 상태와 출처가 사라지지 않도록 ToolResultLedger와 결과 Metadata 전달을 구현했습니다.

source type, data version, model version, index version, reference time을 결과 Event와 completed 응답에 보존하고, RAG index version은 일반 data version과 분리했습니다. ERROR, NO_DATA, PARTIAL, STALE 상태를 성공 결과로 축약하지 않도록 회귀 테스트도 추가했습니다.

### Prompt Injection, UI Action, 응답 진단

RAG 근거와 Context 안의 문장을 시스템 지시로 실행하지 않고 데이터로만 해석하도록 Azure Agent와 Context Interpreter Prompt 경계를 보강했습니다.

또한 UI Action allowlist, 실행 결과와 목표 상태 비교, Live/Training 정책을 Backend에서 검증했습니다. Azure 응답이 비어 있거나 계약에 맞지 않을 때는 원문이나 사용자 입력을 로그에 남기지 않고 finish reason, response length, token diagnostics와 같은 제한된 Metadata만 기록하도록 했습니다.

## Technical Decisions

| 문제 | 판단 | 구현 |
|---|---|---|
| Provider 연결 전 Agent 흐름 검증 필요 | 외부 의존성을 먼저 끌어오면 오류 원인을 분리하기 어려움 | Contract-based Mock Tool/RAG Provider와 MockAgentEngine |
| 모호한 지역·재난 입력 | 임의 fallback은 잘못된 재난 안내로 이어질 수 있음 | AgentPlan 검증과 clarification, 명시적 fail-closed |
| LLM의 자유 형식 응답 | JSON object만으로는 필드·타입 계약을 보장하지 못함 | JSON Schema 우선, 지원 불가 시 JSON mode와 동일한 로컬 검증 |
| Provider 결과 상태 손실 | 검색 실패를 데이터 없음으로 오해할 수 있음 | ToolResultLedger와 status/provenance/version 전달 |
| 근거 안의 악성 지시 | RAG 결과가 Prompt Instruction처럼 해석될 수 있음 | Tool/RAG/Context 내용을 데이터로 취급하는 Prompt 경계와 회귀 테스트 |
| Token 원인 불명확 | 근거 없이 예산이나 비용을 임의 조정하면 안 됨 | finish reason과 completion/reasoning token의 제한적 진단 후 reasoning effort 조정 |

## Service Flow

현재 코드에서 확인 가능한 실행 흐름은 다음과 같습니다.

- 사용자 질문과 화면 Context가 AgentRunRequest로 전달됩니다.
- Context Interpretation이 질문을 구조화하고, AgentPlan이 실행에 필요한 조건을 정리합니다.
- Tool Router가 Tool sequence, clarification, 모드별 허용 여부를 결정합니다.
- Tool/RAG Provider가 결과와 상태·출처·버전을 반환합니다.
- Orchestration 계층이 결과를 Ledger로 모은 뒤 Agent Event와 최종 응답에 반영합니다.
- Azure OpenAI Provider가 Structured Answer 계약을 검증하며, 계약 위반이나 외부 오류는 성공으로 바꾸지 않습니다.

> **Image placeholder — Service Flow 1 / Agent Context**
>
> 실제 Agent/RAG 동작 화면은 추후 제공 예정입니다.
>
> *Caption: Service UI — team implementation. The case study focuses on my verified AI/Backend contributions behind this flow.*

> **Image placeholder — Service Flow 2 / Tool and RAG Result**
>
> 실제 Tool/RAG 결과와 Structured Answer 화면은 추후 제공 예정입니다.
>
> *Caption: Service UI — team implementation. The case study focuses on my verified AI/Backend contributions behind this flow.*

## Engineering Workflow

기술 구현 외에도 GitHub Issue와 Project를 사용해 Agent 작업을 작은 검증 단위로 분해했습니다.

ForeShield의 [GitHub Project](https://github.com/orgs/ForeShield/projects/1) 작업 목록에서 제가 작성한 Issue #15–#19가 확인됩니다.

- #15 AgentRunRequest 실행 Context Fixture
- #16 계약 기반 Mock ToolProvider
- #17 계약 기반 Mock RagProvider
- #18 Mock Provider 기반 AgentEngine 스트리밍
- #19 AgentEngine 계약·정책 테스트

Project 화면에서는 해당 작업이 siaSim 담당, LLM/RAG 영역, P1 우선순위, Feature 또는 Chore, Done 상태로 표시되고 PR #36과 연결되어 있습니다. 즉, 실행 기반을 한 번에 크게 구현하기보다 Context → Provider → Engine → Test 순서로 나누고, 하나의 PR에서 Issue 완료 흐름을 검증한 기록이 남아 있습니다.

Project Item을 누가 생성했는지는 GitHub 화면에서 별도로 제공되지 않으므로, Issue 작성자와 Project Item 생성자를 동일하다고 주장하지 않습니다. 이후 Tool Router, Provider orchestration, Live/Training 정책도 별도 Issue와 PR로 분리해 추적했습니다.

## Reliability & Safety

- 지역·재난·lead가 불명확하면 임의 값을 선택하지 않고 clarification 또는 오류로 종료
- RAG 결과의 NO_RESULTS, INSUFFICIENT_EVIDENCE, ERROR, PARTIAL, STALE 상태를 구분
- Structured Output 계약 위반을 성공 응답으로 변환하지 않음
- RAG와 Context 안의 Embedded Prompt Instruction을 데이터로만 취급
- Live/Training 모드별 Tool allowlist와 외부 영향 차단
- UI Action 실행 대상과 실제 화면 상태가 다르면 fail-closed
- Token diagnostics에 사용자 입력과 모델 출력 원문을 기록하지 않음
- 실제 Azure·PostgreSQL·Search 운영 상태는 별도 E2E 검증이 필요함

## Result & Validation

PR과 Issue에 기록된 검증 결과는 다음과 같습니다.

- PR #36: PR 본문 기준 로컬 pytest 34 passed, 17 skipped, Ruff 통과, GitHub Actions의 의존성 설치·린트·테스트·비밀값 검사 통과
- PR #54: PR 본문 기준 로컬 pytest 139 passed, 19 skipped, PostgreSQL 기반 통합 테스트는 CI에서 검증
- PR #80: PR 본문 기준 178 passed, 35 skipped. Azure smoke test는 인증정보가 없을 때 의도적으로 skip
- PR #221: Tool Router 집중 테스트 21 passed
- PR #234: Agent Provider·Context Interpretation·Azure Chat 집중 테스트 32 passed. 로컬 전체 수집은 uvicorn 환경 문제로 실행되지 않음
- PR #241: Agent Provider·Context Interpretation·Azure Chat 테스트 35 passed, 변경 파일 Ruff 통과
- PR #245: 관련 Agent/SSE 테스트 34 passed, 전체 로컬 회귀 1054 passed·101 skipped·환경 의존 실패 11건
- PR #252: 관련 테스트 67개, Ruff, compile check 통과
- PR #254: 현재 Open PR이며 변경 파일 테스트 19 passed. Azure 실제 호출과 운영 배포 후 smoke test는 별도 검증 필요

위 수치는 제가 새로 측정한 결과가 아니라 각 PR에 기록된 검증 결과입니다. 실제 Production 성능, 사용자 수, 정확도, Latency 개선 수치는 이 문서에서 주장하지 않습니다.

## Evidence

핵심 근거는 다음 PR에서 직접 확인할 수 있습니다.

- [PR #36 — Contract-based Mock Agent Engine foundation](https://github.com/ForeShield/backend/pull/36)
- [PR #194 — Structured natural-language Context Interpretation](https://github.com/ForeShield/backend/pull/194)
- [PR #221 — Intent-based Tool Router policy](https://github.com/ForeShield/backend/pull/221)
- [PR #230 — Embedded Prompt Instruction defense](https://github.com/ForeShield/backend/pull/230)
- [PR #234 — Azure answer schema](https://github.com/ForeShield/backend/pull/234)
- [PR #245 — Provider result orchestration and provenance](https://github.com/ForeShield/backend/pull/245)

전체 PR·Issue·Commit과 핵심 파일 경로는 [ForeShield Evidence](../evidence/foreshield.md)에 정리했습니다.

Private Repository의 PR과 코드 링크는 외부 채용 담당자에게 보이지 않을 수 있습니다.

## Lessons Learned

- Agent 기능은 Model 호출부터 시작하기보다 입력·Tool·결과·오류 계약을 먼저 고정해야 테스트 가능한 구조가 됩니다.
- Context Interpretation과 ConversationContext의 실제 HTTP/SSE 주입은 서로 다른 문제이므로, 계약·해석·요청 연결을 분리해 검증해야 합니다.
- RAG 결과의 내용뿐 아니라 상태, 출처, 버전, 기준 시점을 함께 보존해야 후속 답변에서 근거를 잃지 않습니다.
- Token 문제는 비용·Latency를 임의로 조정하기보다 finish reason과 usage를 먼저 관측한 뒤 변경해야 합니다.
- 다음 단계에서는 최종 Architecture가 확정된 후 다이어그램과 실제 서비스 화면을 추가하고, Azure·PostgreSQL·Search 운영 E2E는 별도 검증 결과가 있을 때만 기록할 예정입니다.
