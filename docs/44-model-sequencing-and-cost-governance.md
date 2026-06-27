# 44. Model Sequencing and Cost Governance

작성 기준일: 2026-06-27  
상태: Approved Policy v1.0

## 1. 기본 원칙

모든 작업을 하나의 모델에 맡기지 않는다.

모델은 역할에 따라 명시적으로 선택하며 자동 승격·자동 유료 Fallback은 금지한다.

```text
Plan / Vision / Review
→ Claude Sonnet 4.6

Public 대량 실행 후보
→ Budget Cloud Executor

Private·Confidential 반복 실행
→ Windows Local Fast

고위험·고복잡도 예외
→ Claude Opus 4.8 수동 승격
```

## 2. 기본 Planner

기본 Planner는 `claude-sonnet-4-6`으로 고정한다.

역할:

- 요구사항 구조화
- 스크린샷·화면 분석
- 구현 계획
- 영향 파일 추정
- 디자인 시스템 검토
- 실행 계약 작성
- 테스트·Diff 최종 검토
- OpenKnowledge용 승인 문서 초안

기본 설정:

```yaml
model: claude-sonnet-4-6
effort: medium
automatic_escalation: false
max_planner_retries: 2
```

## 3. Opus 승격

`claude-opus-4-8`은 기본값이 아니다.

다음 조건 중 하나를 만족할 때만 사용자 승인 후 사용한다.

- Sonnet이 동일 문제를 두 번 실패
- 데이터 손실 또는 보안 위험이 큰 설계
- 여러 저장소를 걸친 대형 Migration
- 요구사항 충돌이 심한 Architecture 결정
- 출시 전 고위험 감사
- Sonnet 결과가 Benchmark 기준을 통과하지 못했으며 Local·Budget Executor로 해결 불가

Opus 사용 시에도 `effort`를 명시하고, 기본 `high`를 무조건 사용하지 않는다.

## 4. Local Executor

Windows Local Fast는 다음 작업의 기본 Executor다.

- PRIVATE·CONFIDENTIAL 코드와 문서
- 반복적인 컴포넌트 수정
- 코드 설명
- 단일 파일 버그 수정
- 테스트 실패 분석
- UX Writing
- 문서 정리
- Git diff 요약
- OpenKnowledge 문서 읽기·제한적 쓰기

초기 모델:

```text
Qwen2.5-Coder-7B-Instruct Q4_K_M
Context 8192
Thinking Off
동시 요청 1
```

## 5. Budget Cloud Executor

Budget Cloud Executor는 PUBLIC 또는 별도 승인된 PRIVATE 작업에만 사용한다.

후보 예시:

- GLM 계열 API
- OpenRouter를 통한 승인 모델
- OpenCode Zen PAYG 승인 모델

용도:

- 다중 파일 구현
- 출력량이 큰 코드 생성
- 장시간 반복 작업
- Local Fast가 성능 기준을 통과하지 못한 PUBLIC 작업

후보는 비용이 저렴하다는 이유만으로 승인하지 않는다. 동일 Fixture에서 완료 작업당 비용과 사람 개입을 비교한다.

## 6. Vision Workaround

Local Executor의 Vision 성능이 부족할 경우:

1. Sonnet이 스크린샷을 분석한다.
2. 분석 결과를 구조화된 Execution Contract로 만든다.
3. 이미지 자체가 아니라 승인된 텍스트 명세를 Local 또는 Budget Executor에 전달한다.
4. 실제 구현 결과는 Screenshot, Test, Diff로 검증한다.

원본 화면에 PRIVATE·CONFIDENTIAL 정보가 있으면 Cloud Vision을 사용하지 않는다.

## 7. Execution Contract

Planner와 Executor 사이에는 채팅 전체가 아니라 다음 구조를 전달한다.

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
planner_model:
executor_profile:
budget_cap_usd:
wiki_sources:
```

## 8. 비용 기준

공식 Claude API 기준으로 2026-06-27 현재:

- Claude Sonnet 4.6: 입력 $3 / 출력 $15 per MTok
- Claude Opus 4.8: 입력 $5 / 출력 $25 per MTok

Sonnet은 Opus 대비 입력·출력 단가가 40% 낮다.

단가만으로 모델을 선택하지 않는다.

```text
완료 작업당 비용
=
API 비용
+ 재시도 비용
+ 사람 개입 시간
+ 실패 복구 시간
```

비용 기록 항목:

- 요청·응답 Token
- Cache Hit
- API 비용
- 재시도 수
- 작업 완료 여부
- 테스트 통과 여부
- 사람 개입 횟수
- 전체 소요 시간

공식 참고:

- https://platform.claude.com/docs/en/about-claude/models/overview

## 9. 예산과 Fallback

- 초기 Cloud PAYG 월 한도: $10
- 자동 충전: Off
- 자동 Local→Cloud: Off
- 자동 Sonnet→Opus: Off
- 자동 Budget Cloud→Opus: Off
- 단일 작업 예산 초과 시 중단하고 승인 요청

권장 초기 작업 한도:

```yaml
sonnet_plan_usd: 0.50
sonnet_review_usd: 0.25
budget_cloud_execute_usd: 1.00
opus_escalation_usd: user_approval_required
```

## 10. 데이터 등급별 허용

### PUBLIC

- Sonnet
- 승인된 Budget Cloud
- Local
- 승인 후 Opus

### PRIVATE

- Local 기본
- Sonnet은 필요한 최소 Context만 명시적 승인 후
- Budget Cloud는 별도 Provider 승인 필요

### CONFIDENTIAL

- Local only
- Cloud Planner·Vision·Executor 사용 금지

## 11. 모델 선택 로그

모든 작업 시작 전에 다음을 출력한다.

```text
Data Class:
Planner:
Executor:
Reviewer:
External Transfer:
Budget Cap:
Automatic Fallback: Disabled
Wiki Sources:
```

작업 완료 후 실제 사용 모델과 비용을 OpenKnowledge의 작업 결과 문서에 기록한다.
