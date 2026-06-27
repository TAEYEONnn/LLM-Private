# AI Workspace v3

Windows AI 서버와 MacBook Air 클라이언트를 결합한 개인용 로컬·클라우드 하이브리드 LLM 워크스페이스 설계 패키지입니다.

## 핵심 원칙
- 특정 제품이 아니라 OpenAI-compatible API 규격과 운영 정책을 통일합니다.
- Ollama와 llama.cpp를 동등한 런타임 후보로 비교합니다.
- 모델 단독이 아니라 모델 × 양자화 × 템플릿 × 런타임 × 하네스 × 도구 세트 조합을 승인합니다.
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
6. 벤치마크 자동화, Open WebUI RAG, 로컬 Embedding, ComfyUI 순으로 확장

OpenKnowledge는 로컬 LLM 연결 전에는 설치하지 않습니다. 연결 검증 후 `docs/82-openknowledge-integration-plan.md`를 따라 별도 Pilot으로 진행합니다.

## 주요 문서

- `CLAUDE.md` — Claude Code 실행 오케스트레이터
- `docs/80-implementation-roadmap.md` — 전체 단계와 Gate
- `docs/81-completion-checklist.md` — 완료 체크리스트
- `docs/82-openknowledge-integration-plan.md` — LLM 연결 이후 OpenKnowledge 도입 계획
- `docs/93-skill-evaluation-policy.md` — 외부 Skill·Plugin·MCP 검토 정책
- `config/openknowledge-pilot.example.yml` — OpenKnowledge Pilot Manifest
- `config/skill-registry.example.yml` — 외부 도구 후보 레지스트리

작성 기준일: 2026-06-27
문서 버전: v3.1
구축 상태: 설계 완료 / 설치 전
