# 83. OpenKnowledge Bootstrap Plan

작성 기준일: 2026-06-28  
상태: Candidate Migration Plan v2.0

## 1. 목적

현재 `LLM-Private` 저장소의 승인된 설계 문서를 OpenKnowledge 기반 `AI-Knowledge` 저장소로 선별 이관한다.

이 작업은 다음 조건이 충족된 뒤 진행한다.

- OpenKnowledge PUBLIC Pilot 승인
- ChatGPT 읽기 연결 또는 Project 대안 검증
- Draft·Reviewed·Approved Workflow 안정화
- 작업 완료 Hook 기본 동작 확인

Windows Local LLM과 CORE-LINK는 진입 조건이 아니다.

## 2. Source of Truth

초기 운영 기준:

```text
LLM-Private
= 시스템 구축 명세의 원본

AI-Knowledge
= 모델과 Agent가 읽기 쉬운 승인된 지식 사본
```

같은 문서를 양쪽에서 동시에 수정하지 않는다.

Bootstrap 기간에는 `LLM-Private → AI-Knowledge` 단방향 수동 이관만 허용한다.

## 3. 이관 대상

### 우선 이관

- 전체 Architecture
- 데이터·보안 정책
- Permission Model
- Network Baseline
- Backup·Restore·Migration
- Operations Runbook
- Troubleshooting Matrix
- Implementation Roadmap
- Decision Log
- OpenKnowledge Integration Plan
- ChatGPT + OpenKnowledge First Plan
- Work Capture Schema 설명
- Model Sequencing and Cost Governance
- Local LLM Stack Composition

### 조건부 이관

- Benchmark 결과
- Incident 기록
- 실제 Runtime·Model 승인 결과
- 설치 Handoff의 비밀값을 제거한 요약
- 작업 완료 이벤트의 장기 보존 가치가 있는 항목

### 이관 금지

- `config/local/`
- `.env` 및 Token
- 실제 Private IP
- API Key
- 원본 실행 로그
- 회사 코드와 내부 문서
- 개인정보
- 미검토 AI 생성 초안
- 전체 채팅 기록

## 4. 목표 구조

```text
AI-Knowledge/
├─ home.md
├─ architecture/
│  ├─ system-overview.md
│  ├─ local-llm-composition.md
│  └─ model-sequencing.md
├─ policies/
│  ├─ data-security.md
│  ├─ permissions.md
│  ├─ network.md
│  └─ cloud-cost.md
├─ integrations/
│  ├─ chatgpt.md
│  ├─ codex.md
│  ├─ opencode.md
│  └─ openknowledge.md
├─ work-log/
├─ decisions/
├─ runbooks/
├─ benchmarks/
├─ incidents/
├─ references/
└─ templates/
```

## 5. 문서 Metadata

이관 문서에는 다음 Frontmatter를 넣는다.

```yaml
---
title:
status: approved
data_class: PRIVATE
owner: user
source_repository: TAEYEONnn/LLM-Private
source_path:
source_commit:
synced_at:
verified_at:
review_cycle_days: 90
supersedes:
---
```

허용 상태:

- `draft`
- `reviewed`
- `approved`
- `deprecated`

Agent 실행 기준으로는 `approved` 문서만 사용한다.

## 6. 이관 절차

1. `LLM-Private`의 Source Commit을 고정한다.
2. 이관 후보 파일 목록을 작성한다.
3. 각 문서의 데이터 등급과 민감정보를 검사한다.
4. 중복·폐기 문서를 제외한다.
5. Wiki 구조에 맞게 문서를 복사한다.
6. Source Metadata를 추가한다.
7. 내부 링크를 OpenKnowledge 경로로 변환한다.
8. 원문 의미가 바뀌지 않았는지 Diff Review한다.
9. ChatGPT 또는 승인된 Cloud 모델이 구조와 누락을 검토한다.
10. Codex·Claude Code·OpenCode 중 하나가 링크·Frontmatter·파일 경로를 검사한다.
11. 사용자가 최종 승인한다.
12. 수동 Git Commit한다.

Local LLM이 승인된 뒤에는 10번 검사를 Local Executor로 교체할 수 있다.

## 7. AI 사용 규칙

### ChatGPT 또는 Cloud Reviewer

- 문서 분류
- 중복 탐지
- 구조 제안
- 요약 초안
- 링크 관계 제안

### Codex·Claude Code·OpenCode

- Markdown 변환
- Frontmatter 삽입
- 링크 경로 수정
- 파일 이동
- 정적 검사

### Local Executor

`LOCAL-NEED` Gate 승인 후 선택적으로 사용한다.

- 대량 Markdown 변환
- 반복적인 링크·Frontmatter 검사
- 외부 전송이 불가능한 문서 처리

### 금지

- AI가 원문을 근거 없이 확장
- AI 추측을 `approved`로 저장
- 전체 채팅 로그 저장
- 자동 Commit·Push
- Source 문서를 삭제 또는 덮어쓰기

## 8. Wiki Context 규칙

Agent는 Wiki 전체를 Context에 넣지 않는다.

```text
검색
→ 관련 approved 문서 3~8개
→ 필요한 단락만 추출
→ Source Path 표시
→ 실행 계약 생성
```

`draft`와 `deprecated` 문서는 기본 검색 결과에서 제외한다.

## 9. 동기화 정책

초기:

- 수동 Pull
- 수동 변환
- 수동 Review
- 수동 Commit
- GitHub Auto Sync Off

안정화 후 다음 조건에서만 자동화 후보를 검토한다.

- Source Commit 기록
- 민감정보 Scanner 통과
- 변경 Preview 제공
- 승인 전 Commit 금지
- 역방향 동기화 금지
- Conflict 발생 시 Fail Closed

## 10. 완료 기준

- [ ] Source Commit 고정
- [ ] 이관 후보 목록 승인
- [ ] 민감정보 0
- [ ] 모든 문서에 Source Metadata
- [ ] 승인 문서만 실행 기준으로 표시
- [ ] 깨진 내부 링크 0
- [ ] 중복 문서와 Deprecated 문서 분리
- [ ] 원문 의미 변경 없음
- [ ] 자동 Push 0
- [ ] Rollback 가능

## 11. Rollback

1. AI-Knowledge의 Bootstrap Commit을 Revert한다.
2. LLM-Private 원본은 변경하지 않는다.
3. OpenKnowledge Index를 재구축한다.
4. ChatGPT·Agent 검색 결과에서 이관 문서가 제거됐는지 확인한다.
5. 작업 완료 Hook Smoke Test를 다시 실행한다.

## 12. 작업 재개 문구

```text
OpenKnowledge PUBLIC Pilot과 Wiki 승인 Workflow가 승인됐다.
docs/83-openknowledge-bootstrap-plan.md를 읽고 LLM-Private의 승인 문서를 AI-Knowledge로 선별 이관할 계획을 작성하라.
아직 파일을 복사하지 말고 Source Commit, 후보 문서, 데이터 등급, 목표 경로, 예상 Diff를 먼저 보고하라.
```
