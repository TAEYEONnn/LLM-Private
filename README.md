# AI Workspace v4

ChatGPT·Codex·Claude Code와 OpenKnowledge를 연결해 작업 결과를 축적하고, 필요성이 실제로 확인된 뒤 Windows Local LLM을 선택적으로 추가하는 개인 AI 워크스페이스 설계 패키지입니다.

## 이 프로젝트가 먼저 만드는 것

처음부터 로컬 모델 서버를 구축하지 않습니다.

먼저 다음 흐름을 검증합니다.

```text
ChatGPT / Codex / Claude Code
→ 실제 작업 수행
→ Build / Test / Git diff
→ 구조화된 작업 완료 이벤트
→ OpenKnowledge 기록
→ 다음 작업에서 검색·재사용
```

핵심 가치는 다음 두 가지입니다.

- 반복해서 설명하던 프로젝트 맥락을 Wiki에 축적
- 코드·문서 작업 결과를 Hook으로 자동 정리

Windows Local LLM은 이후 반복량·비용·보안 필요성이 실제 데이터로 확인됐을 때 붙이는 선택적 Executor입니다.

## 핵심 원칙

- OpenKnowledge를 프로젝트 지식의 Source of Truth로 검증합니다.
- ChatGPT는 기획·분석·이미지 이해·문서 작업 인터페이스로 사용합니다.
- Codex·Claude Code·OpenCode는 저장소 수정과 테스트를 수행할 수 있습니다.
- 전체 대화가 아니라 승인된 결과·결정·실패 원인만 Wiki에 기록합니다.
- `approved` 문서는 Agent가 직접 덮어쓰지 않습니다.
- 자동 Git Push, 자동 Cloud Fallback, 자동 유료 승격을 금지합니다.
- PUBLIC / PRIVATE / CONFIDENTIAL 데이터 등급을 분리합니다.
- 로컬 LLM은 필요성 측정 전 설치하지 않습니다.

## 구축 순서

1. 현재 작업 방식과 반복 비용 기준선 기록
2. 별도 PUBLIC 저장소에서 OpenKnowledge Pilot
3. ChatGPT 읽기 연결 또는 ChatGPT Project 대안 검증
4. Codex·Claude Code·OpenCode 작업 완료 Hook
5. 제한된 Wiki 쓰기와 승인 Workflow
6. 최소 2주 또는 작업 30건 사용량 측정
7. `LOCAL-NEED` Gate 판단
8. 필요가 확인되면 Windows Ollama·llama.cpp·Qwen Pilot
9. Local Executor를 기존 Hook과 OpenKnowledge에 연결
10. 필요에 따라 RAG·Embedding·ComfyUI·외부 Skill 확장

## 로컬 LLM 판단

다음 문제가 실제로 반복될 때만 구축합니다.

- Cloud 사용 제한이나 비용이 지속적으로 불편함
- 외부 전송이 불가능한 자료 작업이 많음
- 장시간 Batch·대량 변환이 필요함
- 4B~9B 모델로 처리할 수 있는 정형 반복 작업이 명확함

그렇지 않으면 ChatGPT·Codex·OpenKnowledge 구성만 유지합니다.

## 주요 문서

- `CLAUDE.md` — 실행 오케스트레이터
- `docs/80-implementation-roadmap.md` — Wiki First 전체 단계와 Gate
- `docs/81-completion-checklist.md` — 단계별 완료 체크리스트
- `docs/82-openknowledge-integration-plan.md` — OpenKnowledge PUBLIC Pilot
- `docs/83-openknowledge-bootstrap-plan.md` — 기존 Git 문서의 Wiki 선별 이관
- `docs/84-chatgpt-openknowledge-first-plan.md` — ChatGPT·Hook·측정·LOCAL-NEED 계획
- `docs/43-local-llm-stack-composition.md` — Local LLM이 승인된 뒤의 구성 원칙
- `docs/44-model-sequencing-and-cost-governance.md` — 선택적 모델 역할과 비용 정책
- `config/work-capture-schema.example.yml` — 작업 완료 이벤트 Schema
- `config/openknowledge-pilot.example.yml` — OpenKnowledge Pilot Manifest
- `config/orchestration-policy.example.yml` — 모델 역할·비용·승격 정책

작성 기준일: 2026-06-28  
문서 버전: v4.0  
구축 상태: Wiki First 설계 완료 / 설치 전
