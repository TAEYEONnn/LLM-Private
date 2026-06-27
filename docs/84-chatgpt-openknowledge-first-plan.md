# 84. ChatGPT + OpenKnowledge First Plan

작성 기준일: 2026-06-28  
상태: Approved Roadmap v1.0

## 1. 목적

로컬 LLM 구축보다 먼저 다음 가치를 검증한다.

```text
ChatGPT / Codex / Claude Code
→ 실제 작업 수행
→ 작업 완료 이벤트 생성
→ 승인된 요약만 OpenKnowledge에 기록
→ 다음 작업에서 검색·재사용
```

핵심 목표는 모델을 새로 운영하는 것이 아니라 다음 두 가지다.

1. 반복되는 작업 비용과 설명 비용을 줄인다.
2. 작업 과정에서 발생한 결정·결과·실패를 장기 지식으로 축적한다.

Windows Local LLM은 이 흐름에서 나중에 교체 가능한 Executor다. 초기 필수 조건이 아니다.

## 2. 역할

### ChatGPT

- 기획, 화면·이미지 분석, 문서화, 의사결정
- OpenKnowledge의 승인된 문서 검색
- 작업 종료 요약 생성

### Codex / Claude Code / OpenCode

- 저장소 파일 읽기·수정
- 명령과 테스트 실행
- Git diff 확인
- 구조화된 작업 완료 이벤트 생성

### OpenKnowledge

- 프로젝트 Wiki와 Source of Truth
- 결정, 실행 계약, 테스트 결과, 장애 기록
- Git 기반 변경 이력
- 여러 모델과 Agent 사이의 공용 기억

### Windows Local LLM

- 반복량·비용·보안 필요성이 실제로 확인된 뒤 추가하는 선택적 Executor
- OpenKnowledge와 Hook 구조를 변경하지 않고 실행 모델만 교체

## 3. Phase 0 — 기준선

설치 전에 현재 방식을 1주 또는 작업 10건 이상 기록한다.

기록 항목:

- 작업 유형
- 사용한 제품과 모델
- 소요 시간
- 반복 입력량
- 수정 파일 수
- 테스트 결과
- 작업 완료 여부
- 다시 설명해야 했던 맥락
- 사용량 제한 또는 비용 불편
- 외부 전송이 불가능했던 자료 여부

기준선 없이 로컬 LLM의 필요성을 추정하지 않는다.

## 4. Phase 1 — OpenKnowledge PUBLIC Pilot

`docs/82-openknowledge-integration-plan.md`를 따른다.

초기 조건:

- 별도 PUBLIC Pilot 저장소
- GitHub Auto Sync Off
- Semantic Search Off
- Local Telemetry Off
- Full Access·Auto Mode Off
- 기존 코드 저장소에서 `ok init` 금지

검증:

- 읽기·검색
- 문서 생성과 부분 편집
- 링크·백링크
- Timeline 복구
- Git diff
- 제거·롤백

Phase 1에는 Windows Local LLM이 필요하지 않다.

## 5. Phase 2 — ChatGPT 읽기 연결

실행 당일 ChatGPT 계정과 제품에서 지원되는 공식 연결 방식을 확인한다.

우선순위:

1. 공식 Remote MCP 또는 Custom App의 읽기 전용 연결
2. 안전한 승인형 Tunnel을 통한 읽기 전용 연결
3. 지원되지 않으면 ChatGPT Project에 승인 문서를 선별 업로드
4. 가장 단순한 대안으로 수동 Export·Import

초기 허용 도구:

- 문서 검색
- 문서 읽기
- 링크·백링크 조회
- Source Path 반환

초기 금지 도구:

- 문서 삭제
- 문서 이동
- 기존 승인 문서 덮어쓰기
- 자동 Commit·Push
- 승인 없는 외부 공유

읽기 연결이 안정화되기 전에는 ChatGPT에서 OpenKnowledge 쓰기를 허용하지 않는다.

## 6. Phase 3 — 작업 완료 Hook

코드 또는 문서 작업이 끝나면 Agent가 구조화된 완료 이벤트를 생성한다.

필수 흐름:

```text
작업 수행
→ Build / Lint / Test / Diff
→ 작업 완료 이벤트 생성
→ 민감정보 제거
→ 사용자 또는 정책 승인
→ OpenKnowledge 기록
```

기본 Schema는 `config/work-capture-schema.example.yml`을 따른다.

### 저장할 내용

- 요청의 최종 목적
- 사용한 제품·모델
- 실제 변경 파일
- 실행한 검증
- 성공·실패 결과
- 확정된 결정
- 실패 원인과 해결책
- 다음 작업
- Source Commit과 Branch

### 저장하지 않을 내용

