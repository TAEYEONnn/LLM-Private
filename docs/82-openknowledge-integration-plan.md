# 82. OpenKnowledge Integration Plan

작성 기준일: 2026-06-28  
상태: Approved Wiki-First Plan v2.0

## 1. 목적

OpenKnowledge를 Windows Local LLM 이후에 붙이는 부가 기능이 아니라, 프로젝트 지식·작업 이력·Agent 인수인계를 관리하는 첫 번째 핵심 계층으로 도입한다.

초기 구조:

```text
ChatGPT / Codex / Claude Code / OpenCode
                ↓
        읽기·검색·작업 수행
                ↓
       구조화된 완료 이벤트
                ↓
          OpenKnowledge
                ↓
        Git 기반 Markdown Wiki
```

Windows Local LLM은 이 Pilot의 진입 조건이 아니다. 필요성이 확인되면 이후 동일한 Hook과 Wiki에 추가한다.

## 2. Phase 1 진입 조건

OpenKnowledge Pilot은 다음 조건만 충족하면 시작할 수 있다.

- [ ] Mac의 운영체제와 저장공간 확인
- [ ] 별도 PUBLIC Pilot 디렉터리 준비
- [ ] Git 설치와 로컬 저장소 사용 가능
- [ ] 기존 프로젝트와 분리된 테스트 데이터 준비
- [ ] 설치 전 Snapshot과 Rollback 계획 작성
- [ ] 외부 Skill·MCP 설치 승인

Windows Ollama, llama.cpp, Local Model, Windows API는 필요하지 않다.

## 3. 역할 분리

### OpenKnowledge

- Markdown·MDX WYSIWYG 편집
- 링크·백링크·그래프
- 문서 Timeline과 복구
- MCP 기반 문서 검색·작성
- Git 기반 장기 지식 저장

### ChatGPT

- 기획·분석·이미지 이해
- 승인 문서 검색과 근거 정리
- 작업 종료 요약 초안

### Codex·Claude Code·OpenCode

- 저장소 파일 수정
- 명령·테스트 실행
- Git diff 생성
- 작업 완료 이벤트 생성

### Windows Local LLM

- `LOCAL-NEED` Gate 통과 후 추가 가능한 Executor
- OpenKnowledge Pilot과 작업 Hook을 다시 만들지 않고 기존 구조를 재사용

## 4. 도입 원칙

- Apple Silicon Mac에서는 Desktop App을 우선한다.
- `latest` 대신 검토한 Release 또는 Commit을 고정한다.
- 별도 PUBLIC Pilot 저장소에서 시작한다.
- `LLM-Private`와 실제 코드 저장소에서 바로 `ok init`을 실행하지 않는다.
- GitHub Auto Sync를 기본 비활성화한다.
- Semantic Search를 기본 비활성화한다.
- 민감 Workspace에서는 Local Telemetry를 비활성화한다.
- Full Access와 Auto Mode를 초기값으로 사용하지 않는다.
- 설치 전후 MCP·Skill·Agent 설정 Diff를 확인한다.
- 승인 문서는 Agent가 직접 덮어쓰지 않는다.

## 5. Pilot 준비

### 5.1 Release 고정

1. 공식 Repository와 문서를 확인한다.
2. 설치할 Release Tag 또는 Commit SHA를 기록한다.
3. DMG 또는 Artifact SHA256을 기록한다.
4. 자동 업데이트를 비활성화하거나 업데이트 전 검토를 요구한다.
5. `config/skill-registry.example.yml`과 `config/openknowledge-pilot.example.yml`을 갱신한다.

### 5.2 별도 저장소

권장 경로:

```text
~/AI-Workspace/openknowledge-pilot
```

권장 구조:

```text
openknowledge-pilot/
├─ README.md
├─ projects/
├─ decisions/
├─ work-log/
├─ templates/
└─ test-data/
```

초기 데이터는 PUBLIC 또는 인위적인 Fixture만 사용한다.

