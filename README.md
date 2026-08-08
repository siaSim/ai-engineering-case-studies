# AI Engineering Case Studies

LLM Agent와 RAG를 Backend 시스템에 연결하고, Structured Output·Provenance·Safety·Fail-closed 정책을 통해 신뢰할 수 있는 AI Application을 설계·구현해 온 과정을 정리한 기술 Case Study입니다.

## 핵심 기술 영역 (Focus Areas)

- LLM Application / Agent orchestration
- RAG Backend 연동 및 retrieval contract
- Context Interpretation 및 Intent-based Tool Routing
- Azure OpenAI 및 Structured Output
- Safety, PII 보호 및 Prompt Injection 방어
- FastAPI Backend, 테스트, CI 및 배포 워크플로우

## 주요 프로젝트 (Selected Projects)

### 1. ForeShield — 기후재난 인텔리전스

**역할:** LLM Agent / RAG / Backend · **기간:** 진행 중 · **Team Project**

자연어 요청이 잘못된 Tool 실행으로 이어질 가능성을 줄이기 위해 Context Interpretation, AgentPlan, 필수 입력 검증을 포함한 Contract-based Agent Engine과 Intent-based Tool Router를 구현했습니다.

- Azure OpenAI 응답과 RAG 결과를 Structured Output, Provenance/Version propagation, fail-closed 정책으로 연결
- Tool orchestration 과정에서 결과 상태와 버전 정보를 보존하고, Token diagnostics와 회귀 테스트를 추가
- [ForeShield Case Study](projects/foreshield.md) · [검증된 근거](evidence/foreshield.md)

### 2. PPYURIND — 감정·갈등 기록 분석

**역할:** Backend / Azure AI / RAG / Safety & PII · **기간:** Microsoft AI School 10기 2차 프로젝트 · **Team Project**

감정·갈등 기록을 구조화하고 안전하게 분석하기 위해 Azure OpenAI Structured Output, Emotion Analysis, Tone Conversion을 Backend 응답 계약에 연결했습니다.

- Azure AI Language PII Masking과 Safety Policy를 적용해 민감 정보와 위험 상황을 별도 처리
- 일반 대화와 Legal RAG 요청을 분리하고, Content Safety와 법률 정보 범위를 고려한 Chat Routing을 구현
- [PPYURIND Case Study](projects/ppyurind.md) · [검증된 근거](evidence/ppyurind.md)

### 3. CellGuard AI — 배터리 검사 서비스

**역할:** AI Integration / Inspection Report / Deployment Workflow · **기간:** Microsoft AI School 10기 1차 프로젝트 · **Team Project**

배터리 외관·CT 검사 결과를 서비스 화면과 리포트 흐름으로 연결하기 위해 Custom Vision 및 CT/DeepLab 관련 연동 코드를 작성했습니다.

- 검사 결과 저장과 Report UI를 구현해 검사 결과를 조회 가능한 형태로 연결
- Azure App Service 배포를 위한 GitHub Actions Workflow를 구성
- [CellGuard AI Case Study](projects/cellguard-ai.md) · [검증된 근거](evidence/cellguard-ai.md)

## 기술 스택 (Tech Stack)

**LLM / AI Application**  
Azure OpenAI, Agent, RAG, Structured Output, Azure AI Search, Azure AI Language, Content Safety

**Backend / 신뢰성**
Python, FastAPI, Pydantic, error handling, fail-closed policies, provenance/version propagation, 자동화 테스트

**ML / Computer Vision**  
PyTorch, DeepLab, Azure Custom Vision

**개발 환경 (Engineering)**
GitHub Actions, Azure App Service, CI

## 근거 및 공개 범위 (Evidence & Scope)

PR·Commit·Issue·코드 경로·테스트로 확인 가능한 개인 기여만 기록하며, 확인되지 않은 정확도·성능·사용자 수 등의 정량 수치는 사용하지 않습니다. Private source code, 내부 Prompt·API 계약, Secret, Azure Resource·Endpoint, RAG Corpus, 팀 내부 데이터는 포함하지 않습니다. Private 원본의 PR·코드 링크는 외부 채용 담당자에게 보이지 않을 수 있습니다.

## GitHub / 연락처 (Contact)

- GitHub: [siaSim](https://github.com/siaSim)
- Contact: GitHub 프로필을 통해 연락 부탁드립니다.
