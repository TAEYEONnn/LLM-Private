# 82. OpenKnowledge Integration Plan

작성 기준일: 2026-06-27  
상태: Candidate Integration Plan v1.0

## 1. 목적

Windows 로컬 LLM과 Mac OpenCode의 핵심 연결이 검증된 뒤, OpenKnowledge를 프로젝트 문서·LLM Wiki·에이전트 세컨드 브레인 계층으로 추가한다.

OpenKnowledge는 로컬 모델 런타임을 대체하지 않는다.

```text
Windows AI Server
├─ Ollama 또는 llama.cpp
├─ Fast Coding Model
└─ Embedding Model [후속 단계]

MacBook Air
├─ OpenCode
├─ Claude Code / Native Codex
├─ OpenKnowledge Desktop
└─ Browser

Private Git Repository
└─ AI-Knowledge
   ├─ architecture
   ├─ projects
   ├─ benchmarks
   ├─ decisions
   ├─ design-system
   └─ incidents
```

## 2. 진입 조건

아래 `CORE-LINK` Gate를 모두 통과하기 전에는 OpenKnowledge를 설치하거나 초기화하지 않는다.

- [ ] Windows Ollama 또는 llama.cpp API Health Check 성공
- [ ] Mac에서 Windows API 호출 성공
- [ ] 실제 Provider, Endpoint, Model ID 기록
- [ ] OpenCode에서 Windows Local Fast 프로필 작동
- [ ] Git worktree에서 테스트 파일 수정 성공
- [ ] 테스트 실행 성공
- [ ] Git diff 확인 성공
- [ ] 자동 Local→Cloud 폴백 없음
- [ ] Windows RAM·VRAM과 Mac Swap이 안전 기준 이내
- [ ] Core 결과를 `config/local/windows-handoff.local.yml`에 기록

CORE-LINK를 통과하면 동일 Claude Code 세션 또는 새 세션에서 이 문서를 읽고 OpenKnowledge Pilot으로 이어간다.

## 3. 역할 분리

### OpenKnowledge

- Markdown·MDX WYSIWYG 편집
- 링크·백링크·그래프
- 문서 Timeline과 선택적 복구
- 에이전트가 사용하는 MCP 문서 도구
- Git 기반 지식 저장소

### OpenCode

- Windows 로컬 모델 호출
- OpenKnowledge MCP 사용
- 문서 검색·작성·부분 수정
- 코드와 문서 작업 오케스트레이션

### Open WebUI

후속 단계에서 일반 채팅, PDF·Office 등 혼합 자료 RAG에 사용한다. OpenKnowledge와 경쟁 제품으로 보지 않고 역할을 분리한다.

## 4. 도입 원칙

- Apple Silicon Mac에서는 Desktop App을 우선한다.
- Windows CLI 설치는 초기 Pilot 범위에서 제외한다.
- `latest`를 사용하지 않고 검토 시점의 Release 또는 Commit을 고정한다.
- 별도 PUBLIC Pilot 저장소에서 먼저 검증한다.
- 현재 `LLM-Private` 저장소와 실제 개발 코드 저장소에서 `ok init`을 실행하지 않는다.
- GitHub Auto Sync를 기본 비활성화한다.
- Semantic Search를 기본 비활성화한다.
- 민감 Workspace에서는 Local Telemetry도 비활성화한다.
- Agent Full Access 또는 Auto Mode를 초기 기본값으로 사용하지 않는다.
- OpenKnowledge가 생성·수정하는 MCP, Skill, Agent 설정을 설치 전후 Diff로 확인한다.
- OpenKnowledge가 만든 파일을 신뢰하기 전에 경로·권한·외부 전송을 검토한다.

## 5. Phase 2B — Pilot 준비

### 5.1 Release 고정

1. 공식 GitHub Release와 공식 문서를 확인한다.
2. 설치할 Release Tag 또는 Commit SHA를 기록한다.
3. DMG 또는 Artifact의 SHA256을 기록한다.
4. `config/skill-registry.example.yml`의 OpenKnowledge 항목을 갱신한다.
5. 자동 업데이트는 비활성화하거나 업데이트 전 수동 검토를 요구한다.

### 5.2 별도 저장소 생성

초기 Pilot은 회사 자료나 기존 프로젝트를 사용하지 않는다.

권장 경로:

```text
~/AI-Workspace/openknowledge-pilot
```

권장 구조:

```text
openknowledge-pilot/
├─ README.md
├─ architecture/
├─ decisions/
├─ notes/
└─ test-data/
```

초기 문서는 PUBLIC 또는 인위적으로 만든 테스트 데이터만 사용한다.

### 5.3 설치 전 Snapshot

다음을 기록한다.

- `git status --short`
- `~/.config/opencode/` 파일 목록과 Hash
- 프로젝트의 `opencode.json`, `.mcp.json`, `.agents/` 존재 여부
- 실행 중인 관련 프로세스
- Listening Port
- 사용자 환경변수 중 관련 Key 이름만 기록