### 5.3 설치 전 Snapshot

- `git status --short`
- 프로젝트 파일 목록과 Hash
- `opencode.json`, `.mcp.json`, `.agents/` 존재 여부
- `~/.config/opencode/` 관련 파일
- 실행 중인 관련 프로세스
- Listening Port
- 관련 환경변수 이름만 기록

비밀값 자체는 저장하지 않는다.

## 6. Mac Desktop 설치

- 공식 Release에서 Desktop App을 설치한다.
- Gatekeeper 경고를 무조건 우회하지 않는다.
- 관리자 권한이 필요하면 실행 직전에 승인받는다.
- 별도 Pilot 디렉터리만 연다.
- 기존 코드 저장소나 회사 자료를 열지 않는다.

초기 설정 예시:

```yaml
content:
  dir: .
telemetry:
  localSink:
    enabled: false
```

장비별 설정:

```yaml
autoSync:
  enabled: false
search:
  semantic:
    enabled: false
```

설정 파일과 실제 UI 상태를 모두 확인한다.

## 7. 권한 단계별 Smoke Test

### Level OK-0 — 읽기·검색

- [ ] 문서 목록 조회
- [ ] 본문 읽기
- [ ] 키워드 검색
- [ ] 링크·백링크 조회
- [ ] 프로젝트 밖 경로 접근 거부

### Level OK-1 — 새 Draft 생성

- [ ] 새 문서 한 개 생성
- [ ] 기본 상태가 `draft`
- [ ] Frontmatter 보존
- [ ] Markdown Source와 WYSIWYG 결과 일치
- [ ] Git diff 확인

### Level OK-2 — Draft 부분 편집

- [ ] 특정 단락만 수정
- [ ] 관련 없는 문단 변경 없음
- [ ] 링크와 Asset 경로 유지
- [ ] Timeline에 변경 기록

### Level OK-3 — 복구

- [ ] 테스트 문서를 의도적으로 잘못 수정
- [ ] 이전 버전 확인
- [ ] 선택적 복구 성공
- [ ] Git 상태 정상

### Level OK-4 — 제한된 이동·삭제

별도 테스트 문서에서만 수행한다.

- [ ] 이동 후 링크 확인
- [ ] Trash 이동 확인
- [ ] 복구 확인
- [ ] 프로젝트 밖 삭제 불가

## 8. ChatGPT 읽기 연결

실행 당일 ChatGPT 계정에서 공식적으로 지원되는 연결 방식을 확인한다.

우선순위:

1. 공식 Remote MCP 또는 Custom App
2. 승인된 Tunnel을 통한 Remote MCP
3. ChatGPT Project에 승인 문서 선별 업로드
4. 수동 Export·Import

초기 허용:

- 검색
- 읽기
- Source Path 반환
- 관련 문서 목록 반환

초기 금지:

- 삭제
- 이동
- Approved 문서 수정
- 자동 Commit·Push
- 승인 없는 외부 공유

OpenKnowledge가 로컬에 있어도 ChatGPT 연결 과정에서 검색 Query와 선택된 문서 일부가 Cloud로 전송될 수 있으므로, 데이터 등급과 연결 범위를 문서화한다.

## 9. 작업 완료 Hook

기본 Schema는 `config/work-capture-schema.example.yml`을 따른다.

```text
작업 수행
→ Build / Lint / Typecheck / Test / Git diff
→ 구조화된 완료 이벤트 생성
→ Secret·개인정보 제거
→ 승인
→ OpenKnowledge 저장
```

저장 대상:

- 최종 요청 목적
- 실제 변경 파일
- 실행한 검증
- 확정된 결정
- 실패 원인과 해결책
- 다음 작업
- Source Repository·Branch·Commit
- 사용한 제품·모델·프로필

저장 제외:

- 전체 대화
- 숨겨진 추론
- 원시 Terminal Log 전체
- 비밀값
- 실제 Private IP
- 검증되지 않은 추측

