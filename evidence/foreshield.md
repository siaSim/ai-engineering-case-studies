# ForeShield Evidence

> This was a team project. The table below records only contributions that can be tied to my GitHub-authored PRs, Issues, commits, code paths, tests, or Project records.

ForeShield/backend is a private Repository. External readers may not be able to open the linked PRs, Issues, or files. No private source code, internal Prompt, API contract, Secret, Azure Resource, internal Endpoint, RAG Corpus, or team-internal data is reproduced here.

## Core Code Paths

- [Agent Engine](https://github.com/ForeShield/backend/blob/main/src/foreshield/agent/engine.py)
- [Context Interpretation](https://github.com/ForeShield/backend/blob/main/src/foreshield/agent/interpretation.py)
- [AgentPlan contracts](https://github.com/ForeShield/backend/blob/main/src/foreshield/contracts/agent_plan.py)
- [Intent-based Tool Router](https://github.com/ForeShield/backend/blob/main/src/foreshield/agent/router.py)
- [Azure OpenAI Provider](https://github.com/ForeShield/backend/blob/main/src/foreshield/agent/azure.py)
- [Result orchestration](https://github.com/ForeShield/backend/blob/main/src/foreshield/agent/orchestration.py)
- [Agent Event provenance](https://github.com/ForeShield/backend/blob/main/src/foreshield/contracts/agent_event.py)
- [Azure AI Search integration](https://github.com/ForeShield/backend/blob/main/src/foreshield/rag/azure_search.py)
- [UI Action policy](https://github.com/ForeShield/backend/blob/main/src/foreshield/ui_action_policy.py)
- [Agent Engine tests](https://github.com/ForeShield/backend/tree/main/tests)
- [Tool Router tests](https://github.com/ForeShield/backend/blob/main/tests/test_tool_router.py)
- [Provider tests](https://github.com/ForeShield/backend/blob/main/tests/test_agent_provider.py)
- [Context Interpretation tests](https://github.com/ForeShield/backend/blob/main/tests/test_agent_context_interpretation.py)

## Verified Contribution Table

| Feature | Contribution | PR/Commit | Evidence Level |
|---|---|---|---|
| AgentRunRequest fixtures | live/training Context Fixture와 검증 helper 구현 | [Issue #15](https://github.com/ForeShield/backend/issues/15) · [PR #36](https://github.com/ForeShield/backend/pull/36) | Direct Issue + authored PR |
| Contract-based Agent Engine | Mock Tool/RAG Provider, AgentEvent streaming, terminal event와 상태 정책 구현 | [Issue #16](https://github.com/ForeShield/backend/issues/16) · [Issue #17](https://github.com/ForeShield/backend/issues/17) · [Issue #18](https://github.com/ForeShield/backend/issues/18) · [Issue #19](https://github.com/ForeShield/backend/issues/19) · [PR #36](https://github.com/ForeShield/backend/pull/36) | Direct authored PR + tests |
| Conversation/SSE runtime boundary | AgentEngine을 Conversation/SSE 흐름에 연결하고 Tool execution·RAG provenance 정합성 보완 | [PR #54](https://github.com/ForeShield/backend/pull/54) · [Issue #49](https://github.com/ForeShield/backend/issues/49) · [Issue #50](https://github.com/ForeShield/backend/issues/50) · [Issue #51](https://github.com/ForeShield/backend/issues/51) · [Issue #52](https://github.com/ForeShield/backend/issues/52) · [Issue #53](https://github.com/ForeShield/backend/issues/53) | Direct authored PR; shared runtime boundary |
| Configurable Real RAG Provider | Mock/Real Provider와 local/Azure Search 경계를 연결하고 지역 필터·provenance 유지 | [Issue #71](https://github.com/ForeShield/backend/issues/71) · [PR #80](https://github.com/ForeShield/backend/pull/80) | Direct authored PR; Azure smoke separate |
| Azure OpenAI Response Provider | Azure Agent 응답 Provider와 기존 Agent 계약 연결 | [PR #102](https://github.com/ForeShield/backend/pull/102) | Direct authored PR |
| Azure token usage propagation | Azure completion usage를 Agent 경로로 전달 | [PR #120](https://github.com/ForeShield/backend/pull/120) | Direct authored PR |
| AgentPlan contracts | AgentPlan과 Conversation Context 계약, 실행 전 필수 필드 검증 정의 | [Issue #144](https://github.com/ForeShield/backend/issues/144) · [PR #187](https://github.com/ForeShield/backend/pull/187) | Direct authored PR |
| Context Interpretation | 자연어에서 intent, region, hazard, lead, time range, RAG 필요 여부를 구조화 | [Issue #144](https://github.com/ForeShield/backend/issues/144) · [PR #194](https://github.com/ForeShield/backend/pull/194) | Direct authored PR |
| Context contract hardening | Azure ContextInterpretation Structured Output, JSON fallback, 안전한 오류 진단 보완 | [PR #220](https://github.com/ForeShield/backend/pull/220) | Direct authored PR |
| Intent-based Tool Router | intent별 Tool sequence, clarification, RAG 추가, 병렬화 후보 정책 구현 | [Issue #145](https://github.com/ForeShield/backend/issues/145) · [PR #221](https://github.com/ForeShield/backend/pull/221) | Direct authored PR + unit tests |
| UI Action validation | allowlist, region/hazard/lead 검증, 실행 결과와 제안 목표 비교 | [PR #224](https://github.com/ForeShield/backend/pull/224) | Direct authored PR; frontend adapter shared boundary |
| Prompt Injection defense | Tool/RAG/Context 내용을 지시가 아닌 데이터로 처리하는 Prompt 경계와 회귀 테스트 추가 | [Issue #228](https://github.com/ForeShield/backend/issues/228) · [PR #230](https://github.com/ForeShield/backend/pull/230) | Direct authored PR + tests |
| Structured Answer output | Azure final answer에 strict JSON Schema와 JSON mode fallback 적용 | [Issue #232](https://github.com/ForeShield/backend/issues/232) · [PR #234](https://github.com/ForeShield/backend/pull/234) | Direct authored PR + focused tests |
| Token diagnostics | finish reason, response length, completion/reasoning token의 제한적 진단 추가 | [Issue #239](https://github.com/ForeShield/backend/issues/239) · [PR #241](https://github.com/ForeShield/backend/pull/241) | Direct authored PR + tests |
| Provider result orchestration | ToolResultLedger와 source/data/model/index version, reference time, 상태 보존 | [Issue #146](https://github.com/ForeShield/backend/issues/146) · [PR #245](https://github.com/ForeShield/backend/pull/245) | Direct authored PR + tests |
| Reasoning effort handling | 최종 Azure 응답에 제한된 reasoning effort 적용, schema fallback 정합성 검증 | [PR #248](https://github.com/ForeShield/backend/pull/248) | Direct authored PR + tests |
| Live/Training Tool policy | 모드별 Tool allowlist, 외부 영향 차단, 승인 전 Training Action 보류 | [Issue #147](https://github.com/ForeShield/backend/issues/147) · [PR #252](https://github.com/ForeShield/backend/pull/252) | Direct authored PR + tests |
| UI execution target validation | open_chart 실행 목표와 실제 화면 상태 불일치 시 fail-closed | [Issue #231](https://github.com/ForeShield/backend/issues/231) · [PR #255](https://github.com/ForeShield/backend/pull/255) | Direct authored PR; frontend contract shared |
| CI maintenance | Agent answer policy test import 정리로 Ruff CI 오류 수정 | [Issue #148](https://github.com/ForeShield/backend/issues/148) · [PR #258](https://github.com/ForeShield/backend/pull/258) | Direct authored PR; no logic change |
| GitHub Project task decomposition | Agent 기반 작업을 Fixture → Mock Tool → Mock RAG → AgentEngine → Test Issue로 분해하고 PR과 연결 | [ForeShield Project #1](https://github.com/orgs/ForeShield/projects/1) · [Issue #15](https://github.com/ForeShield/backend/issues/15) · [Issue #16](https://github.com/ForeShield/backend/issues/16) · [Issue #17](https://github.com/ForeShield/backend/issues/17) · [Issue #18](https://github.com/ForeShield/backend/issues/18) · [Issue #19](https://github.com/ForeShield/backend/issues/19) · [PR #36](https://github.com/ForeShield/backend/pull/36) | Project view + authored Issues + linked PR |

## Project and Issue Management Evidence

ForeShield Project 화면에서 확인된 Issue #15–#19의 공통 정보는 다음과 같습니다.

- Assignee: siaSim
- Area: LLM/RAG
- Priority: P1
- Work type: Feature 또는 Chore
- Status: Done
- Linked pull request: PR #36
- Target date: Project 화면에 2026-08-04로 표시

Issue 본문과 PR #36의 연결 내용은 다음 작업 분해를 보여줍니다.

1. #15: AgentRunRequest Context Fixture
2. #16: Mock ToolProvider
3. #17: Mock RagProvider
4. #18: Mock AgentEngine streaming
5. #19: AgentEngine 계약·정책 테스트

이 기록을 근거로 “기능을 검증 가능한 단위로 쪼개고, 담당자·우선순위·상태·PR 연결을 관리했다”고 표현할 수 있습니다.

다만 GitHub Project 화면은 Project Item을 누가 생성했는지 별도로 노출하지 않습니다. 따라서 Issue 작성자와 Project Item 생성자를 동일하다고 표현하지 않습니다.

RAG 관련 Project Item #20–#23은 Project 화면에서 확인되지만 siaSim이 아닌 다른 담당자와 연결되어 있으므로 개인 기여로 집계하지 않았습니다.

## Boundary and Status Notes

- [PR #218](https://github.com/ForeShield/backend/pull/218)의 실제 HTTP/SSE ConversationContext 주입 구현은 Nyahong 작성 PR입니다. 이 문서에서는 해당 기능을 siaSim 단독 구현으로 주장하지 않습니다.
- [PR #254](https://github.com/ForeShield/backend/pull/254)은 현재 Open 상태이며, 변경 파일 테스트와 CI 기록은 별도 검토가 필요합니다.
- PR에 기록된 테스트 수치는 PR 작성자가 보고한 결과입니다. 이 포트폴리오 작성 과정에서 Private Repository의 전체 테스트를 재실행한 결과가 아닙니다.
- Azure OpenAI, Azure AI Search, PostgreSQL의 실제 Production E2E와 배포 상태는 이 GitHub 기록만으로 확정하지 않습니다.

## Commit History

전체 Commit은 ForeShield/backend의 author:siaSim 활동 기록에서 확인할 수 있습니다.
