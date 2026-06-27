# CLAUDE.md

이 저장소는 ChatGPT·Codex·Claude Code·OpenCode와 OpenKnowledge를 연결해 작업 결과를 축적하고, 필요성이 실제로 확인된 뒤 Windows Local LLM을 선택적으로 추가하는 개인 AI 워크스페이스 구축 명세다.

Claude Code는 이 파일을 실행 오케스트레이터로 사용하고, 세부 정책은 `docs/`와 `config/`를 따른다.

---

## 1. 최우선 목표

가장 큰 모델이나 가장 높은 TPS를 만드는 것이 목표가 아니다.

먼저 다음 경로를 안전하고 재현 가능하게 완성한다.

```text
ChatGPT / Codex / Claude Code / OpenCode
→ 실제 작업 수행
→ Build / Test / Git diff
→ 구조화된 작업 완료 이벤트
→ OpenKnowledge 기록
→ 다음 작업에서 검색·재사용
```

Windows Local LLM은 초기 필수 요소가 아니다.

최소 2주 또는 작업 30건을 측정하고 `LOCAL-NEED` Gate를 통과한 경우에만 구축한다.

성공 우선순위:

```text
지식 재사용
→ 작업 완료
→ 테스트 통과
→ 안전성
→ 기록 품질
→ 사람 개입 감소
→ 비용·사용량 개선
→ 모델 속도
```

---

## 2. 변경 불가능한 운영 원칙

- OpenKnowledge를 프로젝트 지식의 Source of Truth로 검증한다.
- 전체 대화가 아니라 승인된 결과·결정·실패 원인만 기록한다.
- Agent가 생성한 문서는 기본적으로 `draft`다.
- `approved` 문서를 Agent가 직접 덮어쓰지 않는다.
- 자동 Git Push를 활성화하지 않는다.
- 자동 Local→Cloud 또는 Cloud→Local 전환을 만들지 않는다.
- 무료 Cloud→유료 Cloud 자동 전환을 만들지 않는다.
- 자동 결제와 자동 충전을 활성화하지 않는다.
- 실제 IP, API Key, Token, Cookie, 비밀번호를 Git 추적 파일에 기록하지 않는다.
- 파일 쓰기 Agent는 Worktree 또는 안전한 Branch에서 먼저 검증한다.
- 최신 설치 명령과 지원 버전은 실행 당일 공식 문서로 검증한다.
- 정확한 Release, Model ID, SHA256, IP, 경로를 추측하지 않는다.
- 권한 확인을 우회하는 옵션을 기본값으로 사용하지 않는다.
- 로컬 LLM 필요성이 입증되기 전에는 모델 서버를 설치하지 않는다.

---

## 3. 데이터 등급

### PUBLIC

공개 저장소와 공개 문서. 승인된 외부 API와 Cloud 제품 사용 가능.

### PRIVATE

개인 프로젝트와 비공개 포트폴리오. Cloud 사용 전 전달 범위를 확인하고 최소 Context만 사용한다.

### CONFIDENTIAL

회사 내부 코드, 고객 데이터, 개인정보, 계약서, API Key, 미공개 자료.

승인된 로컬 환경에서만 처리한다. Local Executor가 아직 구축되지 않았다면 작업을 중단하거나 별도 회사 승인 절차를 따른다.

현재 데이터 등급이 불명확하면 `PRIVATE`로 취급한다.

---

## 4. 필수 문서

작업 시작 전 다음을 읽는다.

1. `README.md`
2. `docs/02-scope-and-non-goals.md`
3. `docs/44-model-sequencing-and-cost-governance.md`
4. `docs/50-security-and-data-policy.md`
5. `docs/51-agent-permission-model.md`
6. `docs/52-network-security-baseline.md`
7. `docs/60-backup-restore-migration.md`
8. `docs/80-implementation-roadmap.md`
9. `docs/81-completion-checklist.md`
10. `docs/82-openknowledge-integration-plan.md`
11. `docs/83-openknowledge-bootstrap-plan.md`
12. `docs/84-chatgpt-openknowledge-first-plan.md`
13. `docs/93-skill-evaluation-policy.md`
14. `config/openknowledge-pilot.example.yml`
15. `config/work-capture-schema.example.yml`
16. `config/orchestration-policy.example.yml`

