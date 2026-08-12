# PPYURIND 기여 근거 (Evidence)

> 이 장부는 공개적으로 연결 가능한 GitHub 메타데이터와 일반화한 설명만 기록합니다. PPYURIND Backend 원본은 private이며, source code, PR diff, 내부 Prompt, API contract, Secret, Azure Resource 이름, Endpoint, RAG Corpus는 복사하지 않습니다.

## 기여 범위 및 작성 원칙 (Scope & Attribution)

현재 Backend 기록에서 siaSim이 MVP, media, emotion, safety, taxonomy, PII, routing, reliability 영역의 PR을 작성한 사실을 확인할 수 있습니다. 이 문서는 모든 Repository 변경을 AI 기여로 제시하지 않고, Case Study와 직접 연결되는 PR을 중심으로 정리합니다.

- Team architecture/data-flow reference: 프로젝트 결과보고서의 팀 전체 설계 자료입니다. 전체 시스템 맥락 설명용이며 개인 구현 근거로 사용하지 않습니다.

### Supplemental Evaluation Evidence

아래 평가 결과는 GitHub PR·Commit 기반 소프트웨어 Evidence와 구분한 프로젝트 평가 결과입니다. 평가 원본 Eval Harness와 결과 파일은 이 공개 Repository에 포함하지 않습니다.

- **Python Eval Harness**
- **Responsible AI Hard Set:** 60건
- **General Stability Set:** 120건
- **총 평가 건수:** 180건
- **General Stability Set:** 120/120 HTTP 200
- **응답 구조 안정성:** 100%
- **Responsible AI Hard Set 위험 Recall:** 33.3%
- **Responsible AI Hard Set F1:** 37.5%

이 결과는 이후 개선 우선순위를 도출하는 데 사용했습니다. Hard Set의 낮은 Recall/F1 결과도 관찰된 결과 그대로 기록하며, GitHub Commit으로 확인한 수치처럼 표현하지 않습니다.

팀원이 작성한 초기 구현은 개인 기여와 구분했습니다. 특히 `my0614`의 PR #6·#8에는 초기 Azure OpenAI emotion analysis / Content Safety 연동이, PR #13에는 초기 Tone Conversion 구현이 포함되어 있습니다. Case Study에서는 siaSim이 작성한 후속 policy, contract, masking, routing, reliability 작업만 개인 기여로 표현합니다.

## 검증된 개인 기여 (Verified Contributions)

| 기능 | 개인 기여 | PR / Commit | 근거 수준 |
|---|---|---|---|
| Tone Conversion response contract | Azure success, backend fallback, empty/error fallback을 `source` 값으로 구분하고 기존 응답 필드와 호환되게 확장 | PR #20 (Private) · Commit 135ed38 (Private) | PR 메타데이터 직접 확인 + schema/service/test 파일 변경 |
| Structured emotion analysis extension | Cognitive separation, keyword sentiment, score clamp, label normalization을 Azure OpenAI 결과와 API response에 연결 | PR #30 (Private) · Commit 6142601 (Private) | PR 메타데이터 직접 확인 + schema/service/provider/test 파일 변경 |
| PII masking provider | Azure AI Language PII Detection을 공통 masking 경로에 연결하고 설정 누락·호출 실패 시 regex fallback 및 async wrapper 유지 | PR #28 (Private) · Commit d291776 (Private) | PR 메타데이터 직접 확인 + provider/util/router/service/test 파일 변경 |
| AI Safety Policy | `emergency`를 optional meta/safety로 보존하면서 legacy `danger` 저장 호환성 유지; 긴급 정책과 법률 표현 제한을 중앙화 | PR #26 (Private) · Commit fe02d51 (Private) | PR 메타데이터 직접 확인 + policy/schema/test patch; Commit 기록상 15 passed |
| Label taxonomy versioning | 감정·갈등·Safety·RAG 신뢰도·AI Tone 등 공통 label group과 helper를 추가하고 legacy alias를 정규화 | PR #27 (Private) · Commit 22c688b (Private) | PR 메타데이터 직접 확인 + taxonomy/test patch; Commit 기록상 21 passed |
| Legal / general RAG routing | 현재 질문과 최근 user history를 기준으로 법률 맥락에서만 Search data source를 연결하고 일반 관계 상담과 분리 | PR #31 (Private) · Commit 31fc594 (Private) | PR 메타데이터 직접 확인 + chat/law_rag/test patch |
| Safety routing in chat | Content Safety 결과, 위험 표현, 부정문·인용 문맥을 결합해 Safety Card 우선 여부를 구분하고 RAG/LLM fallback을 유지 | PR #31 (Private) | PR 메타데이터 직접 확인 + chat service 테스트; 품질 지표는 주장하지 않음 |
| Legal RAG specificity | 불륜·도박·접근금지 등 근거가 약한 주제는 결과 단정 대신 상담 전 기록 정리와 전문가 상담 범위로 제한; 주제별 guidance 분리 | PR #32 (Private) · Commit 4868d92 (Private) | PR 메타데이터 직접 확인 + law_rag/test patch |
| Emotion record deduplication | 같은 사용자·최근 시간 창·유사도 기준으로 기존 기록 update 또는 신규 create를 선택하고 빈 입력을 거절 | PR #22 (Private) · Commit bf518c2 (Private) | PR 메타데이터 직접 확인 + repository/service/schema/test patch |
| Record timestamp reliability | 중복 update 시 기록 시각을 갱신해 최신 분석 기준을 유지 | PR #33 (Private) · Commit 73d74d5 (Private) | PR 메타데이터 직접 확인 + service/test patch; PR 기록상 7 passed |
| Report date boundary | 종료일 당일 기록이 누락되지 않도록 기간 경계를 반개방 구간으로 정리 | PR #24 (Private) · Commit 1a4c1c5 (Private) | PR 메타데이터 직접 확인 + report/test patch; Commit 기록상 69 passed |
| Emotion record storage/share boundary | 분석 결과를 저장·조회·공유 흐름에 연결하고 private 기본 상태와 source record 연결을 보강 | PR #19 (Private) · Commit c7f4f9c (Private) | PR 메타데이터 직접 확인 + emotion/community 파일 변경 |
| Authenticated multimodal media API | 인증된 `/media` 경계와 `text`/`voice`/`image` 입력 타입을 추가하고, 업로드 확장자·MIME·파일 크기를 검증 | PR #16 (Private) · Commit e1862bf (Private) | Direct commit diff + changed router/schema/service/test files |
| Browser realtime STT token flow | 서버 키를 직접 반환하지 않고 short-lived Azure Speech token과 region을 반환하는 인증 endpoint 및 브라우저 SDK 연결 안내를 추가; 테스트는 `expires_in`과 key 미노출을 확인 | PR #16 (Private) · Commit e1862bf (Private) | Direct commit diff + media README/router/service/test patch |
| Uploaded audio STT fallback/test endpoint | `POST /media/stt`에서 업로드 오디오를 Blob에 저장한 뒤 Azure Speech single-utterance 인식 결과를 반환 | PR #16 (Private) · Commit e1862bf (Private) | Direct commit diff + media service/test patch; no alternate-provider fallback claimed |
| Azure Vision OCR and Blob upload | 이미지 업로드를 검증하고 Blob upload 후 Azure Vision Read API로 텍스트를 추출해 `original_text`와 `masked_text`를 반환 | PR #16 (Private) · Commit e1862bf (Private) | Direct commit diff + vision/blob service and media test patch |
| Media result to privacy-safe analysis/persistence | STT/OCR 결과를 masking한 텍스트로 Emotion Analysis와 records/emotion 저장 경계에 전달하도록 입력 계약·서비스를 연결 | PR #16 (Private) · Commit e1862bf (Private) | Direct commit diff + emotion/records service and masking test patch |
| Azure Speech SDK runtime handling | 업로드 STT의 짧은 fallback/test flow에서 `asyncio.to_thread` 대신 직접 Speech SDK를 실행하도록 수정하고, cancellation detail의 secret/token을 redact하는 테스트를 추가 | Commit 6319d1e (Private) | Direct siaSim commit diff + speech/media test patch |

