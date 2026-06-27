# 80. Implementation Roadmap

## Phase 0 — 기준선과 구성 원칙

- Native Codex 기준 작업 10개 기록
- T00~T12 Fixture 확정
- 현재 하드웨어·네트워크·스토리지 진단
- `docs/43-local-llm-stack-composition.md` 확인
- `docs/44-model-sequencing-and-cost-governance.md` 확인
- 새 Foundation Model 학습이 아니라 기존 Open-weight Model과 Runtime을 조합하는 프로젝트임을 확인

## Phase 1 — Windows Core

- Ollama 설치·검증
- llama.cpp 설치·검증
- Fast Primary 모델 설치
- 동일 모델 런타임 비교
- OpenCode 연결 준비
- Git worktree
- Private LAN 방화벽
- Windows Handoff 작성

## Phase 2A — Mac 연결과 CORE-LINK Gate

- Windows API Health Check
- Mac OpenCode 프로필
- Windows Local Fast 연결
- Native Codex 분리
- Mac Local Fallback
- Worktree 파일 수정
- 테스트 실행
- Git diff 확인
- 자동 Cloud Fallback 0 확인

### CORE-LINK 완료 기준

- Mac에서 Windows Local Model 호출 성공
- 실제 Provider·Endpoint·Model ID 기록
- Worktree 수정·테스트·Diff 성공
- RAM·VRAM·Swap 안전 기준 통과
- `config/local/windows-handoff.local.yml` 작성

CORE-LINK 통과 전에는 OpenKnowledge, Open WebUI, ComfyUI, 외부 Skill을 추가하지 않는다.

## Phase 2B — OpenKnowledge PUBLIC Pilot

세부 절차는 `docs/82-openknowledge-integration-plan.md`를 따른다.

- Mac OpenKnowledge Desktop Release 고정
- 별도 PUBLIC Pilot 저장소 생성
- GitHub Sync Off
- Semantic Search Off
- 민감 Workspace Local Telemetry Off
- OpenCode MCP 설정 Diff 검토
- 읽기·검색·생성·부분 편집·복구 Smoke Test
- Windows Local Fast 종단 문서 작업
- 외부 전송 검사
- 롤백 검증

### OpenKnowledge Pilot Gate

- Hard Fail 0
- 외부 전송 0
- 프로젝트 밖 쓰기 0
- 자동 Git Push 0
- 읽기·검색 95% 이상
- 생성·부분 편집 90% 이상
- Timeline 복구 100%
- Core Baseline 성능 저하 20% 미만

## Phase 2C — PRIVATE Knowledge Base 승격

OpenKnowledge Pilot 승인 후에만 수행한다.

- 별도 Private `AI-Knowledge` 저장소
- architecture, projects, design-system, benchmarks, decisions, runbooks, incidents 구조
- 초기에는 수동 Commit만 사용
- GitHub Auto Sync는 별도 승인
- 회사 CONFIDENTIAL 자료는 로컬 정책 검증 전 반입 금지

## Phase 2D — 기존 Git 문서 Bootstrap

세부 절차는 `docs/83-openknowledge-bootstrap-plan.md`를 따른다.

- `LLM-Private` Source Commit 고정
- 승인된 문서만 선별 이관
- Source Path·Commit·Data Class Metadata 추가
- `LLM-Private → AI-Knowledge` 단방향 수동 이관
- Sonnet이 분류·구조를 검토
- Local Executor가 Frontmatter·링크·경로를 검사
- 자동 Commit·Push 금지
- 원본 `LLM-Private` 문서 변경 금지

## Phase 3 — 벤치마크 자동화와 승인 상태

- Run 로그
- 자동 채점
- Stack 승인 상태
- 회귀 세트
- OpenKnowledge MCP 활성·비활성 비교
- Sonnet Planner와 Local Executor의 종단 작업 비교

## Phase 4 — 명시적 모델 Sequencing

- 기본 Planner·Vision·Reviewer: Claude Sonnet 4.6
- PRIVATE·CONFIDENTIAL Executor: Windows Local Fast
- PUBLIC Budget Cloud Executor 후보 Benchmark
- Opus 4.8은 사용자 승인 기반 예외 승격
- `config/orchestration-policy.example.yml` 적용
- 자동 Sonnet→Opus, Local→Cloud 전환 금지
- 완료 작업당 비용 기록

## Phase 5 — Open WebUI RAG

- 일반 채팅
- PDF·Office 등 혼합 자료 RAG
- 출처 표시
- OpenKnowledge와 역할 분리

## Phase 6 — 로컬 Embedding과 Semantic Search

- Windows OpenAI-compatible Embedding Endpoint
- Embedding 모델 승인
- OpenKnowledge Semantic Search 로컬 연결
- 외부 Content Egress 0 검증
- Lexical 대비 품질 비교

## Phase 7 — ComfyUI

- LLM 언로드
- 이미지 생성·인페인팅·업스케일
- VRAM 반환 검증

## Phase 8 — Medium / Deep Experimental

- Fast 실패 과제만 Medium으로 확장
- Deep는 Experimental 유지
- 20B 이상은 별도 승인
- Prompt·RAG·Tool 개선으로 해결되지 않는 반복 실패에만 LoRA 검토

## Phase 9 — Cloud PAYG

- 월 10달러 하드 캡
- 자동 충전 Off
- 자동 Fallback Off
- Sonnet 기본 Planner
- Budget Cloud는 PUBLIC 작업부터 검증
- Opus는 예외 승인

## Phase 10 — 외부 Skill 선택 도입

- Superpowers Project-level 시험
- UI/UX Pro Max 디자인 저장소 전용 시험
- Headroom은 Baseline 완료 후 A/B
- Memory·Proxy·Compression은 하나씩만 추가
