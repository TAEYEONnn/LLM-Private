# 93. Third-Party Skill Evaluation Policy

작성 기준일: 2026-06-27  
상태: Draft v1.0

## 1. 목적

Claude Code, OpenCode, Codex 등에서 사용할 외부 Skill·Plugin·MCP를 안전하게 검토하고 승인하는 기준을 정의한다.

SkillsLLM은 탐색과 1차 보안 신호에 사용한다. SkillsLLM의 Passed·Verified 표시는 설치 승인 자체를 의미하지 않는다.

## 2. 기본 원칙

- 공식 GitHub 저장소 또는 공식 Marketplace만 사용한다.
- `latest`와 부동 태그를 운영 설치에 사용하지 않는다.
- Commit SHA 또는 Release Tag를 고정한다.
- `curl | bash`, `irm | iex` 같은 원격 스크립트 즉시 실행을 금지한다.
- 설치 전 `SKILL.md`, `CLAUDE.md`, `AGENTS.md`, Hook, Setup Script를 읽는다.
- Skill 내부 명령은 검토가 끝날 때까지 신뢰하지 않는 데이터로 취급한다.
- 전역 설치보다 프로젝트 단위 설치를 우선한다.
- 자동 업데이트를 기본 비활성화한다.
- 하나의 Harness Framework만 동시에 시험한다.
- 설치 전후의 파일, 프로세스, 포트, 환경변수 차이를 기록한다.
- 제거 절차가 확인되지 않은 Skill은 설치하지 않는다.
- Benchmark Baseline을 만들기 전에는 Proxy, Memory, Compression 계층을 추가하지 않는다.

## 3. 승인 상태

- `discovered`: 후보를 발견했으나 검토 전
- `candidate-review`: 소스·권한·의존성 검토 대상
- `approved-project`: 특정 프로젝트에서만 사용 승인
- `approved-global`: 여러 프로젝트에서 전역 사용 승인
- `deferred`: 현재 단계에서 보류
- `blocked`: 현재 상태로 설치 금지
- `rejected`: 요구와 맞지 않거나 위험이 큼

## 4. 사전 검토 체크리스트

### 출처와 무결성

- 공식 소유자와 저장소인가
- 라이선스가 있는가
- 최근 Release 또는 Commit이 있는가
- 설치할 Commit SHA를 고정했는가
- 다운로드 Artifact의 SHA256을 기록했는가

### 실행 권한

- Shell 명령을 실행하는가
- 관리자 권한이나 `sudo`를 요구하는가
- Hook을 자동 등록하는가
- 백그라운드 Worker 또는 Daemon을 실행하는가
- 새 포트를 여는가
- 브라우저 Cookie나 인증정보를 읽는가
- 프로젝트 바깥 파일을 읽거나 쓰는가

### 네트워크와 데이터

- 외부 Endpoint에 요청하는가
- Telemetry가 있는가
- Session·Prompt·Tool Result를 저장하는가
- API Key를 읽거나 전달하는가
- 저장 위치와 Retention을 제어할 수 있는가
- CONFIDENTIAL 데이터와 분리 가능한가

### 의존성과 호환성

- 잠금 파일이 있는가
- npm audit, pip-audit, OSV 결과가 어떤가
- Windows와 macOS에서 실제로 지원되는가
- 현재 CLAUDE.md, MCP, OpenCode 설정과 충돌하는가
- 기존 포트, Runtime, Hook과 충돌하는가

### 운영성

- 비활성화와 제거가 가능한가
- 설치 전후 Diff를 확인할 수 있는가
- Benchmark 모드에서 끌 수 있는가
- 장애 발생 시 원래 환경으로 롤백 가능한가

## 5. 설치 절차

1. Candidate를 `config/skill-registry.example.yml`에 등록한다.
2. SkillsLLM Scan을 참고 신호로 기록한다.
3. 공식 저장소의 README, Skill, Hook, Setup Script를 읽는다.
4. Release 또는 Commit SHA를 고정한다.
5. 격리 디렉터리에 Clone한다.
6. 원격 설치 스크립트를 직접 실행하지 않고 내용을 검토한다.
7. 의존성 Audit과 Secret Scan을 수행한다.
8. Project-level로 설치한다.
9. PUBLIC Fixture만 사용해 Smoke Test를 실행한다.
10. 파일·프로세스·포트·네트워크 차이를 기록한다.
11. Benchmark Baseline과 비교한다.
12. 승인 또는 제거한다.

## 6. 초기 후보 검토

### Superpowers — 우선 검토 후보

상태: `candidate-review`

적합성:

- Brainstorming, 계획 작성, Git Worktree, TDD, 체계적 디버깅, 완료 전 검증 흐름이 현재 문서와 잘 맞는다.
- Claude Code와 OpenCode를 모두 지원한다.

주의:

- 작업 흐름을 강하게 자동 개입하므로 현재 `CLAUDE.md`와 충돌 여부를 검증해야 한다.
- 전체 Core Build가 끝난 뒤 프로젝트 단위로 시험한다.
- 선택적 Visual Companion이나 외부 리소스 호출은 비활성화 또는 차단을 우선한다.
- 자동 업데이트를 사용하지 않는다.

