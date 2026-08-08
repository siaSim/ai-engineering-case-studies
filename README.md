# AI Engineering Case Studies

LLM Agent와 RAG를 Backend 시스템에 연결하고, Structured Output·Provenance·Safety·Fail-closed 정책을 통해 신뢰할 수 있는 AI Application을 설계·구현해 온 과정을 정리한 기술 Case Study입니다.

## 핵심 기술 영역 (Focus Areas)

- LLM Agent, Context Interpretation, AgentPlan, Intent-based Tool Router
- RAG Backend, Azure OpenAI, Azure AI Search, Structured Output
- Orchestration, Provenance / Version propagation, Reliability / Safety, fail-closed 정책
- PII Protection, Content Safety, AI Backend, 테스트·CI
- Computer Vision 결과를 검사 서비스·저장·리포트·배포 흐름에 연결하는 AI Service Integration

## 주요 프로젝트 (Selected Projects)

### 1. ForeShield — 기후재난 인텔리전스

**역할:** LLM Agent / RAG / Backend · **기간:** 2026.07 – 진행 중 · **Team Project**

- 모호한 자연어 요청이 잘못된 Tool 실행으로 이어질 가능성을 줄이기 위해 Context Interpretation → AgentPlan → Intent-based Tool Router 계약을 구현했습니다.
- Azure OpenAI와 RAG 결과를 Structured Output, Orchestration, Provenance / Version propagation으로 연결하고 fail-closed·Prompt Injection 방어·Token diagnostics·회귀 테스트를 적용했습니다.

[ForeShield Case Study](projects/foreshield.md) · [검증된 근거](evidence/foreshield.md)

### 2. PPYURIND — 감정·갈등 기록 분석

**역할:** AI Backend / LLM Application · **기간:** 2026.06.29–2026.07.05 (siaSim PR 기록 기준) · **Team Project**

- text / voice / image 입력을 STT·OCR과 PII Protection을 거쳐 Structured LLM Analysis로 연결하고, Azure AI 기반 Emotion Analysis·Tone Conversion 응답 계약을 확장했습니다.
- Safety Policy·Content Safety를 적용하고, 일반 상담과 Legal RAG를 분리하는 RAG Chat Routing 및 법률 정보 범위를 구현했습니다.
- Responsible AI Hard Set 60건과 General Stability Set 120건을 대상으로 Python API Eval Harness를 구성·실행하고, Stability 120/120 HTTP 200·응답 구조 안정성 100% 및 Hard Set Recall 33.3%·F1 37.5%를 측정해 개선 우선순위를 도출했습니다.

[PPYURIND Case Study](projects/ppyurind.md) · [검증된 근거](evidence/ppyurind.md)

### 3. CellGuard AI — 배터리 검사 서비스

**역할:** Team Lead / AI Service Integration / Computer Vision Application · **기간:** 2026.05 · **Team Project**

- Azure Custom Vision과 CT / DeepLab inference 결과를 검사 애플리케이션에 통합했습니다.
- 검사 결과 저장·이력 필터·상세 리포트 흐름을 구현하고, GitHub Actions + Azure App Service 기반 Streamlit 프로토타입 배포·시연 흐름을 구성했습니다.

[CellGuard AI Case Study](projects/cellguard-ai.md) · [검증된 근거](evidence/cellguard-ai.md)

## 기술 스택 (Tech Stack)

**LLM / AI Application**
Azure OpenAI, AgentPlan, Context Interpretation, Intent-based Tool Router, RAG, Structured Output, Azure AI Search, Azure AI Language, Content Safety

**Backend / 신뢰성**
Python, FastAPI, Pydantic, Orchestration, provenance / version propagation, fail-closed policies, 자동화 테스트, CI

**Computer Vision / Service Integration**
Azure Custom Vision, DeepLab, PyTorch

**개발·배포 환경 (Engineering & Delivery)**
GitHub Actions, Azure App Service

## 근거 및 공개 범위 (Evidence & Scope)

PR·Commit·Issue·코드 경로·테스트로 확인 가능한 개인 기여만 기록하며, 확인되지 않은 정확도·성능·사용자 수 등의 정량 수치는 사용하지 않습니다. Private source code, 내부 Prompt·API 계약, Secret, Azure Resource·Endpoint, RAG Corpus, 팀 내부 데이터는 포함하지 않습니다. Private 원본의 PR·코드 링크는 외부 채용 담당자에게 보이지 않을 수 있습니다.

## GitHub / 연락처 (Contact)

- GitHub: [siaSim](https://github.com/siaSim)
- Contact: GitHub 프로필을 통해 연락 부탁드립니다.
