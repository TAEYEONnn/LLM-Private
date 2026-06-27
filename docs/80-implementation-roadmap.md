# 80. Implementation Roadmap

작성 기준일: 2026-06-28  
상태: Approved Roadmap v4.0

## Phase 0 — 기준선과 범위 확정

- 현재 ChatGPT·Codex·Claude Code 사용 방식 기록
- 최소 10개 실제 작업을 기준선으로 수집
- 반복 설명, 사용량 제한, 비용 불편, 민감 자료 문제 기록
- OpenKnowledge를 Source of Truth로 사용할지 최종 확인
- `docs/84-chatgpt-openknowledge-first-plan.md` 확인
- 로컬 LLM은 초기 필수가 아니라 조건부 Executor임을 확인

## Phase 1 — OpenKnowledge PUBLIC Pilot

세부 절차는 `docs/82-openknowledge-integration-plan.md`를 따른다.

- Mac OpenKnowledge Desktop Release 또는 Commit 고정
- 별도 PUBLIC Pilot 저장소 생성
- GitHub Sync Off
- Semantic Search Off
- Local Telemetry Off
- Full Access·Auto Mode Off
- 기존 코드 저장소와 `LLM-Private`에서 `ok init` 금지
- 읽기·검색·생성·부분 편집·복구 Smoke Test
- Git diff와 Rollback 검증

### Phase 1 Gate

- Hard Fail 0
- 프로젝트 밖 쓰기 0
- 자동 Git Push 0
- 민감정보 기록 0
- Timeline 복구 성공
- 제거·롤백 성공

## Phase 2 — ChatGPT 읽기 연결

실행 당일 공식 지원 범위를 확인한다.

우선순위:

1. 공식 Remote MCP 또는 Custom App 읽기 연결
2. 승인형 Tunnel을 통한 읽기 연결
3. 지원되지 않으면 ChatGPT Project에 승인 문서 선별 업로드
4. 수동 Export·Import

초기에는 검색·읽기만 허용하고 삭제·이동·덮어쓰기를 금지한다.

### Phase 2 Gate

- 승인 문서 검색 성공률 95% 이상
- Source Path 반환
- Prompt Injection 대응 확인
- 승인 없는 쓰기 0
- 외부 전송 범위 문서화

## Phase 3 — 작업 완료 Hook

세부 Schema는 `config/work-capture-schema.example.yml`을 따른다.

- ChatGPT·Codex·Claude Code·OpenCode 작업 종료 시 완료 이벤트 생성
- Build·Lint·Typecheck·Test·Git diff 결과 수집
- 민감정보 제거
- 사용자 또는 정책 승인 후 OpenKnowledge 기록
- 전체 채팅과 원시 로그는 저장하지 않음
- 작업 결과, 결정, 실패 원인, 다음 작업만 저장

### Phase 3 Gate

- 작업 완료 이벤트 기록 성공률 90% 이상
- 중복 기록과 불필요한 로그 최소화
- Secret·Token·Private IP 기록 0
- Approved 문서 자동 덮어쓰기 0
- 기록으로 인한 작업 지연이 허용 범위 이내

## Phase 4 — 제한된 Wiki 쓰기

- 새 Draft 생성
- 기존 Draft 부분 편집
- Reviewed 문서에는 변경 제안만 생성
- Approved 문서는 직접 덮어쓰지 않음
- 사용자 승인 후에만 Approved 승격
- 자동 Commit·Push 금지

## Phase 5 — Codex·Claude Code 저장소 작업 연결

- Git worktree 또는 안전한 Branch에서 수정
- 적용 가능한 Build·Lint·Typecheck·Test 실행
- Git diff 확인
- 작업 완료 이벤트 생성
- OpenKnowledge에 승인된 결과 기록
- Wiki 기록 실패가 코드 작업 자체를 손상시키지 않도록 분리

## Phase 6 — 실제 사용량 측정

