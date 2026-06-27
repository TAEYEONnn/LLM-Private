# 81. Completion Checklist

작성 기준일: 2026-06-28

## Phase 0 — 기준선

- [ ] 실제 작업 10건 이상 기록
- [ ] 반복 설명과 반복 수정 유형 분류
- [ ] ChatGPT·Codex·Claude Code 사용량 불편 기록
- [ ] 외부 전송 불가 작업 여부 기록
- [ ] Wiki First 목표 확인

## Phase 1 — OpenKnowledge PUBLIC Pilot

- [ ] `docs/82-openknowledge-integration-plan.md` 확인
- [ ] 공식 Release 또는 Commit 고정
- [ ] Artifact SHA256 기록
- [ ] 별도 PUBLIC Pilot 저장소
- [ ] 기존 코드·LLM-Private 저장소에서 초기화하지 않음
- [ ] GitHub Auto Sync Off
- [ ] Semantic Search Off
- [ ] Local Telemetry Off
- [ ] Full Access·Auto Mode Off
- [ ] 초기화 전후 MCP·설정 Diff
- [ ] 읽기·검색 성공
- [ ] 새 Draft 생성 성공
- [ ] Draft 부분 편집 성공
- [ ] Timeline 복구 성공
- [ ] 테스트 문서 이동·Trash·복구 성공
- [ ] 프로젝트 밖 쓰기 0
- [ ] 자동 Git Push 0
- [ ] 롤백 검증
- [ ] `openknowledge-handoff.local.yml`

## Phase 2 — ChatGPT 읽기 연결

- [ ] 실행 당일 공식 지원 방식 확인
- [ ] Remote MCP·Custom App·Project 대안 비교
- [ ] 초기 권한이 검색·읽기로 제한됨
- [ ] Source Path 반환
- [ ] 승인 없는 쓰기 0
- [ ] Prompt Injection 대응
- [ ] Cloud로 전송되는 Query·문서 범위 기록
- [ ] 민감 자료 제외

## Phase 3 — 작업 완료 Hook

- [ ] `config/work-capture-schema.example.yml` 적용
- [ ] 작업 완료 시 이벤트 생성
- [ ] 작업 실패 시 이벤트 생성
- [ ] Build·Lint·Typecheck·Test 결과 기록
- [ ] Git diff 요약 기록
- [ ] Source Repository·Branch·Commit 기록
- [ ] 사용 제품·모델 기록
- [ ] Secret·Token·Cookie 제거
- [ ] Private IP와 개인정보 제거
- [ ] 전체 채팅 로그 저장 안 함
- [ ] 숨겨진 추론 저장 안 함
- [ ] 승인 후 OpenKnowledge 기록
- [ ] 기록 실패가 원본 작업을 손상시키지 않음

## Phase 4 — Wiki 승인 Workflow

- [ ] Agent 기본 문서 상태 `draft`
- [ ] `reviewed` 문서 변경은 제안 방식
- [ ] `approved` 직접 덮어쓰기 금지
- [ ] 사용자 승인 후 Approved 승격
- [ ] `deprecated` 문서 기본 검색 제외
- [ ] 자동 Commit·Push 0

## Phase 5 — 저장소 작업 연결

- [ ] Codex·Claude Code·OpenCode 중 Actor 선택
- [ ] Worktree 또는 안전한 Branch
- [ ] 파일 수정 범위 확인
- [ ] 적용 가능한 테스트 실행
- [ ] Git diff 확인
- [ ] 작업 완료 이벤트 생성
- [ ] OpenKnowledge 기록 승인

## Phase 6 — 사용량 측정

- [ ] 최소 14일 또는 작업 30건
- [ ] 반복 작업 비율
- [ ] Cloud 사용량 제한·비용 불편
- [ ] 재시도 수
- [ ] 사람 개입 횟수
- [ ] Wiki 검색·재사용 횟수
- [ ] 다시 설명하지 않아도 된 시간
- [ ] 외부 전송 불가 작업 수
- [ ] 로컬 처리 가능 정형 작업 비율

## Phase 7 — LOCAL-NEED Gate

- [ ] 실제 데이터 기반 판단
- [ ] Cloud만으로 충분한지 평가
- [ ] Local 구축 예상 절감 효과
- [ ] 설치·운영 시간 포함
- [ ] 민감 작업 필요성
- [ ] Batch 작업 필요성
- [ ] Local LLM 승인 또는 보류 결정 기록

## Phase 8 — Windows Local LLM Pilot

LOCAL-NEED 승인 후에만 진행한다.

- [ ] Windows 자원 진단
- [ ] Ollama Health Check
- [ ] llama.cpp Health Check
- [ ] Qwen2.5-Coder 7B Q4_K_M부터 시작
- [ ] Context 8192
- [ ] 동시 요청 1
- [ ] 동일 모델·동일 Fixture 비교
- [ ] Private LAN 방화벽
- [ ] Worktree 수정
- [ ] 테스트 실행
- [ ] Git diff 확인
- [ ] 기존 Work Capture Hook 재사용
- [ ] `windows-handoff.local.yml`

## Phase 9 — Mac Local 연결

- [ ] Windows API TCP Health Check
- [ ] Windows API HTTP Health Check
- [ ] OpenCode Local Profile
- [ ] 실제 Provider·Endpoint·Model ID 기록
- [ ] 자동 Local→Cloud Fallback 0
- [ ] RAM·VRAM·Swap 안전 기준

## AI-Knowledge Bootstrap

- [ ] `docs/83-openknowledge-bootstrap-plan.md` 확인
- [ ] OpenKnowledge Pilot 승인
- [ ] Wiki 승인 Workflow 안정화
- [ ] LLM-Private Source Commit 고정
- [ ] 이관 후보 문서 승인
- [ ] Local 설정·비밀값 제외
- [ ] Source Repository·Path·Commit Metadata
- [ ] 깨진 내부 링크 0
- [ ] 자동 Commit·Push 0
- [ ] LLM-Private 원본 변경 0
- [ ] Bootstrap Rollback 검증

## 선택 확장

- [ ] Open WebUI 필요성 확인
- [ ] 로컬 Embedding 필요성 확인
- [ ] Semantic Search 필요성 확인
- [ ] ComfyUI 필요성 확인
- [ ] 외부 Skill 필요성 확인