비밀값 자체는 기록하지 않는다.

## 6. Phase 2C — Mac Desktop 설치

### 6.1 설치

- OpenKnowledge macOS Desktop App을 공식 Release에서 설치한다.
- Gatekeeper 경고가 있으면 우회하지 않고 서명과 출처를 확인한다.
- 설치 과정에서 관리자 권한이 필요하면 실행 전에 승인을 받는다.

### 6.2 프로젝트 생성

- 별도 Pilot 디렉터리를 프로젝트로 연다.
- Knowledge Base는 Project Root에 초기화한다.
- 기존 저장소나 회사 프로젝트는 열지 않는다.

### 6.3 초기 설정

공유 설정 `.ok/config.yml`:

```yaml
content:
  dir: .
telemetry:
  localSink:
    enabled: false
```

장비별 설정 `.ok/local/config.yml`:

```yaml
autoSync:
  enabled: false
search:
  semantic:
    enabled: false
```

필수 검증:

```text
ok config validate
```

Desktop App만 사용하는 경우에도 Settings와 실제 파일 내용을 모두 확인한다.

## 7. Phase 2D — OpenCode MCP 연결

`ok init` 또는 Desktop 초기화가 다음 위치를 수정할 수 있다고 가정한다.

- `.ok/`
- `.agents/skills/`
- `opencode.json`
- `.mcp.json`
- `~/.config/opencode/opencode.json`
- `~/.ok/`

### 연결 절차

1. OpenKnowledge 초기화 전 Snapshot을 저장한다.
2. 초기화를 실행한다.
3. 변경 파일을 모두 Diff한다.
4. MCP Server 이름, 실행 명령, Working Directory를 확인한다.
5. 사용자 전역 OpenCode 설정 변경이 발생하면 Project 설정으로 축소 가능한지 검토한다.
6. 자동 추가된 Skill과 지시문을 읽는다.
7. 원격 다운로드, Hook, Daemon, 외부 Endpoint가 있으면 중단하고 보고한다.
8. OpenCode에서 실제 Provider가 Windows Local Fast인지 확인한다.
9. Cloud Provider가 등록되거나 Fallback으로 사용되지 않는지 확인한다.

## 8. Phase 2E — 권한 단계별 Smoke Test

처음부터 Full Access를 부여하지 않는다.

### Level OK-0 — 읽기

- [ ] 문서 목록 조회
- [ ] 문서 본문 읽기
- [ ] 키워드 검색
- [ ] 링크·백링크 조회
- [ ] 프로젝트 밖 경로 접근 거부

### Level OK-1 — 생성

- [ ] 새 문서 한 개 생성
- [ ] Frontmatter 보존
- [ ] Markdown Source와 WYSIWYG 결과 일치
- [ ] Agent Activity에 변경 기록
- [ ] Git diff로 변경 확인

### Level OK-2 — 부분 편집

- [ ] 기존 문서의 특정 단락만 수정
- [ ] 관련 없는 문단 변경 없음
- [ ] 링크와 Asset 경로 유지
- [ ] Timeline에 편집 Burst 기록

### Level OK-3 — 복구

- [ ] 테스트 문서를 의도적으로 잘못 수정
- [ ] Timeline에서 이전 버전 확인
- [ ] 선택적 복구 성공
- [ ] Git 상태 정상

### Level OK-4 — 제한된 삭제·이동

별도 테스트 문서에서만 수행한다.

- [ ] 문서 이동
- [ ] 링크 갱신 확인
- [ ] Trash 이동 확인
- [ ] 복구 확인
- [ ] 프로젝트 밖 삭제 불가

## 9. Phase 2F — 로컬 LLM 종단 테스트

Windows Local Fast 모델을 사용해 다음 작업을 수행한다.

### 과제 1 — 지식 베이스 생성

```text
로컬 LLM 워크스페이스의 구조를 설명하는 개요 문서를 만들고,
Windows 서버, Mac 클라이언트, 보안 정책을 각각 별도 문서로 분리해 연결하라.
```

### 과제 2 — 결정 로그 갱신

```text
제공된 테스트 문서를 근거로 결정 사항과 미결정 사항을 분리하고,
새 결정 문서를 작성한 뒤 관련 페이지에 링크하라.
```

### 과제 3 — 불일치 탐지

```text
두 개의 테스트 명세를 비교해 충돌하는 설정을 찾고,
원문을 변경하지 않은 상태로 충돌 보고서를 작성하라.
```

### 성공 기준

- [ ] 실제 Provider가 Windows Local Fast
- [ ] Cloud 호출 0
- [ ] 허위 문서 경로 0
- [ ] 불필요한 파일 수정 0
- [ ] MCP Tool Error 없이 작업 완료
- [ ] Timeline과 Git diff 일치
- [ ] 사람 개입 1회 이하
- [ ] RAM·VRAM·Swap 안전 기준 이내