## 10. 문서 승인 Workflow

```text
draft
→ reviewed
→ approved
→ deprecated
```

규칙:

- Agent는 기본적으로 `draft`만 생성한다.
- Reviewed 문서는 변경 제안으로 수정한다.
- Approved 문서는 직접 덮어쓰지 않는다.
- Approved 승격은 사용자 또는 명시된 승인 절차가 수행한다.
- Deprecated 문서는 실행 Context에서 기본 제외한다.

## 11. 승인 Gate

### `approved-pilot`

- Hard Fail 0
- 프로젝트 밖 쓰기 0
- 자동 Git Push 0
- 승인 없는 외부 전송 0
- 읽기·검색 성공률 95% 이상
- Draft 생성·부분 편집 성공률 90% 이상
- Timeline 복구 100%
- Secret·Token 기록 0
- 제거·롤백 검증

### `rejected`

다음 중 하나라도 발생하면 제거 또는 재설계한다.

- 프로젝트 밖 파일 수정
- 자동 Push 또는 Remote History 변경
- 비밀값 로그 기록
- 승인 없는 Approved 문서 덮어쓰기
- 비활성화할 수 없는 Hook·Worker·Daemon
- 알 수 없는 외부 Endpoint로의 전송

## 12. PRIVATE 지식 저장소 승격

Pilot 승인 후 별도 Private 저장소를 만든다.

권장 이름:

```text
AI-Knowledge
```

권장 구조:

```text
AI-Knowledge/
├─ home.md
├─ projects/
├─ decisions/
├─ work-log/
├─ runbooks/
├─ incidents/
├─ references/
└─ templates/
```

초기에는 수동 Commit만 사용한다.

GitHub Sync는 Branch·Conflict·OAuth Scope·Rollback 검증 후 별도 승인한다.

## 13. 기존 문서 Bootstrap

`docs/83-openknowledge-bootstrap-plan.md`를 따른다.

이 단계는 OpenKnowledge Pilot과 승인 Workflow가 안정화된 뒤 수행하며 Windows Local LLM을 요구하지 않는다.

## 14. Local LLM 연계

최소 2주 또는 작업 30건 측정 후 `LOCAL-NEED` Gate를 통과한 경우에만 연계한다.

Local LLM이 추가되더라도 다음은 그대로 유지한다.

- OpenKnowledge 문서 구조
- 작업 완료 Hook
- 승인 Workflow
- Git 검증
- 작업 이벤트 Schema

변경되는 것은 Executor Profile뿐이다.

## 15. 롤백

1. ChatGPT·Codex·Claude Code·OpenCode의 OpenKnowledge 연결을 비활성화한다.
2. 자동 생성된 Project Skill과 MCP 설정을 격리한다.
3. Snapshot으로 설정을 복원한다.
4. OpenKnowledge App을 종료한다.
5. 관련 Worker·Port가 남아 있지 않은지 확인한다.
6. Pilot 저장소 삭제는 사용자 승인 후 수행한다.
7. Git 원본에서 복구 가능함을 확인한다.

## 16. 완료 산출물

- `config/local/openknowledge-handoff.local.yml`
- 설치 Release 또는 Commit
- Artifact SHA256
- 변경된 MCP·Agent 설정 Diff
- Smoke Test 결과
- 외부 전송 범위
- Timeline 복구 결과
- GitHub Sync 상태
- Semantic Search 상태
- Hook 상태
- 승인 상태

## 17. 작업 재개 문구

```text
README.md, docs/82-openknowledge-integration-plan.md,
docs/84-chatgpt-openknowledge-first-plan.md를 읽어라.
Windows Local LLM을 먼저 설치하지 마라.
OpenKnowledge PUBLIC Pilot의 Release, 변경 범위, 권한, 생성 파일, 외부 전송, 롤백 계획을 먼저 보고하라.
```
