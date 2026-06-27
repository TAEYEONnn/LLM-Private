# 43. Local LLM Stack Composition

작성 기준일: 2026-06-27  
상태: Approved Architecture v1.0

## 1. 결론

이 프로젝트는 새로운 Foundation Model을 처음부터 학습하는 프로젝트가 아니다.

기존 오픈 가중치 모델과 오픈소스 런타임을 가져와 사용자의 장비·업무·보안 기준에 맞게 조합하고 검증하는 프로젝트다.

```text
기존 Open-weight Model
+ Quantization
+ Runtime
+ Chat Template
+ Context
+ Tool Harness
+ Permission Policy
+ RAG / Wiki
+ Model Routing
+ Benchmark
= 사용자 전용 Local LLM System
```

따라서 “내가 원하는 로컬 LLM을 만든다”는 표현은 다음 의미로 사용한다.

- 원하는 모델을 선택한다.
- 장비에 맞는 Quantization을 선택한다.
- Ollama 또는 llama.cpp 런타임을 선택한다.
- Context, Thinking, Temperature, 동시 요청 수를 정한다.
- OpenCode 등 Tool Harness를 연결한다.
- 읽기·쓰기·Shell 권한을 제한한다.
- OpenKnowledge, RAG, 프로젝트 규칙을 연결한다.
- Cloud Planner와 Local Executor의 역할을 나눈다.
- 실제 업무 Fixture로 승인 여부를 결정한다.

## 2. 직접 만드는 영역과 가져오는 영역

### 가져오는 영역

- Qwen, Gemma 등 Base 또는 Instruct Model Weight
- Ollama, llama.cpp 같은 Runtime
- OpenCode, Claude Code 같은 Agent Harness
- OpenKnowledge 같은 Wiki·MCP 계층
- 공개 Tool·Skill·Plugin

### 직접 설계하는 영역

- 어떤 업무를 어떤 모델에 배정할지
- 모델·양자화·런타임 조합
- Prompt와 실행 계약
- Tool Calling 형식과 Parser
- 프로젝트별 Context 구성
- PUBLIC / PRIVATE / CONFIDENTIAL 정책
- 자동 Fallback 금지와 승인 절차
- Worktree, Test, Diff 검증 흐름
- Wiki 문서 구조와 승인 상태
- 비용 한도와 승격 조건
- Benchmark와 운영 Runbook

이 직접 설계 영역이 실제 제품성과 안전성을 결정한다.

## 3. 현재 하드웨어에서 가능한 범위

### 가능한 범위

- 4B~9B Q4/Q5 모델의 로컬 추론
- 코드 설명·단일 파일 수정·테스트 복구
- 제한된 Tool Calling
- 로컬 문서 검색과 Wiki 연동
- 로컬 Embedding
- Prompt·Template·Context 최적화
- 필요 시 LoRA Adapter 시험

### 현재 범위를 벗어나는 항목

- 대형 Foundation Model의 처음부터 학습
- 수백 B급 모델의 실용적 로컬 실행
- 대규모 Full Fine-tuning
- 여러 대형 모델의 동시 GPU 상주
- 무제한 Context와 다중 동시 Agent

## 4. Fine-tuning에 대한 입장

Fine-tuning은 Phase 1 목표가 아니다.

우선순위는 다음과 같다.

1. 모델 선택
2. Prompt와 Execution Contract
3. Wiki·RAG Context 개선
4. Tool Schema와 Permission 개선
5. Benchmark 실패 유형 분석
6. 위 방법으로 해결되지 않는 반복 패턴에만 LoRA 검토

RTX 3060 Ti 8GB 환경에서는 소형 모델의 제한된 LoRA가 가능할 수 있지만, 시간·데이터 품질·회귀 위험을 고려해 별도 Experimental 단계로 분리한다.

## 5. 승인 단위

모델 이름 하나를 승인하지 않는다.

다음 전체 Stack ID를 승인한다.

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
× wiki/rag state
```

구성 요소 하나가 바뀌면 새 Stack으로 재검증한다.

## 6. 사용자 관점의 최종 결과

사용자가 얻게 되는 것은 새로 학습한 독자 모델이 아니라 다음과 같은 개인 AI 작업 환경이다.

```text
Sonnet Planner
→ 승인된 실행 명세
→ Local Qwen Executor
→ Build / Test / Diff
→ OpenKnowledge 기록
```

필요한 경우에만 Budget Cloud Executor 또는 Opus Escalation을 수동으로 선택한다.

이 구조는 모델을 소유하는 것보다 작업 흐름·데이터·비용·검증을 통제하는 데 목적이 있다.
