# 44. Model Sequencing and Cost Governance

작성 기준일: 2026-06-28  
상태: Approved Policy v2.0

## 1. 기본 원칙

초기에는 ChatGPT·Codex·Claude Code·OpenCode를 그대로 사용하고, OpenKnowledge에 작업 결과를 축적한다.

Windows Local LLM은 기본 전제가 아니다. 최소 2주 또는 작업 30건 측정 후 `LOCAL-NEED` Gate를 통과한 경우에만 추가한다.

```text
기획·분석·이미지 이해
→ ChatGPT 또는 승인된 Cloud 모델

저장소 수정·테스트
→ Codex / Claude Code / OpenCode

작업 결과 축적
→ OpenKnowledge

반복·민감·Batch 작업
→ LOCAL-NEED 승인 후 Windows Local Executor
```

자동 유료 승격과 자동 Local↔Cloud Fallback은 금지한다.

## 2. 초기 기본 Workflow

```text
요청
→ ChatGPT·Codex·Claude Code 중 적합한 제품 선택
→ 작업 수행
→ Build / Lint / Typecheck / Test / Git diff
→ 구조화된 완료 이벤트 생성
→ OpenKnowledge 기록 승인
```

초기 단계에서는 제품 안에서 제공하는 기본 모델 선택을 사용할 수 있다. 특정 Cloud 모델을 시스템 전체의 필수 Planner로 고정하지 않는다.

작업 시작 시 다음을 기록한다.

```text
Data Class:
Actor Product:
Selected Model or Mode:
External Transfer:
Budget or Usage Limit:
Automatic Fallback: Disabled
Wiki Sources:
```

## 3. Sonnet 사용

Claude Sonnet은 다음 작업의 선택적 Cloud 모델이다.

- 복잡한 요구사항 구조화
- 화면·스크린샷 분석
- 구현 계획
- 디자인 시스템 검토
- 테스트·Diff 리뷰
- OpenKnowledge 문서 구조화

Sonnet은 로컬 시스템의 일부가 아니며 Anthropic Cloud에서 실행된다.

Sonnet을 사용하지 않아도 Phase 1~6의 Wiki First Workflow는 구축할 수 있다.

## 4. Opus 승격

Opus는 기본값이 아니다.

다음과 같은 예외 상황에서만 사용자 승인 후 사용한다.

- 일반 모델이 같은 문제를 반복 실패
- 데이터 손실 또는 보안 위험이 큰 설계
- 여러 저장소에 걸친 대형 Migration
- 요구사항 충돌이 심한 Architecture 결정
- 출시 전 고위험 감사

자동 Sonnet→Opus 승격을 금지한다.

## 5. Codex·Claude Code·OpenCode

이 도구들은 저장소 작업 Actor다.

- 파일 읽기·수정
- 명령 실행
- 테스트
- Git diff
- 작업 완료 이벤트 생성

도구와 모델을 구분한다.

```text
Codex·Claude Code·OpenCode
= 작업을 수행하는 Harness 또는 Product

GPT·Claude·Qwen 등
= 그 안에서 사용되는 Model
```

실제 성공 여부는 모델의 자기평가가 아니라 Git과 테스트로 판정한다.

## 6. Windows Local Executor

Windows Local Fast는 `LOCAL-NEED` Gate 통과 전에는 `deferred`다.

승인 후 후보:

```text
Qwen2.5-Coder-7B-Instruct Q4_K_M
Context 8192
Thinking Off
동시 요청 1
```

적합 작업:

- 정형 반복 수정
- 대량 문서 변환
- 코드 설명
- 단일 파일 오류 수정
- 테스트 실패 요약
- Git diff 요약
- 외부 전송이 불가능한 자료
- 장시간 Batch 작업

Local Executor가 추가돼도 OpenKnowledge Hook과 문서 구조는 변경하지 않는다.

## 7. Budget Cloud Executor

Budget Cloud Executor도 초기 필수가 아니다.

다음 경우에만 후보를 검토한다.

- 현재 제품 사용량 제한이 반복적으로 발생
- 출력량이 큰 PUBLIC 작업이 많음
- 동일 Fixture에서 사람 개입과 완료 작업당 비용이 개선됨

후보는 가격만으로 승인하지 않는다.

## 8. Vision Workaround

로컬 모델에 Vision 기능이 부족할 경우 Cloud Vision을 선택적으로 사용할 수 있다.

1. Cloud 모델이 이미지나 화면을 분석한다.
2. 구조화된 텍스트 Execution Contract를 만든다.
3. Executor는 승인된 텍스트 명세를 사용한다.
4. 결과는 Screenshot·Test·Diff로 검증한다.

민감한 화면은 Cloud Vision으로 보내지 않는다.

## 9. Execution Contract

Planner와 Executor 사이에는 전체 채팅 대신 다음 구조를 전달한다.

```yaml
task_id:
data_class:
goal:
constraints:
target_files:
reference_assets:
implementation_steps:
acceptance_tests:
prohibited_actions:
actor_product:
selected_model_or_mode:
executor_profile:
budget_or_usage_limit:
wiki_sources:
```

## 10. 비용과 사용량 기준

가격표는 변경될 수 있으므로 실행 당일 공식 문서를 확인한다.

단가보다 완료 작업당 비용을 우선한다.

```text
완료 작업당 비용
=
직접 비용
+ 재시도
+ 사람 개입
+ 실패 복구
+ 유지보수 시간
```

기록 항목:

- 사용 제품과 모델
- 요청·응답 Token 또는 Usage
- 직접 비용
- 재시도 수
- 작업 완료 여부
- 테스트 통과 여부
- 사람 개입 횟수
- 전체 소요 시간
- Wiki 재사용 여부

## 11. 예산과 Fallback

- 자동 충전 Off
- 자동 유료 승격 Off
- 자동 Local→Cloud Off
- 자동 Cloud→Local Off
- 단일 작업 예산 또는 사용량 한도 초과 시 중단
- 유료 API 등록은 별도 승인

## 12. 데이터 등급

### PUBLIC

- 승인된 Cloud 제품 사용 가능
- OpenKnowledge Pilot 가능

### PRIVATE

- 최소 Context만 Cloud에 전달
- 사용 전 데이터 범위 확인
- OpenKnowledge는 Private 저장소로 운영

### CONFIDENTIAL

- 승인된 로컬 환경에서만 처리
- Local Executor 구축 전에는 처리하지 않거나 별도 회사 승인 절차를 따른다

## 13. 측정 후 결정

최소 2주 또는 작업 30건 측정 후 다음을 판단한다.

- ChatGPT·Codex만으로 충분한가
- OpenKnowledge 재사용 가치가 있는가
- 반복 작업이 Local 구축을 정당화하는가
- 외부 전송 불가 작업이 실제로 많은가
- 운영 시간까지 포함해 Local이 더 저렴한가

필요가 입증되지 않으면 Local LLM과 추가 Cloud Provider를 설치하지 않는다.
