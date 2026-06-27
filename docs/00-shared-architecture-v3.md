# 00. Shared Architecture v4

작성 기준일: 2026-06-28

## 목적

가장 큰 로컬 모델을 실행하는 것이 아니라, 실제 작업 결과와 프로젝트 지식을 안전하게 축적하고 재사용하는 AI Workflow를 구축한다.

로컬 모델은 초기 핵심이 아니라 필요성이 확인된 뒤 추가하는 선택적 Executor다.

## 1차 구조

```text
ChatGPT
├─ 기획·분석·이미지 이해
├─ OpenKnowledge 읽기·검색
└─ 작업 결과 정리

Codex / Claude Code / OpenCode
├─ 저장소 파일 수정
├─ Build / Test
├─ Git diff
└─ 작업 완료 이벤트 생성

OpenKnowledge
├─ 프로젝트 Wiki
├─ 승인된 결정
├─ 작업 이력
├─ Runbook·Incident
└─ Git 기반 Markdown
```

## 2차 선택 구조

`LOCAL-NEED` Gate 통과 후 다음을 추가할 수 있다.

```text
Windows AI Server
├─ Ollama Candidate
├─ llama.cpp Candidate
├─ Local Fast Model
├─ Local Embedding Candidate
└─ Storage

MacBook Air
├─ OpenCode Local Profile
└─ OpenKnowledge·Work Capture 재사용
```

## 통일 대상

- MCP 도구 연결
- Work Capture Event Schema
- 데이터 등급
- Git·Test·Diff 검증
- 자동 Fallback 금지
- 모델·공급자 사용 로그
- Benchmark와 승인 정책

## 데이터 등급

- PUBLIC: 승인된 외부 제품과 API 사용 가능
- PRIVATE: 전달 범위를 확인하고 최소 Context만 외부 사용
- CONFIDENTIAL: 승인된 로컬 환경에서만 처리

## 자동화 금지

- 자동 Local→Cloud 또는 Cloud→Local 전환
- 무료→유료 자동 승격
- 자동 Git Push
- Approved 문서 자동 덮어쓰기
- 실패 후 무한 재시도

## 완료 정의

1차 완료:

- OpenKnowledge PUBLIC Pilot
- ChatGPT 읽기 연결 또는 Project 대안
- Codex·Claude Code·OpenCode 작업 완료 Hook
- 승인된 결과의 Wiki 기록
- 다음 작업에서 기존 지식 재사용

2차 완료:

- 최소 2주 또는 작업 30건 측정
- Local LLM 필요성 승인 또는 보류 결정

Local이 승인된 경우에만 Windows Local Model과 Mac 연결을 추가 검증한다.
