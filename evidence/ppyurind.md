# PPYURIND Evidence

> This ledger records only publicly linkable GitHub metadata and generalized descriptions. The original PPYURIND backend is private; no source code, PR diff, internal prompt, API contract, secret, Azure resource name, endpoint, or RAG corpus is copied here.

## Scope & Attribution

The current backend history confirms siaSim-authored PRs across the MVP, media, emotion, safety, taxonomy, PII, routing, and reliability work. This evidence page foregrounds the PRs relevant to the Case Study rather than presenting every repository change as an AI contribution.

Initial implementations by teammates are kept as attribution boundaries. In particular, PR #6 and PR #8 by `my0614` contain the initial Azure OpenAI emotion-analysis / Content Safety integration, and PR #13 by `my0614` contains the initial Tone Conversion implementation. The Case Study claims only siaSim-authored extensions and follow-up policy, contract, masking, routing, and reliability work.

## Verified Contribution Ledger

| Feature | Contribution | PR/Commit | Evidence Level |
|---|---|---|---|
| Tone Conversion response contract | Azure success, backend fallback, empty/error fallback을 `source` 값으로 구분하고 기존 응답 필드와 호환되게 확장 | [PR #20](https://github.com/AIStreetFighter/ppyurind-backend/pull/20) · [Commit 135ed38](https://github.com/AIStreetFighter/ppyurind-backend/commit/135ed388a992c971ac227990ea529f267c41d3f1) | Direct PR metadata + changed schema/service/test files |
| Structured emotion analysis extension | Cognitive separation, keyword sentiment, score clamp, label normalization을 Azure OpenAI 결과와 API response에 연결 | [PR #30](https://github.com/AIStreetFighter/ppyurind-backend/pull/30) · [Commit 6142601](https://github.com/AIStreetFighter/ppyurind-backend/commit/614260199198a1ba957b0a472dec5a3b187d6d83) | Direct PR metadata + changed schema/service/provider/test files |
| PII masking provider | Azure AI Language PII Detection을 공통 masking 경로에 연결하고 설정 누락·호출 실패 시 regex fallback 및 async wrapper 유지 | [PR #28](https://github.com/AIStreetFighter/ppyurind-backend/pull/28) · [Commit d291776](https://github.com/AIStreetFighter/ppyurind-backend/commit/d291776d7e4b79565e1083e7d8286b5fec33b8ee) | Direct PR metadata + changed provider/util/router/service/test files |
| AI Safety Policy | `emergency`를 optional meta/safety로 보존하면서 legacy `danger` 저장 호환성 유지; 긴급 정책과 법률 표현 제한을 중앙화 | [PR #26](https://github.com/AIStreetFighter/ppyurind-backend/pull/26) · [Commit fe02d51](https://github.com/AIStreetFighter/ppyurind-backend/commit/fe02d51bb5933faf30a6d053329ea4f642f5c39a) | Direct PR metadata + policy/schema/test patch; 15 passed recorded in commit |
| Label taxonomy versioning | 감정·갈등·Safety·RAG 신뢰도·AI Tone 등 공통 label group과 helper를 추가하고 legacy alias를 정규화 | [PR #27](https://github.com/AIStreetFighter/ppyurind-backend/pull/27) · [Commit 22c688b](https://github.com/AIStreetFighter/ppyurind-backend/commit/22c688b4f61de564cada8913b66a12e48859b479) | Direct PR metadata + taxonomy/test patch; 21 passed recorded in commit |
| Legal / general RAG routing | 현재 질문과 최근 user history를 기준으로 법률 맥락에서만 Search data source를 연결하고 일반 관계 상담과 분리 | [PR #31](https://github.com/AIStreetFighter/ppyurind-backend/pull/31) · [Commit 31fc594](https://github.com/AIStreetFighter/ppyurind-backend/commit/31fc594d45a9a5532550be9e3f3056dfa1f8fa08) | Direct PR metadata + chat/law_rag/test patch |
| Safety routing in chat | Content Safety 결과, 위험 표현, 부정문·인용 문맥을 결합해 Safety Card 우선 여부를 구분하고 RAG/LLM fallback을 유지 | [PR #31](https://github.com/AIStreetFighter/ppyurind-backend/pull/31) | Direct PR metadata + chat service tests; no quality metric claimed |
| Legal RAG specificity | 불륜·도박·접근금지 등 근거가 약한 주제는 결과 단정 대신 상담 전 기록 정리와 전문가 상담 범위로 제한; 주제별 guidance 분리 | [PR #32](https://github.com/AIStreetFighter/ppyurind-backend/pull/32) · [Commit 4868d92](https://github.com/AIStreetFighter/ppyurind-backend/commit/4868d929e87dbabb16425ea3cc2c5a394a633ced) | Direct PR metadata + law_rag/test patch |
| Emotion record deduplication | 같은 사용자·최근 시간 창·유사도 기준으로 기존 기록 update 또는 신규 create를 선택하고 빈 입력을 거절 | [PR #22](https://github.com/AIStreetFighter/ppyurind-backend/pull/22) · [Commit bf518c2](https://github.com/AIStreetFighter/ppyurind-backend/commit/bf518c2319e86c684734438a7689cf72f36941e7) | Direct PR metadata + repository/service/schema/test patch |
| Record timestamp reliability | 중복 update 시 기록 시각을 갱신해 최신 분석 기준을 유지 | [PR #33](https://github.com/AIStreetFighter/ppyurind-backend/pull/33) · [Commit 73d74d5](https://github.com/AIStreetFighter/ppyurind-backend/commit/73d74d5d62d6f3557d311285ab890f93c54b17e6) | Direct PR metadata + service/test patch; 7 passed in PR record |
| Report date boundary | 종료일 당일 기록이 누락되지 않도록 기간 경계를 반개방 구간으로 정리 | [PR #24](https://github.com/AIStreetFighter/ppyurind-backend/pull/24) · [Commit 1a4c1c5](https://github.com/AIStreetFighter/ppyurind-backend/commit/1a4c1c5d64a8b46abdbe5282404f03ad4610fbd6) | Direct PR metadata + report/test patch; 69 passed recorded in commit |
| Emotion record storage/share boundary | 분석 결과를 저장·조회·공유 흐름에 연결하고 private 기본 상태와 source record 연결을 보강 | [PR #19](https://github.com/AIStreetFighter/ppyurind-backend/pull/19) · [Commit c7f4f9c](https://github.com/AIStreetFighter/ppyurind-backend/commit/c7f4f9cdddfbb47801669e9deae905d1f0bfd61a) | Direct PR metadata + changed emotion/community files |
| Authenticated multimodal media API | 인증된 `/media` 경계와 `text`/`voice`/`image` 입력 타입을 추가하고, 업로드 확장자·MIME·파일 크기를 검증 | [PR #16](https://github.com/AIStreetFighter/ppyurind-backend/pull/16) · [Commit e1862bf](https://github.com/AIStreetFighter/ppyurind-backend/commit/e1862bf7b32bf370b96017d6d3c3ab63e43e6085) | Direct commit diff + changed router/schema/service/test files |
| Browser realtime STT token flow | 서버 키를 직접 반환하지 않고 short-lived Azure Speech token과 region을 반환하는 인증 endpoint 및 브라우저 SDK 연결 안내를 추가; 테스트는 `expires_in`과 key 미노출을 확인 | [PR #16](https://github.com/AIStreetFighter/ppyurind-backend/pull/16) · [Commit e1862bf](https://github.com/AIStreetFighter/ppyurind-backend/commit/e1862bf7b32bf370b96017d6d3c3ab63e43e6085) | Direct commit diff + media README/router/service/test patch |
| Uploaded audio STT fallback/test endpoint | `POST /media/stt`에서 업로드 오디오를 Blob에 저장한 뒤 Azure Speech single-utterance 인식 결과를 반환 | [PR #16](https://github.com/AIStreetFighter/ppyurind-backend/pull/16) · [Commit e1862bf](https://github.com/AIStreetFighter/ppyurind-backend/commit/e1862bf7b32bf370b96017d6d3c3ab63e43e6085) | Direct commit diff + media service/test patch; no alternate-provider fallback claimed |
| Azure Vision OCR and Blob upload | 이미지 업로드를 검증하고 Blob upload 후 Azure Vision Read API로 텍스트를 추출해 `original_text`와 `masked_text`를 반환 | [PR #16](https://github.com/AIStreetFighter/ppyurind-backend/pull/16) · [Commit e1862bf](https://github.com/AIStreetFighter/ppyurind-backend/commit/e1862bf7b32bf370b96017d6d3c3ab63e43e6085) | Direct commit diff + vision/blob service and media test patch |
| Media result to privacy-safe analysis/persistence | STT/OCR 결과를 masking한 텍스트로 Emotion Analysis와 records/emotion 저장 경계에 전달하도록 입력 계약·서비스를 연결 | [PR #16](https://github.com/AIStreetFighter/ppyurind-backend/pull/16) · [Commit e1862bf](https://github.com/AIStreetFighter/ppyurind-backend/commit/e1862bf7b32bf370b96017d6d3c3ab63e43e6085) | Direct commit diff + emotion/records service and masking test patch |
| Azure Speech SDK runtime handling | 업로드 STT의 짧은 fallback/test flow에서 `asyncio.to_thread` 대신 직접 Speech SDK를 실행하도록 수정하고, cancellation detail의 secret/token을 redact하는 테스트를 추가 | [Commit 6319d1e](https://github.com/AIStreetFighter/ppyurind-backend/commit/6319d1e17981d669c007282ab5f0c85faba1bae7) | Direct siaSim commit diff + speech/media test patch |

## Related siaSim PRs Not Foregrounded

다음 PR도 siaSim 작성자로 확인되지만, 이 Case Study의 AI 핵심 메시지를 흐리지 않도록 본문 대표 근거에서는 제외했습니다.

- MVP / OAuth / schema: [PR #1](https://github.com/AIStreetFighter/ppyurind-backend/pull/1), [#2](https://github.com/AIStreetFighter/ppyurind-backend/pull/2), [#3](https://github.com/AIStreetFighter/ppyurind-backend/pull/3), [#4](https://github.com/AIStreetFighter/ppyurind-backend/pull/4), [#7](https://github.com/AIStreetFighter/ppyurind-backend/pull/7)
- Community / backend reliability: [PR #34](https://github.com/AIStreetFighter/ppyurind-backend/pull/34), [#35](https://github.com/AIStreetFighter/ppyurind-backend/pull/35), [#36](https://github.com/AIStreetFighter/ppyurind-backend/pull/36), [#37](https://github.com/AIStreetFighter/ppyurind-backend/pull/37), [#38](https://github.com/AIStreetFighter/ppyurind-backend/pull/38)

## Attribution Boundaries

- [PR #6](https://github.com/AIStreetFighter/ppyurind-backend/pull/6) and [PR #8](https://github.com/AIStreetFighter/ppyurind-backend/pull/8) are authored by `my0614`; they contain the initial Azure OpenAI emotion-analysis / Content Safety integration. They are team context, not siaSim implementation evidence.
- [PR #13](https://github.com/AIStreetFighter/ppyurind-backend/pull/13) is authored by `my0614`; it contains the initial Tone Conversion implementation. siaSim's verified contribution is the later response-source and fallback contract in PR #20.
- [PR #12](https://github.com/AIStreetFighter/ppyurind-backend/pull/12), [PR #21](https://github.com/AIStreetFighter/ppyurind-backend/pull/21), and [PR #23](https://github.com/AIStreetFighter/ppyurind-backend/pull/23) are authored by `y1nature`; they provide team context for earlier RAG integration/cleanup, not personal ownership claims here.
- Frontend PRs and the overall product UI are not used as evidence of Backend implementation ownership.

## Validation Summary

| Validation type | Confirmed evidence | Boundary |
|---|---|---|
| Policy / taxonomy tests | Commit `fe02d51`: 15 passed; Commit `22c688b`: 21 passed | PR-level recorded results; not a fresh rerun in this portfolio repository |
| Dedup / timestamp tests | PR #33: 7 passed; PR #22 includes create/update, user scope, empty input scenarios | No claim beyond recorded tests |
| Report boundary tests | Commit `1a4c1c5`: 69 passed | Recorded commit result |
| PII masking tests | PR #28 adds tests and mocks Azure calls | No PII precision/recall or masking accuracy metric confirmed |
| RAG routing / legal scope | PR #31/#32 add targeted routing and topic-specific test scenarios | No answer-quality benchmark or legal correctness metric confirmed |
| LLM evaluation | No public evaluation dataset, accuracy, Recall, F1, latency, or user metric found in the inspected records | Not claimed |

## Publication Policy

This public evidence page uses generalized descriptions and links only. It does not reproduce private code, PR diffs, internal prompts, API contracts, environment variables, Azure resource names, internal endpoints, RAG corpus content, or personal/team data. Private PR links may be inaccessible to external reviewers.
