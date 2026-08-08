# CellGuard AI 기여 근거 (Evidence)

> CellGuard Repository는 public이며, 아래 링크는 외부에서 확인 가능한 Commit과 공개 코드 경로입니다. 이 장부는 `siaSim` authored Commit에서 확인되는 개인 기여만 정리하며, 팀 전체 기능·모델 학습 성과·정량 성능을 개인 성과로 확장하지 않습니다.

## 핵심 코드 경로 (Core Code Paths)

- [Azure Custom Vision integration](https://github.com/ms-ai-school-10th-team3/battery/blob/main/services/custom_vision_out.py)
- [Exterior inspection UI and result normalization](https://github.com/ms-ai-school-10th-team3/battery/blob/main/pages/exterior_inspection.py)
- [DeepLab inference pipeline](https://github.com/ms-ai-school-10th-team3/battery/blob/main/ai_models/deeplab_mobilenet/predict.py)
- [CT inspection and result persistence](https://github.com/ms-ai-school-10th-team3/battery/blob/main/pages/ct_inspection.py)
- [Report storage](https://github.com/ms-ai-school-10th-team3/battery/blob/main/utils/report_storage.py)
- [Inspection history and filtering](https://github.com/ms-ai-school-10th-team3/battery/blob/main/pages/inspection_report.py)
- [Report detail UI](https://github.com/ms-ai-school-10th-team3/battery/blob/main/pages/report.py)
- [Azure App Service workflow](https://github.com/ms-ai-school-10th-team3/battery/blob/main/.github/workflows/main_battery-main.yml)

## 보조 프로젝트 근거 (Supplemental Project Evidence)

- **Team architecture reference:** 프로젝트 발표·설계 자료로, 전체 시스템 맥락 확인용이며 개인 구현 근거로 사용하지 않습니다.

- **Final presentation:** 최종 발표자료의 팀 소개 슬라이드에서 `팀장 심시아`와 담당 영역(Classification 모델, 모델 통합, 모델 평가, Streamlit UI, Azure 배포, 일정 관리·역할 분배)을 확인했습니다. 이는 Team Lead 및 프로젝트 역할에 대한 보조 근거이며, 모든 모델·Frontend·Architecture를 혼자 구현했다는 근거가 아닙니다.
- **Prototype demonstration:** 같은 발표자료에 Azure-Streamlit end-to-end 연동 성공 기록과 당시 서비스 시연 화면이 포함되어 있어, GitHub Actions workflow와 함께 Azure App Service 기반 Streamlit 프로토타입 배포·시연 수준을 보조합니다. Production 운영·실제 사용자 트래픽·SLA·장기 가동률의 근거로 사용하지 않습니다.
- **Project Wiki:** `03-Model-Experiments`, `05-1-Classification-Model-Comparison`, `04-Final-Model-Selection` 기록에서 MobileNetV2·ResNet18 Classification baseline과 Detection/Segmentation 후보 비교, 최종 MVP의 외관 `Custom Vision Object Detection`·CT `MobileNet + DeepLab` 선택 기준을 확인했습니다. 이는 팀 프로젝트의 모델 선택 기록이며, 개인 단독 학습·모델 평가 성과의 직접 Evidence로 취급하지 않습니다.

발표자료·Wiki 원본은 Google Drive에서 검토했지만 Portfolio Repository에 복사하거나 재배포하지 않았습니다. 발표자료에 포함된 팀 전체 성능 수치도 개인 기여의 직접 Evidence로 사용하지 않습니다.

## 검증된 개인 기여 (Verified Contributions)

| 기능 | 개인 기여 | Commit / PR | 근거 수준 |
|---|---|---|---|
| Azure Custom Vision integration | 외관 이미지 바이트를 Custom Vision endpoint에 전달하고 응답 상태·빈 응답을 확인하는 호출 함수 연결 | [Commit c17ddad](https://github.com/ms-ai-school-10th-team3/battery/commit/c17ddad9dde8a040d7dde6172e2262bacf06580f) | 본인 authored Commit + `services/custom_vision_out.py`, `pages/exterior_inspection.py` 변경 |
| Custom Vision configuration | endpoint/key를 환경 변수 또는 Streamlit secret에서 읽도록 변경 | [Commit 31653f8](https://github.com/ms-ai-school-10th-team3/battery/commit/31653f8cac6b55704f4e54951546fb402b063cf6) | 본인 authored Commit + 설정 조회 코드 변경 |
| 2-stage exterior inference | Swelling 결과와 일반 결함 분류 결과를 threshold 기반으로 분기하는 Custom Vision ensemble 연결 | [Commit 05fb808](https://github.com/ms-ai-school-10th-team3/battery/commit/05fb8083d366d7ec14b968968fb115c86429d6a5) | 본인 authored Commit + 호출 경로 변경 |
| DeepLab model integration | DeepLab MobileNet 구조, 전처리, 모델 로드, `predict_one_image` inference와 mask/overlay 생성 경로 추가 | [Commit af8fcf8](https://github.com/ms-ai-school-10th-team3/battery/commit/af8fcf851e7517a2af1b005fe4a4c0e0df4572c6) | 본인 authored Commit + `ai_models/deeplab_mobilenet/` 코드 추가 |
| CT judgement and storage | CT inference의 `judgement` 결과에서 불량 유형·위험도·권장 조치를 만들고 공통 저장 함수에 연결 | [Commit a558218](https://github.com/ms-ai-school-10th-team3/battery/commit/a55821837a2a8a1c7f78f06ea9ba624ea56903f6) | 본인 authored Commit + `pages/ct_inspection.py` 변경 |
| CT inspection records | CT 검사 결과와 모델 식별자를 보고서 데이터에 연결 | [Commit 85f52ec](https://github.com/ms-ai-school-10th-team3/battery/commit/85f52ecf38969aef9c1c30a47bce6afd6942b0fb) | 본인 authored Commit + CT 기록 경로 변경 |
| Report page and data handling | 검사 리포트 페이지, CSV 로드·날짜/숫자 정규화, 상세 보고서 조회 흐름 구현 | [Commit e64b792](https://github.com/ms-ai-school-10th-team3/battery/commit/e64b7921bf94ae1c0c218a165374ba7f96466558) | 본인 authored Commit + `pages/report.py` 추가 |
| Shared report storage | 보고서 목록·Battery ID 조회·공통 저장 helper를 추가하고 report page와 연결 | [Commit a87459b](https://github.com/ms-ai-school-10th-team3/battery/commit/a87459bb730b7137e3ae76e2c00cb17caf0942c0) · [Commit 649c9b3](https://github.com/ms-ai-school-10th-team3/battery/commit/649c9b36d8f65e97eda1de301f33cdfc4813dbba) | 본인 authored Commit + `utils/report_storage.py` 변경 |
| Azure-compatible report path | 보고서 저장 경로를 helper로 분리하고 Azure App Service `/home` 저장 경계를 고려 | [Commit cdbcdf9](https://github.com/ms-ai-school-10th-team3/battery/commit/cdbcdf9db3244bf08a2bc9e5d3b2c87382648579) | 본인 authored Commit + storage path 변경 |
| Report timestamp and artifact paths | KST timestamp, report directory 생성, 원본 이미지·overlay 경로 저장을 보강 | [Commit 1ef4a9a](https://github.com/ms-ai-school-10th-team3/battery/commit/1ef4a9aac38b9ebf46bf56beb1ad5f118c9d5505) · [Commit ff12528](https://github.com/ms-ai-school-10th-team3/battery/commit/ff12528fcd05579c60261e55a222bbe3e9565e27) | 본인 authored Commit + storage/UI 연결 |
| History filtering | 기간·라인·검사 유형·판정 결과·Battery ID 검색과 결과 정렬을 검사 이력 페이지에 연결 | [Commit 0c7a6b0](https://github.com/ms-ai-school-10th-team3/battery/commit/0c7a6b0075dcf770b9d4add665c455bf03763a45) | 본인 authored Commit + filter code 변경 |
| Report drill-down UI | 이력 행 선택 결과를 session state에 저장하고 상세 report page로 이동 | [Commit 7eeef65](https://github.com/ms-ai-school-10th-team3/battery/commit/7eeef6592b6d5aeb0a520df5fc989bf90db7f362) | 본인 authored Commit + history/detail 연결 |
| Azure App Service workflow | Python setup, dependency install, artifact upload, Azure login, Web App deploy 단계를 GitHub Actions에 정의 | [Commit 80dc8e9](https://github.com/ms-ai-school-10th-team3/battery/commit/80dc8e98146e6fa03c69b4c350f06e59271c0ecf) · [Commit ec75de7](https://github.com/ms-ai-school-10th-team3/battery/commit/ec75de701bb670b82efad2864f5b1633f038164b) | 본인 authored Commit + workflow YAML 변경 |

## 기여 범위 및 상태 (Boundary and Status Notes)

- Team Lead 표기는 최종 발표자료의 팀 소개 슬라이드라는 보조 근거로 추가했습니다. 이는 일정 관리·역할 분배와 프로젝트 책임 범위를 설명하지만, 모든 모델 학습·Frontend·Architecture를 혼자 구현했다는 의미는 아닙니다. 개인 구현 주장은 아래 GitHub authored Commit을 기준으로 제한합니다.
- Repository README와 팀 전체 파일에는 CNN, PyTorch, Azure ML, 모델 평가 지표, 향후 기능 등 전체 프로젝트 범위가 설명되어 있지만, 개인 authored Commit으로 확인되지 않는 모델 학습·성능 비교·수상·전체 기술 리딩은 개인 기여로 집계하지 않았습니다.
- `af8fcf8`에서 DeepLab 구조와 inference code가 추가된 것은 확인되지만, 학습을 제가 주도했다거나 README에 포함된 mIoU 수치를 달성했다는 근거로 사용하지 않았습니다.
- Azure App Service workflow Commit과 최종 발표자료의 시연 기록을 종합해 프로토타입 배포·시연 수준까지 기록했습니다. Production 운영·운영 지속성·가동률은 주장하지 않습니다.

## 검증 요약 (Validation Summary)

| 검증 유형 | 확인된 근거 | 범위 및 한계 |
|---|---|---|
| External inference boundary | Custom Vision 호출에서 비정상 status와 빈 응답을 예외 처리 | 모델 품질이나 서비스 가용성 지표는 확인하지 않음 |
| CT inference output | 모델 weight 로드, `eval` 추론, mask·overlay·JSON 결과 생성 코드 | 정확도·mIoU·F1 수치는 주장하지 않음 |
| Result persistence | 외관·CT 결과를 공통 CSV 저장 helper와 report fields에 연결 | 운영 DB 안정성이나 동시성 검증은 확인하지 않음 |
| Report UI | 날짜·라인·검사 유형·판정·Battery ID 필터와 상세 페이지 이동 코드 | 사용자 규모·latency·사용성 지표는 확인하지 않음 |
| Model selection documentation | Classification baseline 및 Detection/Segmentation 후보 비교와 최종 MVP 모델 선택 기록 | 팀 프로젝트 문서이며 개인 단독 학습·성능 수치의 직접 Evidence로 사용하지 않음 |
| Prototype deployment/demo | GitHub Actions workflow와 발표자료의 Azure-Streamlit end-to-end 연동·시연 기록 | production 운영·실제 사용자 트래픽·SLA·가동률은 확인하지 않음 |

## 공개 범위 원칙 (Publication Policy)

이 문서는 public Repository의 Commit·코드 경로·일반화한 설명만 사용합니다. Secret 값, 실제 endpoint, Azure Resource 식별자, 개인 데이터, 모델 weight 파일 내용은 복사하지 않습니다. 모델 성능과 팀 전체 결과는 별도 검증 자료 없이는 개인 성과로 표현하지 않습니다.