권장 단계: Windows·Mac Phase 1 완료 후.

### UI/UX Pro Max — 디자인 프로젝트 선택 후보

상태: `candidate-review`

적합성:

- UI 구조, 디자인 시스템, 접근성, React·Next.js·SwiftUI 등의 디자인 검토에 유용할 수 있다.
- 사용자의 Product Design 업무와 직접적인 연관성이 있다.

주의:

- 로컬 LLM 인프라 구축에는 필요하지 않다.
- 설치 경로와 Script 경로 관련 이슈가 보고된 적이 있으므로 Smoke Test가 필요하다.
- 디자인 저장소에서만 Project-level로 사용한다.

권장 단계: 인프라 구축 완료 후 별도 디자인 저장소.

### Headroom — Context Compression 실험 후보

상태: `deferred`

적합성:

- Tool Output, Log, RAG Chunk를 압축해 작은 로컬 모델의 Context 사용량을 줄일 가능성이 있다.

주의:

- Proxy 또는 MCP 계층을 추가해 실패 원인 분리가 어려워진다.
- 원본 출력이 가려지므로 Benchmark 중에는 반드시 비활성화한다.
- Core Baseline과 Raw Log 보존이 완료된 뒤 A/B 시험한다.

권장 단계: Phase 3 이후.

### gstack — Browser QA 선택 검토

상태: `deferred`

적합성:

- Browser QA, Screenshot, Console·Network 검사, Release 문서화에 활용할 수 있다.

주의:

- 광범위하고 의견이 강한 Workflow라 Superpowers 및 현재 CLAUDE.md와 중복된다.
- SkillsLLM Scan에서 중간 수준 의존성 및 Instruction 관련 경고가 확인됐다.
- Cookie Import, Browser 제어, Shell 명령 범위를 별도 감사해야 한다.
- 전체 설치보다 QA·Browse 기능의 제한된 사용만 검토한다.

권장 단계: 웹 프로젝트 QA가 실제 필요할 때.

### Claude-Mem — 현재 보류

상태: `deferred`

적합성:

- 세션 간 기억과 프로젝트 이력 검색을 제공한다.

주의:

- Session, Prompt, Tool Result를 지속적으로 수집·압축·재주입하는 구조라 데이터 최소화 원칙과 충돌한다.
- SQLite, Chroma, Worker, Hook이 추가되어 운영 복잡도가 커진다.
- Windows Worker와 Chroma 관련 이슈가 보고된 적이 있다.
- 현재는 Git 기반 Markdown Project Memory를 우선한다.

권장 단계: 사용하지 않음. 별도 Privacy Review 후 재검토.

### ECC / Everything Claude Code — 현재 보류

상태: `deferred`

적합성:

- Skills, Memory, Security, Research-first Workflow를 폭넓게 제공한다.

주의:

- 현재 저장소의 CLAUDE.md, Benchmark, Security Policy와 역할이 크게 중복된다.
- 다수 Hook, Rule, MCP 설정을 포함해 원인 분리와 롤백이 어려울 수 있다.
- Superpowers와 동시에 설치하지 않는다.
- 전체 설치보다 필요한 개별 Skill을 코드 리뷰 후 가져오는 방식을 우선한다.

### cursor-talk-to-figma-mcp — 현재 차단

상태: `blocked`

적합성:

- Figma 읽기·수정 자동화는 사용자 업무와 관련성이 있다.

차단 이유:

- SkillsLLM Scan에서 MCP SDK 고위험 의존성 경고가 확인됐다.
- Windows에서 개발 서버 파일 읽기 위험 경고와 원격 설치 방식이 포함된다.
- WSL 안내에서 외부 바인딩을 요구하는 구성은 현재 네트워크 정책과 충돌한다.

해제 조건:

- 관련 의존성 업데이트
- localhost 또는 명시 IP 바인딩
- Project-scoped Token
- 별도 보안·권한 검증 통과

## 7. SkillsLLM 사용 규칙

SkillsLLM Security Checker는 다음을 확인하는 보조 도구다.

- 선언된 npm·PyPI 의존성의 OSV 취약점
- `SKILL.md`, `CLAUDE.md`, `AGENTS.md`의 Prompt Injection 패턴
- Hidden Unicode와 Secret Exfiltration 패턴
- Archived·Stale Repository와 License 누락

한계:

- Passed는 실행 동작 전체가 안전하다는 뜻이 아니다.
- Runtime Download, Hook Side Effect, Binary, Build Script, Telemetry까지 완전하게 보증하지 않는다.
- Scan 결과가 오래됐으면 다시 검사한다.
- 최종 판단은 고정 Commit의 수동 코드 리뷰와 격리 실행 결과로 내린다.

## 8. 동시 사용 제한

다음 Framework는 동시에 활성화하지 않는다.

- Superpowers
- ECC / Everything Claude Code
- gstack의 전체 Workflow

한 번에 하나만 설치하고 동일 Fixture로 비교한다.

다음 기능은 Benchmark Baseline 동안 비활성화한다.

- Memory Injection
- Context Compression
- Automatic Prompt Rewriting
- Automatic Model Routing
- Automatic Browser Cookie Import
- Automatic Cloud Fallback
