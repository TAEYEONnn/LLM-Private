# 70. Operations Runbook

## Local Fast
ComfyUI 중지 → Fast Stack → 실제 Endpoint/Model 출력 → Health Check → OpenCode → 언로드.

## Local Medium
Fast 실패 과제만 사용.

## Local Deep
불필요 앱 종료, 4K/8K 확인, RAM/VRAM 모니터링, 작업 후 즉시 언로드.

## RAG
Open WebUI, 로컬 Embedding, SOURCE_REQUIRED, 출처 확인.

## Image
LLM 언로드 → VRAM 반환 → ComfyUI → 작업 후 반환.

## Cloud
데이터 등급 → Provider/Model → 자동 충전/폴백 Off → 비용 기록.
