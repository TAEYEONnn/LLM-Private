# 02. Scope and Non-Goals

작성 기준일: 2026-06-28

## 범위

초기 범위:

- OpenKnowledge PUBLIC Pilot
- ChatGPT 읽기 연결 또는 ChatGPT Project 대안
- Codex·Claude Code·OpenCode 작업 완료 Hook
- 구조화된 Work Capture Event
- Draft·Reviewed·Approved 문서 Workflow
- Git 기반 Wiki와 Rollback
- 최소 2주 또는 작업 30건 사용량 측정
- Local LLM 필요성 판단

조건부 후속 범위:

- Windows Local LLM API
- Ollama·llama.cpp 비교
- Mac OpenCode Local Profile
- Local Embedding
- Open WebUI RAG
- ComfyUI
- 외부 Skill

## 초기 비범위

- Windows Local LLM 선설치
- 다중 사용자 서비스
- 공개 API
- 인터넷 Port Forwarding
- 자동 Provider Routing
- 자동 유료 승격
- 자동 Git Push
- Approved 문서 자동 덮어쓰기
- Full Access Agent
- 파인튜닝
- 완전 자율 장기 Agent

## 최소 성공 경로

```text
ChatGPT / Codex / Claude Code / OpenCode
→ 실제 작업
→ Build / Test / Git diff
→ 작업 완료 이벤트
→ OpenKnowledge 기록
→ 다음 작업에서 재사용
```

## Local 확장 성공 경로

`LOCAL-NEED` Gate가 통과된 경우에만 다음을 추가한다.

```text
Windows Local Executor
→ 기존 Work Capture Hook
→ OpenKnowledge 기록
```