## 10. Phase 2G — 외부 전송 검사

Pilot 중 다음을 확인한다.

- Semantic Search가 Off인지
- OpenAI Embedding Endpoint 호출이 없는지
- GitHub Auto Sync가 Off인지
- GitHub OAuth Token이 생성되지 않았는지
- Support Bundle을 생성하거나 전송하지 않았는지
- 외부 Telemetry Push가 비활성화되어 있는지
- OpenKnowledge, OpenCode, Browser의 외부 연결 대상

알 수 없는 외부 Endpoint가 발견되면 Pilot을 중단한다.

## 11. 승인 Gate

### `approved-pilot`

아래를 모두 만족하면 별도 PRIVATE 지식 저장소 도입을 승인한다.

- Hard Fail 0
- 외부 전송 0
- 읽기·검색 성공률 95% 이상
- 생성·부분 편집 성공률 90% 이상
- Timeline 복구 100%
- 프로젝트 밖 쓰기 0
- 자동 Git Push 0
- Core LLM Baseline 성능 저하 20% 미만
- 제거와 롤백 절차 검증

### `rejected`

다음 중 하나라도 발생하면 제거한다.

- 승인 없는 외부 전송
- 자동 Cloud Provider 사용
- 프로젝트 밖 파일 수정
- 자동 Git Push 또는 Remote History 변경
- 비밀값 로그 기록
- 원본 저장소 손상
- 비활성화할 수 없는 Hook·Worker·Daemon

## 12. Phase 2H — PRIVATE 지식 저장소 승격

Pilot 승인 후 별도 Private Git 저장소를 만든다.

권장 이름:

```text
AI-Knowledge
```

권장 구조:

```text
AI-Knowledge/
├─ home.md
├─ architecture/
├─ projects/
├─ design-system/
├─ benchmarks/
├─ decisions/
├─ runbooks/
├─ incidents/
├─ references/
└─ templates/
```

초기에는 수동 Git Commit만 사용한다.

GitHub Sync는 다음 조건을 모두 만족할 때만 별도 승인한다.

- Private 저장소
- Branch Protection 또는 별도 Sync Branch
- Commit되지 않은 변경 없음
- 충돌 복구 Drill 성공
- 자동 Commit 형식 검토
- OAuth Scope 확인

## 13. Semantic Search 후속 단계

Semantic Search는 기본 Off로 유지한다.

활성화 조건:

- Windows에 로컬 OpenAI-compatible Embedding Endpoint 구축
- Embedding Model 승인
- PRIVATE 데이터 외부 전송 없음
- Query와 Page Content가 로컬 Endpoint로만 전송됨을 확인
- 재인덱싱 시간과 저장공간 측정
- Lexical Search 대비 실제 검색 품질 개선 확인

Cloud Embedding은 PUBLIC Pilot에서 별도 승인을 받지 않는 한 사용하지 않는다.

## 14. 롤백

1. OpenCode에서 `open-knowledge` MCP를 비활성화한다.
2. 자동 생성된 Project Skill을 제거하거나 격리한다.
3. `opencode.json`, `.mcp.json`, 사용자 전역 설정을 Snapshot으로 복원한다.
4. OpenKnowledge App을 종료한다.
5. 관련 Worker·Port가 남아 있지 않은지 확인한다.
6. Pilot 저장소는 보존하거나 삭제 전 사용자 승인을 받는다.
7. macOS App 제거는 설정·데이터 경로를 확인한 후 수행한다.
8. 제거 후 Core LLM Smoke Test를 다시 실행한다.

## 15. 완료 산출물

- `config/local/openknowledge-handoff.local.yml`
- 설치 Release 또는 Commit
- Artifact SHA256
- 변경된 MCP·OpenCode 설정 Diff
- Smoke Test 결과
- 네트워크 외부 전송 검사 결과
- Timeline 복구 결과
- GitHub Sync 상태
- Semantic Search 상태
- 승인 상태

Handoff 예시:

```yaml
openknowledge:
  status: approved-pilot
  version:
  release_or_commit:
  artifact_sha256:
  project_path:
  mcp_name: open-knowledge
  opencode_profile: windows-local-fast
  github_sync: false
  semantic_search: false
  local_telemetry: false
  external_egress_detected: false
  smoke_test_status:
  rollback_verified:
```

## 16. 작업 재개 문구

CORE-LINK 완료 후 Claude Code에 다음과 같이 요청한다.

```text
Windows 로컬 LLM과 Mac OpenCode의 CORE-LINK 검증이 끝났다.
docs/82-openknowledge-integration-plan.md를 읽고 Phase 2B Pilot 준비부터 시작하라.
현재는 설치하지 말고 Release, 변경 범위, 권한, 예상 생성 파일, 롤백 계획을 먼저 보고하라.
```
