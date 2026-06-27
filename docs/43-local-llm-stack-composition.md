# 43. Local LLM Stack Composition

작성 기준일: 2026-06-28  
상태: Deferred Architecture v2.0

## 1. 현재 위치

이 문서는 Windows Local LLM이 필요하다고 승인된 뒤 사용하는 후속 설계다.

OpenKnowledge·ChatGPT 읽기 연결·작업 완료 Hook·사용량 측정이 먼저다.

```text
Wiki First Workflow
→ 최소 2주 또는 작업 30건 측정
→ LOCAL-NEED Gate
→ 필요성이 확인된 경우에만 Local LLM Stack 구축
```

## 2. 결론

이 프로젝트는 새로운 Foundation Model을 처음부터 학습하는 프로젝트가 아니다.

기존 오픈 가중치 모델과 오픈소스 런타임을 사용자의 장비·업무·보안 기준에 맞게 조합하고 검증한다.

```text
Open-weight Model
+ Quantization
+ Runtime
+ Chat Template
+ Context
+ Tool Harness
+ Permission Policy
+ OpenKnowledge Hook
+ Benchmark
= 사용자 전용 Local Executor
```

Local Executor는 전체 시스템이 아니라 교체 가능한 실행 계층이다.

## 3. 가져오는 영역

- Qwen·Gemma 등 Base 또는 Instruct Model Weight
- Ollama·llama.cpp 같은 Runtime
- OpenCode·Claude Code 등 Agent Harness
- OpenKnowledge Wiki·MCP 계층
- 공개 Tool·Skill·Plugin

## 4. 직접 설계하는 영역

- 어떤 반복 작업을 Local에 배정할지
- 모델·양자화·런타임 조합
- Prompt와 Execution Contract
- Tool Calling 형식
- Context 길이와 동시 요청 수
- 데이터 등급과 외부 전송 정책
- Worktree·Test·Diff 검증 흐름
- Work Capture Hook
- 비용·운영시간 비교
- Benchmark와 Rollback

## 5. 현재 하드웨어에서 가능한 범위

### 현실적인 범위

- 4B~9B Q4/Q5 모델 추론
- 코드 설명
- 단일 파일 수정
- 반복적인 문서 변환
- 테스트 실패 요약
- 제한된 Tool Calling
- 로컬 문서 검색
- 로컬 Embedding 후보

### 현재 범위를 벗어나는 항목

- 대형 Foundation Model 처음부터 학습
- 수백 B급 모델의 실용적 로컬 실행
- 대규모 Full Fine-tuning
- 여러 대형 모델의 동시 GPU 상주
- 무제한 Context와 다중 동시 Agent

## 6. 초기 후보

LOCAL-NEED 승인 후 다음부터 시작한다.

```text
Qwen2.5-Coder-7B-Instruct Q4_K_M
Context 8192
Thinking Off
동시 요청 1
```

런타임 후보:

- Ollama
- llama.cpp

동일 Model·Quantization·Context·Template·Fixture로 비교한다.

## 7. Fine-tuning

Fine-tuning은 초기 Local Pilot 목표가 아니다.

우선순위:

1. Prompt와 Execution Contract
2. Wiki Context 품질
3. Tool Schema와 Permission
4. Model·Quantization·Runtime 선택
5. 실패 유형 분석
6. 반복 실패가 명확할 때만 LoRA 검토

## 8. 승인 단위

모델 이름 하나를 승인하지 않는다.

```text
model
× model revision
× quantization
× runtime version
× chat template
× context length
× thinking mode
× harness version
× tool schema
× permission profile
× wiki state
× work-capture version
```

구성 요소가 바뀌면 새 Stack으로 재검증한다.

## 9. 성공 기준

- 기존 Cloud Workflow보다 반복 작업 비용 또는 사용량 불편 감소
- 테스트 통과율이 승인 기준 충족
- 재작업과 사람 개입이 허용 범위 이내
- OpenKnowledge Hook을 그대로 재사용
- 외부 전송이 불가능한 작업을 안전하게 처리
- 설치·운영 시간을 포함해 실질적 이득이 있음

## 10. 사용자 관점의 결과

필요가 확인된 경우 최종 흐름은 다음과 같다.

```text
ChatGPT 또는 선택한 Planner
→ 승인된 실행 명세
→ Windows Local Executor
→ Build / Test / Diff
→ 기존 Work Capture Hook
→ OpenKnowledge 기록
```

필요가 확인되지 않으면 Local Executor를 설치하지 않고 ChatGPT·Codex·OpenKnowledge 구성을 유지한다.