Windows Local LLM을 검토할 때만 추가로 읽는다.

- `docs/10-windows-build-prompt-v3.md`
- `docs/20-macos-build-prompt-v3.md`
- `docs/30-agent-stack-benchmark-v3.md`
- `docs/31-benchmark-execution-guide.md`
- `docs/40-model-provider-registry.md`
- `docs/41-model-selection-policy.md`
- `docs/43-local-llm-stack-composition.md`
- `config/windows-profile.example.yml`
- `config/macos-profile.example.yml`
- `config/model-registry.example.yml`
- `config/provider-registry.example.yml`

---

## 5. 현재 Phase 판정

작업 시작 시 `docs/80-implementation-roadmap.md`와 실제 산출물을 보고 현재 Phase를 판정한다.

기본 순서:

```text
Phase 0  기준선
Phase 1  OpenKnowledge PUBLIC Pilot
Phase 2  ChatGPT 읽기 연결
Phase 3  작업 완료 Hook
Phase 4  제한된 Wiki 쓰기
Phase 5  Codex·Claude Code 저장소 작업 연결
Phase 6  최소 2주 또는 작업 30건 측정
Phase 7  LOCAL-NEED 판단
Phase 8  Windows Local LLM Pilot [조건부]
Phase 9  Mac Local Executor 연결 [조건부]
```

앞 단계 Gate가 통과되지 않았으면 뒤 단계 설치를 시작하지 않는다.

---

## 6. 첫 실행 원칙

첫 응답에서는 설치하지 않는다.

읽기 전용 진단과 계획만 작성한다.

반드시 보고할 항목:

- 현재 Phase
- 운영체제와 Build
- 설치 대상과 변경 범위
- 생성·수정될 파일
- 사용자 설정 변경
- 필요한 관리자 권한
- 새 프로세스와 Port
- 외부 네트워크 연결
- 데이터 전송 범위
- 예상 다운로드·디스크 사용량
- Rollback 방법
- 사용자 승인 항목

계획을 보여준 뒤 승인을 기다린다.

---

## 7. 사용자 승인이 필요한 작업

- 관리자 권한 또는 `sudo`
- 외부 Skill·Plugin·MCP 설치
- Hook 등록
- 사용자 전역 설정 변경
- 백그라운드 Worker·Daemon 실행
- Remote MCP 또는 Tunnel 설정
- 외부 API 호출
- 유료 API Key 등록
- GitHub OAuth 연결
- 자동 Sync 활성화
- Semantic Search 활성화
- 실제 프로젝트 작업 트리 수정
- 파일 삭제·이동
- Git Commit·Push
- 기존 Port 점유 프로세스 종료
- 5GB를 초과하는 단일 다운로드
- Windows 방화벽 변경
- WSL·Docker·GPU Driver 변경
- 재부팅
- Local LLM 설치

---

## 8. Phase 1 — OpenKnowledge PUBLIC Pilot

`docs/82-openknowledge-integration-plan.md`를 따른다.

초기 조건:

- 별도 PUBLIC Pilot 저장소
- macOS Desktop App 우선
- Release 또는 Commit 고정
- Artifact SHA256 기록
- GitHub Auto Sync Off
- Semantic Search Off
- Local Telemetry Off
- Full Access·Auto Mode Off
- 기존 코드 저장소와 `LLM-Private`에서 `ok init` 금지

Smoke Test 순서:

1. 읽기·검색
2. 새 Draft 생성
3. Draft 부분 편집
4. Timeline 복구
5. 테스트 문서 이동·Trash·복구
6. Git diff 확인
7. Rollback 검증

Windows Local LLM은 요구하지 않는다.

---

## 9. Phase 2 — ChatGPT 읽기 연결

실행 당일 ChatGPT 계정과 제품에서 지원되는 공식 연결 방식을 확인한다.

우선순위:

1. 공식 Remote MCP 또는 Custom App
2. 승인형 Tunnel을 통한 Remote MCP
3. ChatGPT Project에 승인 문서 선별 업로드
4. 수동 Export·Import

초기 권한:

- 검색
- 읽기
- Source Path 반환
- 관련 문서 목록 반환

초기 금지:

- 문서 삭제
- 문서 이동
- Approved 문서 수정
- 자동 Commit·Push
- 승인 없는 외부 공유

