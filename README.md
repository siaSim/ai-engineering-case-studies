# AI Engineering Case Studies

AI Engineer building reliable LLM applications and backend systems with Agent, RAG, structured outputs, and safety-first execution policies.

LLM Agent와 RAG를 실제 Backend 서비스에 연결하고, Structured Output, Provenance, Safety, Fail-closed 정책까지 설계하는 AI Engineer 포트폴리오입니다.

## Focus Areas

- LLM Application / Agent orchestration
- RAG backend integration and retrieval contracts
- Context interpretation and intent-based tool routing
- Azure OpenAI and structured outputs
- Safety, PII protection, prompt-injection defense
- FastAPI backend, testing, CI, and deployment workflows

## Selected Projects

### 1. ForeShield — Climate Disaster Intelligence

복잡한 자연어 요청이 잘못된 Tool 실행으로 이어지는 문제를 줄이기 위해 Context Interpretation, AgentPlan, 필수 입력 검증을 포함한 Contract-based Agent Engine과 Intent-based Tool Router를 구현했습니다.

- Azure OpenAI 응답과 RAG 결과를 Structured Output, Provenance/Version propagation, fail-closed 정책으로 연결
- Tool orchestration 과정에서 결과 상태와 버전 정보를 보존하고, Token diagnostics와 회귀 테스트를 추가
- [ForeShield Case Study](projects/foreshield.md) · [Verified Evidence](evidence/foreshield.md)

### 2. PPYURIND — Emotion and Conflict Record Analysis

감정·갈등 기록을 안전하게 분석하기 위해 Azure OpenAI Structured Output, Emotion Analysis, Tone Conversion을 Backend 응답 계약에 연결했습니다.

- Azure AI Language PII Masking과 Safety Policy를 적용해 민감 정보와 위험 상황을 별도 처리
- 일반 대화와 Legal RAG 요청을 분리하고, Content Safety와 법률 정보 범위를 고려한 Chat Routing을 구현
- [PPYURIND Case Study](projects/ppyurind.md) · [Verified Evidence](evidence/ppyurind.md)

### 3. CellGuard AI — Battery Inspection Service

배터리 외관·CT 검사 결과를 서비스 화면과 리포트 흐름으로 연결하기 위해 Custom Vision 및 CT/DeepLab 관련 연동 코드를 작성했습니다.

- 검사 결과 저장과 Report UI를 구현해 검사 결과를 조회 가능한 형태로 연결
- Azure App Service 배포를 위한 GitHub Actions Workflow를 구성
- [CellGuard AI Case Study](projects/cellguard-ai.md) · [Verified Evidence](evidence/cellguard-ai.md)

## Tech Stack

**LLM / AI Application**  
Azure OpenAI, Structured Outputs, Prompt Engineering, Agent, RAG, Azure AI Search, Azure AI Language, Content Safety

**Backend / Reliability**  
Python, FastAPI, Pydantic, API contracts, error handling, fail-closed policies, provenance and version management

**ML / Computer Vision**  
PyTorch, DeepLab, OpenCV, Azure Custom Vision

**Engineering**  
GitHub, GitHub Actions, Azure App Service, automated tests, CI

## Evidence and Publication Scope

이 포트폴리오는 GitHub에서 확인 가능한 PR, Commit, Issue, 코드 경로, 테스트 기록을 기준으로 작성합니다. 프로젝트 전체 성과가 아니라 제가 확인 가능한 기여만 기술하며, 확인되지 않은 정확도·성능·사용자 수 등의 수치는 작성하지 않습니다.

ForeShield와 PPYURIND의 원본 Repository는 Private일 수 있어 외부 채용 담당자가 PR 또는 코드를 직접 확인하지 못할 수 있습니다. 이 Repository에는 Private source code, 내부 Prompt, API 계약, Secret, Azure Resource 정보, 내부 Endpoint, RAG Corpus, 팀 내부 데이터가 포함되지 않습니다.

## GitHub / Contact

- GitHub: [siaSim](https://github.com/siaSim)
- Contact: GitHub 프로필을 통해 연락 부탁드립니다.
