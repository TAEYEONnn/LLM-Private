# 81. Completion Checklist

## 공통
- [ ] 모델·공급자 레지스트리
- [ ] 실제 Model ID 기록
- [ ] 자동 폴백 Off
- [ ] 데이터 등급
- [ ] 백업·복원

## Windows
- [ ] Ollama Health Check
- [ ] llama.cpp Health Check
- [ ] 동일 모델·동일 조건 비교
- [ ] Private LAN 방화벽
- [ ] Worktree 수정
- [ ] Fast Level 4
- [ ] `windows-handoff.local.yml`

## Mac CORE-LINK
- [ ] Windows API TCP Health Check
- [ ] Windows API HTTP Health Check
- [ ] OpenCode Windows Local Fast 프로필
- [ ] Native Codex 분리
- [ ] Mac 폴백
- [ ] Worktree 파일 수정
- [ ] 테스트 실행
- [ ] Git diff 확인
- [ ] 실제 Provider·Endpoint·Model ID 기록
- [ ] 자동 Cloud Fallback 0
- [ ] RAM·VRAM·Swap 안전 기준

## OpenKnowledge PUBLIC Pilot
- [ ] `docs/82-openknowledge-integration-plan.md` 확인
- [ ] 공식 Release 또는 Commit 고정
- [ ] Artifact SHA256 기록
- [ ] 별도 PUBLIC Pilot 저장소
- [ ] 기존 코드·LLM-Private 저장소에서 초기화하지 않음
- [ ] GitHub Auto Sync Off
- [ ] Semantic Search Off
- [ ] Local Telemetry Off
- [ ] Full Access·Auto Mode Off
- [ ] 초기화 전후 MCP·OpenCode 설정 Diff
- [ ] 읽기·검색 성공
- [ ] 새 문서 생성 성공
- [ ] 부분 편집 성공
- [ ] Timeline 복구 성공
- [ ] 테스트 문서 이동·Trash·복구 성공
- [ ] Windows Local Fast 종단 문서 작업 성공
- [ ] 외부 전송 0
- [ ] 자동 Git Push 0
- [ ] 프로젝트 밖 쓰기 0
- [ ] 롤백 검증
- [ ] `openknowledge-handoff.local.yml`

## PRIVATE Knowledge Base
- [ ] 별도 Private `AI-Knowledge` 저장소
- [ ] 초기 수동 Commit 운영
- [ ] Branch와 충돌 정책
- [ ] GitHub Sync 별도 승인
- [ ] CONFIDENTIAL 데이터 반입 정책

## RAG / Semantic Search
- [ ] Open WebUI 출처 표시
- [ ] 자료 밖 답변 억제
- [ ] 로컬 Embedding Endpoint
- [ ] OpenKnowledge Semantic Search 로컬 전용
- [ ] 외부 Content Egress 0

## Image
- [ ] LLM 언로드
- [ ] 생성·편집·업스케일
- [ ] VRAM 반환