최소 2주 또는 작업 30건 동안 측정한다.

- 반복 작업 비율
- Cloud 사용량 제한과 비용 불편
- 재시도와 사람 개입
- Wiki 검색·재사용 횟수
- 다시 설명하지 않아도 된 시간
- 외부 전송 불가 작업 수
- 소형 로컬 모델로 처리 가능한 정형 업무 비율

## Phase 7 — LOCAL-NEED 판단

세부 기준은 `docs/84-chatgpt-openknowledge-first-plan.md`를 따른다.

### Local 구축 승인 조건

다음 중 하나 이상이 실제 데이터로 확인되어야 한다.

- 반복 작업 때문에 Cloud 사용 제한이나 비용이 지속적으로 불편함
- PRIVATE·CONFIDENTIAL 작업이 반복적으로 Cloud 사용을 막음
- 장시간 Batch·다량 변환 작업이 존재함
- 4B~9B 모델로 처리 가능한 정형 작업이 분리됨
- 네트워크 단절 시에도 필요한 핵심 작업이 있음
- 예상 절감 효과가 설치·운영 비용보다 큼

조건이 확인되지 않으면 로컬 LLM 구축을 보류한다.

## Phase 8 — Windows Local LLM Pilot

LOCAL-NEED Gate 통과 후에만 수행한다.

- Windows 하드웨어·스토리지 진단
- Ollama 설치·검증
- llama.cpp 설치·검증
- Qwen2.5-Coder 7B Q4_K_M부터 시작
- Context 8192
- 동시 요청 1
- 동일 모델·동일 Fixture 런타임 비교
- Worktree 수정·테스트·Diff
- 기존 OpenKnowledge Hook 재사용
- Cloud 작업 대비 완료율·재작업·비용 비교

## Phase 9 — Mac Local Executor 연결

- Windows Local API Health Check
- Mac OpenCode Local Profile
- Private LAN 방화벽
- 자동 Local→Cloud Fallback Off
- 실제 Provider·Endpoint·Model ID 기록
- `config/local/windows-handoff.local.yml` 작성

## Phase 10 — 모델 Sequencing

- ChatGPT·Codex를 기본 인터페이스로 유지
- Sonnet은 필요 시 계획·Vision·검토에 사용
- Local Executor는 반복·민감 작업에 사용
- Opus는 예외 승인
- 자동 Sonnet→Opus, Local→Cloud 전환 금지
- 완료 작업당 비용 기록

## Phase 11 — 기존 Git 문서 Bootstrap

세부 절차는 `docs/83-openknowledge-bootstrap-plan.md`를 따른다.

- `LLM-Private` Source Commit 고정
- 승인 문서만 AI-Knowledge로 선별 이관
- Source Path·Commit·Data Class Metadata 추가
- 단방향 수동 이관
- 자동 Commit·Push 금지

이 단계는 Phase 1~4가 안정화된 뒤 진행하며 Local LLM을 요구하지 않는다.

## Phase 12 — Open WebUI RAG

필요가 확인된 경우에만 진행한다.

- PDF·Office 등 혼합 자료 RAG
- 출처 표시
- 자료 밖 답변 억제
- OpenKnowledge와 역할 분리

## Phase 13 — Semantic Search

- 기본 Lexical Search 품질 먼저 측정
- 외부 Embedding 사용 시 데이터 전송 승인
- 필요하면 Windows 로컬 Embedding Endpoint 구축
- 외부 Content Egress 0 검증

## Phase 14 — ComfyUI

- 이미지 생성·인페인팅·업스케일 필요성 확인
- Local LLM 모델 언로드 후 사용
- VRAM 반환 검증

## Phase 15 — Experimental

- Medium·Deep 모델
- LoRA
- Context Compression
- Memory Injection
- 외부 Skill

기본 Workflow가 안정화된 뒤 하나씩 A/B 검증한다.