- 모델의 비공개 사고 과정
- 전체 채팅 로그
- 중복된 오류 로그 원문
- API Key·Token·Cookie
- 실제 Private IP
- 개인정보
- 검증되지 않은 추측

## 7. Phase 4 — 제한된 쓰기

읽기 연결과 Hook이 안정화된 후에만 승인형 쓰기를 시험한다.

허용 순서:

1. 새 Draft 문서 생성
2. 기존 Draft 문서 부분 편집
3. Reviewed 문서 변경 제안
4. 사용자 승인 후 Approved 승격

`approved` 문서는 Agent가 직접 덮어쓰지 않는다. 변경 제안 또는 새 Revision을 만든다.

문서 상태:

```text
draft
→ reviewed
→ approved
→ deprecated
```

## 8. Phase 5 — Codex·Claude Code 작업 연결

실제 저장소 작업은 Codex 또는 Claude Code가 수행할 수 있다.

작업 완료 조건:

- Worktree 또는 안전한 Branch에서 수정
- Build·Lint·Typecheck·Test 중 적용 가능한 검증 실행
- Git diff 확인
- 변경 범위가 요청과 일치
- 작업 완료 이벤트 생성
- OpenKnowledge 기록 승인

ChatGPT는 기획·검토 인터페이스로 사용할 수 있지만, 실제 저장소 수정 결과는 Git과 테스트가 최종 판정한다.

## 9. Phase 6 — 사용량 측정

최소 2주 또는 작업 30건 동안 다음을 측정한다.

- 전체 작업 수
- 반복 작업 비율
- Agent가 처리한 작업 수
- Cloud 사용량 또는 사용 제한
- 재시도 수
- 사람 개입 횟수
- OpenKnowledge 재사용 횟수
- Wiki 검색으로 절약한 설명 시간
- 외부 전송 불가로 처리하지 못한 작업
- 로컬 모델로 처리 가능해 보이는 정형 작업 비율

초기 판단은 비용 추정이 아니라 실제 기록으로 내린다.

## 10. LOCAL-NEED Gate

다음 중 하나 이상이 실제 데이터로 확인될 때만 Windows Local LLM 구축을 승인한다.

- 반복 작업량이 충분히 많아 Cloud 사용 제한이나 비용이 지속적으로 불편함
- PRIVATE·CONFIDENTIAL 자료 때문에 Cloud로 처리할 수 없는 작업이 반복됨
- 장시간 Batch 작업이나 다량 변환 작업이 존재함
- 4B~9B 모델로 처리 가능한 정형 업무가 명확히 분리됨
- Cloud 장애·네트워크 단절 시에도 필요한 핵심 작업이 있음
- Local Executor 도입 예상 절감 효과가 설치·운영 비용보다 큼

다음 상황이면 보류한다.

- 대부분의 작업이 이미지 이해·고난도 추론·최신 정보에 의존함
- 반복 작업량이 적음
- ChatGPT·Codex 사용량 제한이 실제 문제로 나타나지 않음
- 로컬 7B 품질로는 재작업이 더 많을 것으로 예상됨
- 유지보수 시간을 확보하기 어려움

## 11. Phase 7 — Windows Local LLM Pilot

LOCAL-NEED Gate 통과 후에만 기존 Windows 구축 계획을 실행한다.

- Ollama와 llama.cpp 비교
- Qwen2.5-Coder 7B Q4_K_M부터 검증
- Context 8192
- 동시 요청 1
- Worktree 수정·테스트·Diff
- OpenKnowledge Hook 재사용
- Cloud와 동일 Fixture 비교

Local LLM은 Wiki나 Hook을 새로 만드는 것이 아니라 기존 Executor를 교체·추가하는 방식으로 붙인다.

## 12. 성공 기준

### Wiki First 성공

- 작업 완료 이벤트 기록 성공률 90% 이상
- 승인 문서 검색 성공률 95% 이상
- 자동 삭제·자동 Push 0
- 민감정보 기록 0
- 다음 작업에서 기존 기록을 실제로 재사용
- 기록 때문에 작업 시간이 과도하게 늘어나지 않음

### Local LLM 도입 성공

- Cloud 대비 반복 작업 비용 또는 제한이 의미 있게 감소
- 테스트 통과율이 승인 기준 충족
- 사람 개입과 재작업이 허용 범위 이내
- Wiki Hook과 기존 Workflow를 그대로 재사용

## 13. 작업 재개 문구

```text
README.md와 docs/84-chatgpt-openknowledge-first-plan.md를 읽어라.
현재 Phase를 확인하고, 로컬 LLM을 먼저 설치하지 마라.
OpenKnowledge PUBLIC Pilot과 작업 완료 Hook의 변경 범위, 권한, 생성 파일, 롤백 계획을 먼저 보고하라.
```
