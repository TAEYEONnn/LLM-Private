# AI Workspace v3

Windows AI 서버와 MacBook Air 클라이언트를 결합한 개인용 로컬·클라우드 하이브리드 LLM 워크스페이스 설계 패키지입니다.

## 핵심 원칙

- 특정 제품이 아니라 OpenAI-compatible API 규격과 운영 정책을 통일합니다.
- Ollama와 llama.cpp를 동등한 런타임 후보로 비교합니다.
- 모델 단독이 아니라 모델 × 양자화 × 템플릿 × 런타임 × 하네스 × 도구 세트 조합을 승인합니다.
- TPS보다 종단 작업 완료율, 테스트 통과, 사람 개입, 안전성을 우선합니다.
- 파일 쓰기 에이전트는 Git worktree 안에서 먼저 검증합니다.
- 자동 Cloud 전환, 자동 결제, 자동 유료 폴백은 금지합니다.
- PUBLIC / PRIVATE / CONFIDENTIAL 데이터 등급을 분리합니다.

## 문서 읽는 순서

1. `docs/00-shared-architecture-v3.md`
2. `docs/10-windows-build-prompt-v3.md`
3. `docs/20-macos-build-prompt-v3.md`
4. `docs/30-agent-stack-benchmark-v3.md`
5. `docs/40-model-provider-registry.md`
6. `docs/50-security-and-data-policy.md`
7. `docs/70-operations-runbook.md`
8. `docs/80-implementation-roadmap.md`

작성 기준일: 2026-06-27  
문서 버전: v3.0  
구축 상태: 설계 완료 / 설치 전