## 본문에 전면 배치하지 않은 관련 siaSim PR (Related PRs)

다음 PR도 siaSim 작성자로 확인되지만, 이 Case Study의 AI 핵심 메시지를 흐리지 않도록 본문 대표 근거에서는 제외했습니다.

- MVP / OAuth / schema: PR #1 (Private), PR #2 (Private), PR #3 (Private), PR #4 (Private), PR #7 (Private)
- Community / Backend 신뢰성: PR #34 (Private), PR #35 (Private), PR #36 (Private), PR #37 (Private), PR #38 (Private)

## 팀 기여와 개인 기여의 경계 (Attribution Boundaries)

- PR #6 (Private)과 PR #8 (Private)은 `my0614`가 작성했으며 초기 Azure OpenAI emotion analysis / Content Safety 연동을 포함합니다. 이는 팀 맥락이며 siaSim 구현 근거로 사용하지 않습니다.
- PR #13 (Private)은 `my0614`가 작성했으며 초기 Tone Conversion 구현을 포함합니다. siaSim의 확인 가능한 기여는 PR #20의 후속 response source 및 fallback contract입니다.
- PR #12 (Private), PR #21 (Private), PR #23 (Private)은 `y1nature`가 작성했으며, 초기 RAG 연동·정리 작업의 팀 맥락일 뿐 이 문서의 개인 소유권 주장이 아닙니다.
- Frontend PR과 전체 제품 UI는 Backend 구현 소유권의 근거로 사용하지 않습니다.

## 검증 요약 (Validation Summary)

| 검증 유형 | 확인된 근거 | 범위 및 한계 |
|---|---|---|
| Policy / taxonomy tests | Commit `fe02d51`: 15 passed; Commit `22c688b`: 21 passed | PR에 기록된 결과이며 이 Portfolio Repository에서 새로 재실행한 결과는 아님 |
| Dedup / timestamp tests | PR #33: 7 passed; PR #22에는 create/update, user scope, empty input 시나리오 포함 | 기록된 테스트 이상은 주장하지 않음 |
| Report boundary tests | Commit `1a4c1c5`: 69 passed | Commit에 기록된 결과 |
| PII masking tests | PR #28에 테스트가 추가되었고 Azure 호출은 mock 처리 | PII precision/recall 또는 masking accuracy 지표는 확인되지 않음 |
| RAG routing / legal scope | PR #31/#32에 routing 및 주제별 테스트 시나리오 추가 | 답변 품질 benchmark 또는 법률 정확성 지표는 확인되지 않음 |
| LLM evaluation | Supplemental Evaluation Evidence에 Responsible AI Hard Set 60건·General Stability Set 120건과 측정 결과를 별도 기록 | 평가 원본은 이 공개 Repository에 포함하지 않으며, GitHub PR·Commit으로 확인한 수치처럼 표현하지 않음 |

## 공개 범위 원칙 (Publication Policy)

이 공개 Evidence 문서는 일반화한 설명과 링크만 사용합니다. private code, PR diff, 내부 Prompt, API contract, 환경변수, Azure Resource 이름, 내부 Endpoint, RAG Corpus 내용, 개인정보·팀 내부 데이터는 재현하지 않습니다. Private PR 링크는 외부 검토자에게 보이지 않을 수 있습니다.
