# ForeShield 기여 근거 (Evidence)

> 이 문서는 팀 프로젝트 전체가 아닌, GitHub에서 제가 작성한 PR·Issue·Commit·코드 경로·테스트·Project 기록으로 확인 가능한 기여만 정리합니다.

ForeShield/backend는 private Repository입니다. 외부 독자는 연결된 PR·Issue·파일을 열지 못할 수 있습니다. private source code, 내부 Prompt, API contract, Secret, Azure Resource, 내부 Endpoint, RAG Corpus, 팀 내부 데이터는 이 문서에 복제하지 않습니다.

개인 GitHub 기여 기록은 2026.08.03부터 확인되며, 이는 프로젝트 시작일과 별개의 기준입니다.

## 핵심 코드 경로 (Core Code Paths)

- Agent Engine — `src/foreshield/agent/engine.py` (Private repository)
- Context Interpretation — `src/foreshield/agent/interpretation.py` (Private repository)
- AgentPlan contracts — `src/foreshield/contracts/agent_plan.py` (Private repository)
- Intent-based Tool Router — `src/foreshield/agent/router.py` (Private repository)
- Azure OpenAI Provider — `src/foreshield/agent/azure.py` (Private repository)
- Result orchestration — `src/foreshield/agent/orchestration.py` (Private repository)
- Agent Event provenance — `src/foreshield/contracts/agent_event.py` (Private repository)
- Azure AI Search integration — `src/foreshield/rag/azure_search.py` (Private repository)
- UI Action policy — `src/foreshield/ui_action_policy.py` (Private repository)
- Agent Engine tests — `tests/` (Private repository)
- Tool Router tests — `tests/test_tool_router.py` (Private repository)
- Provider tests — `tests/test_agent_provider.py` (Private repository)
- Context Interpretation tests — `tests/test_agent_context_interpretation.py` (Private repository)

## 검증된 개인 기여 (Verified Contributions)

| 기능 | 개인 기여 | PR / Commit | 근거 수준 |
|---|---|---|---|
| AgentRunRequest fixtures | live/training Context Fixture와 검증 helper 구현 | Issue #15 (Private) · PR #36 (Private) | Issue 직접 근거 + 본인 작성 PR |
| Contract-based Agent Engine | Mock Tool/RAG Provider, AgentEvent streaming, terminal event와 상태 정책 구현 | Issue #16 (Private) · Issue #17 (Private) · Issue #18 (Private) · Issue #19 (Private) · PR #36 (Private) | 본인 작성 PR 직접 근거 + 테스트 |
| Conversation/SSE runtime boundary | AgentEngine을 Conversation/SSE 흐름에 연결하고 Tool execution·RAG provenance 정합성 보완 | PR #54 (Private) · Issue #49 (Private) · Issue #50 (Private) · Issue #51 (Private) · Issue #52 (Private) · Issue #53 (Private) | 본인 작성 PR 직접 근거; 공용 runtime 경계 |
| Configurable Real RAG Provider | Mock/Real Provider와 local/Azure Search 경계를 연결하고 지역 필터·provenance 유지 | Issue #71 (Private) · PR #80 (Private) | 본인 작성 PR 직접 근거; Azure smoke 별도 검증 |
| Azure OpenAI Response Provider | Azure Agent 응답 Provider와 기존 Agent 계약 연결 | PR #102 (Private) | 본인 작성 PR 직접 근거 |
| Azure token usage propagation | Azure completion usage를 Agent 경로로 전달 | PR #120 (Private) | 본인 작성 PR 직접 근거 |
| AgentPlan contracts | AgentPlan과 Conversation Context 계약, 실행 전 필수 필드 검증 정의 | Issue #144 (Private) · PR #187 (Private) | 본인 작성 PR 직접 근거 |
| Context Interpretation | 자연어에서 intent, region, hazard, lead, time range, RAG 필요 여부를 구조화 | Issue #144 (Private) · PR #194 (Private) | 본인 작성 PR 직접 근거 |
| Context contract hardening | Azure ContextInterpretation Structured Output, JSON fallback, 안전한 오류 진단 보완 | PR #220 (Private) | 본인 작성 PR 직접 근거 |
| Intent-based Tool Router | intent별 Tool sequence, clarification, RAG 추가, 병렬화 후보 정책 구현 | Issue #145 (Private) · PR #221 (Private) | 본인 작성 PR 직접 근거 + unit tests |
| UI Action validation | allowlist, region/hazard/lead 검증, 실행 결과와 제안 목표 비교 | PR #224 (Private) | 본인 작성 PR 직접 근거; Frontend adapter 공용 경계 |
| Prompt Injection defense | Tool/RAG/Context 내용을 지시가 아닌 데이터로 처리하는 Prompt 경계와 회귀 테스트 추가 | Issue #228 (Private) · PR #230 (Private) | 본인 작성 PR 직접 근거 + 테스트 |
| Structured Answer output | Azure final answer에 strict JSON Schema와 JSON mode fallback 적용 | Issue #232 (Private) · PR #234 (Private) | 본인 작성 PR 직접 근거 + 집중 테스트 |
| Token diagnostics | finish reason, response length, completion/reasoning token의 제한적 진단 추가 | Issue #239 (Private) · PR #241 (Private) | 본인 작성 PR 직접 근거 + 테스트 |
| Provider result orchestration | ToolResultLedger와 source/data/model/index version, reference time, 상태 보존 | Issue #146 (Private) · PR #245 (Private) | 본인 작성 PR 직접 근거 + 테스트 |
| Reasoning effort handling | 최종 Azure 응답에 제한된 reasoning effort 적용, schema fallback 정합성 검증 | PR #248 (Private) | 본인 작성 PR 직접 근거 + 테스트 |
| Live/Training Tool policy | 모드별 Tool allowlist, 외부 영향 차단, 승인 전 Training Action 보류 | Issue #147 (Private) · PR #252 (Private) | 본인 작성 PR 직접 근거 + 테스트 |
| UI execution target validation | open_chart 실행 목표와 실제 화면 상태 불일치 시 fail-closed | Issue #231 (Private) · PR #255 (Private) | 본인 작성 PR 직접 근거; Frontend contract 공용 경계 |
| CI maintenance | Agent answer policy test import 정리로 Ruff CI 오류 수정 | Issue #148 (Private) · PR #258 (Private) | 본인 작성 PR 직접 근거; 로직 변경 없음 |
| GitHub Project task decomposition | Agent 기반 작업을 Fixture → Mock Tool → Mock RAG → AgentEngine → Test Issue로 분해하고 PR과 연결 | ForeShield Project #1 (source items private) · Issue #15 (Private) · Issue #16 (Private) · Issue #17 (Private) · Issue #18 (Private) · Issue #19 (Private) · PR #36 (Private) | Project 화면 + 본인 작성 Issue + 연결된 PR |

