# AI Workspace v3

Windows AI 서버와 MacBook Air 클라이언트를 결합한 개인용 로컬·클라우드 하이브리드 LLM 워크스페이스 설계 패키지입니다.

## 이 프로젝트가 만드는 것

새 Foundation Model을 처음부터 학습하는 프로젝트가 아닙니다.

기존 Open-weight Model, Runtime, Tool Harness, Wiki, 보안 정책을 사용자의 장비와 업무에 맞게 조합하고 검증해 개인용 Local LLM System을 만듭니다.

```text
Open-weight Model
+ Quantization
+ Ollama / llama.cpp
+ OpenCode
+ Permission Policy
+ OpenKnowledge
+ Model Sequencing
+ Benchmark
= 사용자 전용 AI 작업 환경
```

## 핵심 원칙

- 특정 제품이 아니라 OpenAI-compatible API 규격과 운영 정책을 통일합니다.
- Ollama와 llama.cpp를 동등한 런타임 후보로 비교합니다.
- 모델 단독이 아니라 모델 × 양자화 × 템플릿 × 런타임 × 하네스 × 도구 세트 조합을 승인합니다.
- 기본 Cloud Planner는 Claude Sonnet 4.6입니다.
- Claude Opus 4.8은 자동 승격하지 않고 고위험 예외에만 사용자 승인 후 사용합니다.
- PRIVATE·CONFIDENTIAL 실행은 Windows Local Fast를 기본으로 합니다.
- TPS보다 종단 작업 완료율, 테스트 통과, 사람 개입, 안전성을 우선합니다.
- 파일 쓰기는 Git worktree 안에서 먼저 검증합니다.
- 자동 Cloud 전환, 자동 결제, 자동 유료 폴백은 금지합니다.
- PUBLIC / PRIVATE / CONFIDENTIAL 데이터 등급을 분리합니다.

## 구축 순서

1. Windows Ollama·llama.cpp와 Fast 모델 구축
2. Mac OpenCode에서 Windows Local LLM 연결
3. Worktree 수정·테스트·Diff로 `CORE-LINK` 검증
4. 별도 PUBLIC 저장소에서 OpenKnowledge Pilot
5. 승인 후 Private `AI-Knowledge` 저장소 승격
6. `LLM-Private`의 승인 문서를 AI-Knowledge로 선별 Bootstrap
7. Sonnet Planner → Local Executor → Test·Diff → Wiki 기록 흐름 검증
8. Open WebUI RAG, 로컬 Embedding, ComfyUI 순으로 확장

OpenKnowledge는 로컬 LLM 연결 전에는 설치하지 않습니다. 연결 검증 후 `docs/82-openknowledge-integration-plan.md`를 따르고, Pilot 승인 후 `docs/83-openknowledge-bootstrap-plan.md`로 기존 Git 문서를 선별 이관합니다.

## 주요 문서

- `CLAUDE.md` — Claude Code 실행 오케스트레이터
- `docs/43-local-llm-stack-composition.md` — 직접 만드는 영역과 가져오는 영역
- `docs/44-model-sequencing-and-cost-governance.md` — Sonnet 기본 Planner와 비용 정책
- `docs/80-implementation-roadmap.md` — 전체 단계와 Gate
- `docs/81-completion-checklist.md` — 완료 체크리스트
- `docs/82-openknowledge-integration-plan.md` — LLM 연결 이후 OpenKnowledge 도입 계획
- `docs/83-openknowledge-bootstrap-plan.md` — 기존 Git 문서의 Wiki 선별 이관
- `docs/93-skill-evaluation-policy.md` — 외부 Skill·Plugin·MCP 검토 정책
- `config/orchestration-policy.example.yml` — 모델 역할·비용·승격 정책
- `config/openknowledge-pilot.example.yml` — OpenKnowledge Pilot Manifest
- `config/skill-registry.example.yml` — 외부 도구 후보 레지스트리

작성 기준일: 2026-06-27  
문서 버전: v3.2  
구축 상태: 설계 완료 / 설치 전
