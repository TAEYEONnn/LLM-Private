# 00. Shared Architecture v3

## 목적
가장 큰 로컬 모델을 실행하는 것이 아니라, 실제 작업에서 안전하고 재현 가능한 AI 스택을 구축한다.

## 통일 대상
- OpenAI-compatible Chat Completions / Responses
- 필요 시 Anthropic Messages
- MCP 도구 연결
- 공통 모델·공급자 레지스트리
- 공통 벤치마크와 승인 정책

## 구조
```text
MacBook Air
├─ OpenCode
│  ├─ Windows Local Fast
│  ├─ Windows Local Medium
│  ├─ Windows Local Deep Experimental
│  ├─ Cloud PAYG
│  └─ Mac Local Fallback
├─ Native Codex
├─ Claude Code Optional
└─ Browser → Windows Open WebUI

Windows AI Server
├─ Ollama Candidate
├─ llama.cpp Candidate
├─ LM Studio Optional Lab
├─ Open WebUI
├─ ComfyUI
├─ Context Layer
└─ Storage
```

## 모델 계층
- Local Fast: 3B~9B, 8K, 반복·정형 작업
- Local Medium: 10B~14B, Fast 실패 과제 보완
- Local Deep Experimental: 20B~27B 우선, 4K/8K, 단발성 분석
- Cloud Frontier: 복잡한 다중 파일 구현과 장기 에이전트

## 데이터 등급
- PUBLIC: 외부 API 허용
- PRIVATE: 기본 로컬, 명시적 승인 시 외부 전송
- CONFIDENTIAL: 로컬만 허용

## 자동 폴백 금지
Local→Cloud, Free→유료, Provider A→B, 실패 후 무한 재시도를 금지한다.

## 완료 정의
Mac OpenCode에서 Windows 로컬 모델로 Worktree 파일을 수정하고 테스트와 Diff를 확인한다. Ollama와 llama.cpp 비교, RAG 출처, ComfyUI VRAM 반환, 백업·복원까지 검증한다.