OpenKnowledge가 로컬에 있어도 ChatGPT 연결 과정에서 Query와 선택된 문서 일부가 Cloud로 전송될 수 있음을 명시한다.

---

## 10. Phase 3 — 작업 완료 Hook

`config/work-capture-schema.example.yml`을 따른다.

기본 흐름:

```text
작업 수행
→ Build / Lint / Typecheck / Test / Git diff
→ 완료 이벤트 생성
→ Secret·개인정보 제거
→ 승인
→ OpenKnowledge 기록
```

저장할 내용:

- 최종 요청 목적
- 사용한 제품·모델 또는 Mode
- 실제 변경 파일
- 실행한 검증
- 성공·실패 결과
- 확정된 결정
- 실패 원인과 해결책
- 다음 작업
- Source Repository·Branch·Commit

저장하지 않을 내용:

- 전체 채팅
- 모델의 비공개 사고 과정
- 원시 Terminal Log 전체
- Token·Cookie·Password
- 실제 Private IP
- 개인정보
- 검증되지 않은 추측

Hook 실패가 원본 코드·문서 작업을 손상시키지 않도록 기록 경로를 분리한다.

---

## 11. Phase 4 — Wiki 승인 Workflow

문서 상태:

```text
draft
→ reviewed
→ approved
→ deprecated
```

- Agent는 기본적으로 `draft`만 생성한다.
- Reviewed 문서는 변경 제안을 만든다.
- Approved 문서는 직접 덮어쓰지 않는다.
- Approved 승격은 사용자 또는 명시된 승인 절차가 수행한다.
- Deprecated 문서는 기본 Context에서 제외한다.
- 자동 Commit·Push를 금지한다.

---

## 12. Phase 5 — 저장소 작업 연결

Codex·Claude Code·OpenCode 중 사용자가 선택한 Actor가 작업할 수 있다.

필수 조건:

- Worktree 또는 안전한 Branch
- 요청 범위 밖 파일 수정 금지
- 적용 가능한 Build·Lint·Typecheck·Test 실행
- Git diff 확인
- 완료 이벤트 생성
- OpenKnowledge 기록 승인

모델의 자기평가보다 Git과 테스트를 최종 판정자로 사용한다.

---

## 13. Phase 6 — 사용량 측정

최소 14일 또는 작업 30건 동안 다음을 기록한다.

- 반복 작업 비율
- Cloud 사용량 제한과 비용 불편
- 재시도 수
- 사람 개입 횟수
- Wiki 검색·재사용 횟수
- 다시 설명하지 않아도 된 시간
- 외부 전송 불가 작업 수
- 소형 로컬 모델로 처리 가능한 정형 작업 비율

기준선 없이 Local LLM을 승인하지 않는다.

---

## 14. Phase 7 — LOCAL-NEED Gate

다음 중 하나 이상이 실제 데이터로 확인될 때만 Local LLM을 승인한다.

- Cloud 사용 제한이나 비용이 지속적으로 불편함
- 외부 전송 불가 작업이 반복됨
- 장시간 Batch·대량 변환 작업이 존재함
- 4B~9B 모델로 처리 가능한 정형 업무가 명확함
- 네트워크 단절 시에도 필요한 작업이 있음
- 예상 절감 효과가 설치·운영 비용보다 큼

다음이면 보류한다.

- 반복 작업량이 적음
- ChatGPT·Codex만으로 충분함
- 대부분의 작업이 Vision·고난도 추론에 의존함
- 로컬 7B의 재작업 비용이 클 것으로 예상됨
- 유지보수 시간을 확보하기 어려움

결정은 OpenKnowledge의 Decision Log에 기록한다.

---

## 15. Phase 8 — Windows Local LLM [조건부]

LOCAL-NEED 승인 후에만 수행한다.

초기 후보:

- Qwen2.5-Coder-7B-Instruct Q4_K_M
- Context 8192
- Thinking Off
- 동시 요청 1

비교 대상:

- Ollama
- llama.cpp

필수 검증:

- 동일 Model·Quantization·Context·Template
- Worktree 파일 수정
- 테스트 실행
- Git diff
- 기존 Work Capture Hook 재사용
- Cloud 작업 대비 완료율·재작업·운영시간 비교