## GitHub Project 및 Issue 관리 근거 (Project and Issue Management Evidence)

ForeShield Project 화면에서 확인된 Issue #15–#19의 공통 정보는 다음과 같습니다.

- 담당자: siaSim
- 영역: LLM/RAG
- 우선순위: P1
- 작업 유형: Feature 또는 Chore
- 상태: Done
- 연결된 PR: PR #36
- 목표 일자: Project 화면에 2026-08-04로 표시

Issue 본문과 PR #36의 연결 내용은 다음 작업 분해를 보여줍니다.

1. #15: AgentRunRequest Context Fixture
2. #16: Mock ToolProvider
3. #17: Mock RagProvider
4. #18: Mock AgentEngine streaming
5. #19: AgentEngine 계약·정책 테스트

이 기록을 근거로 “기능을 검증 가능한 단위로 쪼개고, 담당자·우선순위·상태·PR 연결을 관리했다”고 표현할 수 있습니다.

다만 GitHub Project 화면은 Project Item을 누가 생성했는지 별도로 노출하지 않습니다. 따라서 Issue 작성자와 Project Item 생성자를 동일하다고 표현하지 않습니다.

RAG 관련 Project Item #20–#23은 Project 화면에서 확인되지만 siaSim이 아닌 다른 담당자와 연결되어 있으므로 개인 기여로 집계하지 않았습니다.

## 기여 범위 및 상태 (Boundary and Status Notes)

- PR #218 (Private)의 실제 HTTP/SSE ConversationContext 주입 구현은 Nyahong 작성 PR입니다. 이 문서에서는 해당 기능을 siaSim 단독 구현으로 주장하지 않습니다.
- PR #254 (Private)은 현재 Open 상태이며, 변경 파일 테스트와 CI 기록은 별도 검토가 필요합니다.
- PR에 기록된 테스트 수치는 PR 작성자가 보고한 결과입니다. 이 포트폴리오 작성 과정에서 Private Repository의 전체 테스트를 재실행한 결과가 아닙니다.
- Azure OpenAI, Azure AI Search, PostgreSQL의 실제 Production E2E와 배포 상태는 이 GitHub 기록만으로 확정하지 않습니다.

## Commit 기록 (Commit History)

전체 Commit은 ForeShield/backend의 author:siaSim 활동 기록에서 확인할 수 있습니다.