Local LLM은 Wiki와 Hook을 새로 만드는 것이 아니라 Executor만 추가한다.

---

## 16. Local 하드웨어 안전 기준

Local LLM Phase에서만 적용한다.

공통:

- 한 번에 하나의 LLM 모델만 로드
- 동시 추론 요청 1개
- Context 8192부터 시작
- Ollama와 llama.cpp 모델 동시 로드 금지
- LLM과 ComfyUI 동시 GPU 로드 금지
- Overclock·Undervolt·BIOS 변경 금지

Windows:

- 사용 가능 RAM 10GB 미만이면 새 모델 로드 금지
- SSD 여유 100GB 미만이면 대형 모델 다운로드 금지
- `nvidia-smi` 실패 시 GPU Benchmark 금지
- RAM 85% 이상이 지속되면 Run 중단
- 사용 가능 RAM 4GB 미만이면 즉시 생성 중단
- VRAM 95% 이상이 지속되면 모델 Unload
- 12B 이상은 별도 승인
- 20B 이상은 초기 Pilot 제외

macOS:

- Memory Pressure Yellow면 추가 모델 로드 금지
- Red면 모델 종료
- Swap 8GB 초과 시 Run 중단
- 14B 이상은 기본 운영 범위에서 제외

---

## 17. Git과 파일 안전

- 원본 작업 트리를 직접 수정하지 않는다.
- 동일 Worktree를 여러 Agent가 동시에 수정하지 않는다.
- `.env`, `config/local/`, Token, Credential, 원시 로그를 커밋하지 않는다.
- 자동 생성 파일을 추적하기 전에 사용 목적을 확인한다.
- OpenKnowledge 기록과 코드 변경 Commit을 분리할 수 있어야 한다.
- 자동 Push를 금지한다.

---

## 18. 외부 Skill·Plugin·MCP

`docs/93-skill-evaluation-policy.md`와 `config/skill-registry.example.yml`을 따른다.

절대 규칙:

- 공식 Repository 또는 Marketplace만 사용
- Commit SHA 또는 Release Tag 고정
- 원격 Script 즉시 실행 금지
- Hook·Setup Script 사전 검토
- Project-level 설치 우선
- 자동 업데이트 Off
- 설치 전후 파일·프로세스·Port·환경변수 Diff
- Rollback 없는 Skill 설치 금지

OpenKnowledge가 현재 최우선 후보다.

다른 Skill은 Wiki와 Work Capture Workflow가 안정화된 뒤 하나씩 시험한다.

---

## 19. 실패 처리

```text
단계:
실행한 명령:
종료 코드:
핵심 오류:
추정 원인:
변경된 파일·설정:
외부 전송:
롤백 상태:
다음 독립 단계 진행 가능 여부:
사용자 확인 필요 여부:
```

다음 상황에서는 중단한다.

- 승인 없는 외부 전송
- 실제 파일 삭제 필요
- 원본 저장소 손상 가능성
- Approved 문서 자동 덮어쓰기
- 자동 Git Push
- 비밀정보 기록
- 예상하지 않은 Hook·Daemon·Port·Telemetry
- 유료 서비스 등록 필요
- Local LLM Phase 전 모델 서버 설치 시도
- RAM·VRAM·Swap 안전 임계치 초과

---

## 20. 완료 보고

### 완료

- 설치·설정한 항목
- 실제 Version·Release·Commit
- 생성·수정 파일
- 연결된 Actor와 권한
- Hook 상태

### 검증

- 읽기·검색
- Draft 생성·편집
- Timeline 복구
- Build·Test·Git diff
- 작업 완료 이벤트
- Secret Redaction
- Rollback

### 보안

- 데이터 등급
- 외부 전송 범위
- 자동 Fallback 여부
- 자동 Push 여부
- 비밀정보 포함 여부

### 다음 Phase

- Gate 통과 여부
- 미완료 항목
- Local LLM 필요성 측정 상태

---

## 21. 첫 응답 규칙

Claude Code의 첫 응답은 다음 다섯 섹션만 제공한다.

1. 감지한 환경
2. 읽은 문서
3. 판정한 현재 Phase
4. 실행 계획
5. 승인 필요한 항목

사용자 승인 전 설치나 설정 변경을 실행하지 않는다.
