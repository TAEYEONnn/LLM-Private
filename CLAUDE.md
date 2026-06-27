# CLAUDE.md

이 저장소는 Windows AI 서버와 macOS 클라이언트를 결합한 개인용 로컬·클라우드 하이브리드 LLM 워크스페이스 구축 명세다.

Claude Code는 이 파일을 실행 오케스트레이터로 사용하고, 세부 정책은 `docs/`와 `config/`를 따른다.

---

## 1. 최우선 목표

가장 큰 모델이나 가장 높은 TPS를 만드는 것이 목표가 아니다.

다음 최소 경로를 안전하고 재현 가능하게 완성한다.

```text
Mac OpenCode
→ Windows Local API
→ Git worktree
→ 파일 수정
→ 테스트 실행
→ Git diff 확인
```

성공 우선순위:

```text
작업 완료
→ 테스트 통과
→ 안전성
→ 도구 호출 정확성
→ 실패 복구
→ 사람 개입
→ 전체 작업 시간
→ TPS·TTFT
```

---

## 2. 변경 불가능한 운영 원칙

- 특정 제품이 아니라 OpenAI-compatible API 규격을 기준으로 통합한다.
- Ollama와 llama.cpp를 동등한 런타임 후보로 비교한다.
- 모델 하나가 아니라 모델·양자화·템플릿·런타임·하네스·도구 조합을 승인한다.
- 자동 Local→Cloud 전환을 만들지 않는다.
- 무료 Cloud→유료 Cloud 자동 전환을 만들지 않는다.
- 자동 결제와 자동 충전을 활성화하지 않는다.
- 실제 IP, API Key, Token, 비밀번호를 Git 추적 파일에 기록하지 않는다.
- 파일 쓰기 에이전트는 Git worktree에서 먼저 검증한다.
- 정확한 모델 파일명, SHA256, IP, 드라이브 문자를 추측하지 않는다.
- 최신 설치 명령과 지원 버전은 실행 당일 공식 문서로 검증한다.
- 한 단계가 실패해도 독립적인 다음 단계는 계속 진행하고 실패를 기록한다.

---

## 3. 데이터 등급

### PUBLIC
공개 저장소와 공개 문서. 승인된 외부 API 사용 가능.

### PRIVATE
개인 프로젝트와 비공개 포트폴리오. 기본 로컬이며 외부 전송은 명시적 승인 필요.

### CONFIDENTIAL
회사 내부 코드, 고객 데이터, 개인정보, 계약서, API Key, 미공개 자료. 로컬 실행만 허용.

현재 데이터 등급이 불명확하면 `PRIVATE`로 취급한다.

CONFIDENTIAL 데이터가 감지되면 Cloud·Codex·Claude 외부 처리 경로를 중단하고 사용자에게 알린다.

---

## 4. 시작 시 공통 절차

### 4.1 운영체제 확인

먼저 현재 환경을 판별한다.

- Windows: `docs/10-windows-build-prompt-v3.md` 경로를 따른다.
- macOS: `docs/20-macos-build-prompt-v3.md` 경로를 따른다.
- 그 외 운영체제에서는 설치하지 말고 지원 범위를 보고한다.

### 4.2 필수 문서 읽기

공통:

1. `README.md`
2. `docs/00-shared-architecture-v3.md`
3. `docs/02-scope-and-non-goals.md`
4. `docs/30-agent-stack-benchmark-v3.md`
5. `docs/31-benchmark-execution-guide.md`
6. `docs/40-model-provider-registry.md`
7. `docs/41-model-selection-policy.md`
8. `docs/50-security-and-data-policy.md`
9. `docs/51-agent-permission-model.md`
10. `docs/52-network-security-baseline.md`
11. `docs/60-backup-restore-migration.md`
12. `docs/80-implementation-roadmap.md`

Windows 추가:

- `docs/10-windows-build-prompt-v3.md`
- `config/windows-profile.example.yml`

macOS 추가:

- `docs/20-macos-build-prompt-v3.md`
- `config/macos-profile.example.yml`

### 4.3 Plan 단계 우선

첫 응답에서는 설치하지 않는다.

먼저 읽기 전용 진단과 실행 계획만 작성한다.

반드시 보고할 항목:

- 감지된 운영체제와 Build
- CPU, RAM, GPU, VRAM
- SSD/HDD 여유 공간
- 기존 설치 도구와 버전
- 충돌 가능성
- 필요한 관리자 권한
- 재부팅 가능성
- 방화벽 변경
- 예상 다운로드 용량
- 예상 디스크 사용량
- 단계별 롤백 방법
- 사용자 승인이 필요한 항목

계획을 보여준 뒤 승인을 기다린다.

---

## 5. 사용자 승인이 필요한 작업

다음은 실행 직전에 사용자 승인을 받는다.

- 관리자 권한 또는 `sudo`
- Windows 방화벽 규칙 추가·변경
- WSL, Docker, GPU 드라이버 설치·변경
- 재부팅
- 시스템 PATH 또는 셸 시작 파일 변경
- 5GB를 초과하는 단일 다운로드
- 기존 모델·컨테이너·가상환경 삭제
- 기존 포트 점유 프로세스 종료
- 유료 API Key 등록
- 외부 API 호출
- 실제 프로젝트 작업 트리 수정
- 파일 삭제
- 이메일, 배포, 캘린더 등 외부 부작용

권한 확인을 우회하는 옵션을 기본값으로 사용하지 않는다.

---

## 6. 모델 선택 계약

`config/model-registry.example.yml`의 사전 선택 후보를 따른다.

### Primary Fast

- Qwen2.5-Coder-7B-Instruct Q4_K_M
- Ollama ID: `qwen2.5-coder:7b-instruct-q4_K_M`
- 초기 Context: 8192
- Thinking: Off

### Agentic Comparison

- Qwen3.5-9B Q4_K_M
- Ollama ID: `qwen3.5:9b-q4_K_M`
- 초기 Context: 8192
- Thinking: Off

규칙:

- Primary부터 설치·검증한다.
- Primary가 하드웨어 또는 호환성 기준에 실패할 때만 Secondary를 검토한다.
- 다른 Fast 모델은 사용자 승인 없이 다운로드하지 않는다.
- Gemma 3 4B는 Windows 코딩 기본값이 아니라 Mac Fallback 또는 일반 작업 비교 후보다.
- GGUF 저장소, 실제 파일명, 분할 여부, SHA256은 다운로드 전에 추측하지 않는다.
- 다운로드 후 실제 Artifact를 계산해 레지스트리에 기록한다.
- Artifact가 확정되기 전에는 `artifact.state: unresolved`를 유지한다.

---

## 7. 네트워크와 로컬 설정 계약

실제 연결값은 다음 경로 중 하나에만 기록한다.

- `config/local/windows.local.yml`
- `config/local/macos.local.yml`
- `.env.local`
- 현재 프로세스 환경변수

위 파일들은 Git에 커밋하지 않는다.

### Windows IP 탐지

- 활성 Private 네트워크의 IPv4를 찾는다.
- Loopback, APIPA, VPN, 가상 어댑터를 제외한다.
- 후보가 하나면 로컬 설정에 기록한다.
- 후보가 여러 개이거나 없을 때만 사용자에게 선택을 요청한다.
- Mac에서 TCP와 HTTP Health Check가 성공해야 확정한다.

### macOS 연결

- `config/local/macos.local.yml` 또는 환경변수를 우선 사용한다.
- 예제 Placeholder를 실제 주소로 사용하지 않는다.
- Windows API에 TCP와 HTTP Health Check를 수행한다.
- Ping 성공만으로 연결 성공을 판정하지 않는다.

---

## 8. Windows 실행 경로

Windows를 먼저 구축한다.

### Phase 1 범위

- 환경 진단
- Ollama 설치·검증
- llama.cpp 설치·검증
- Primary Fast 모델 설치
- 동일 모델 기반 런타임 비교
- OpenCode 연결
- Git worktree 생성
- 테스트 Fixture 실행
- Private LAN 방화벽
- 로컬 설정과 Handoff 작성

### 금지

- Phase 1에서 Open WebUI, ComfyUI, Medium, Deep를 동시에 설치하지 않는다.
- Ollama 또는 llama.cpp를 벤치마크 전에 기본 런타임으로 확정하지 않는다.
- vLLM, SGLang, LocalAI, LiteLLM, Bifrost, llama-swap을 설치하지 않는다.
- 인터넷 포트포워딩을 만들지 않는다.

### Windows 완료 산출물

`config/local/windows-handoff.local.yml`을 생성한다.

포함 항목:

```yaml
windows:
  private_ipv4:
  selected_runtime:
  ollama_version:
  llama_cpp_version:
  api_endpoint:
  fast_primary_model_id:
  model_artifact_sha256:
  firewall_allowed_mac_ipv4:
  health_check_status:
  benchmark_status:
```

이 파일은 Git에 커밋하지 않는다.

Windows Phase 1 완료 전 macOS 구축을 시작하지 않는다.

---

## 9. macOS 실행 경로

macOS 구축은 Windows Handoff를 받은 뒤 시작한다.

### 실제 OS 확인

다음 결과를 기록한다.

```bash
sw_vers -productName
sw_vers -productVersion
sw_vers -buildVersion
uname -m
```

사용자 제공 기준은 macOS Tahoe 26.5.1이지만 실제 출력값을 우선한다.

### Phase 1 범위

- 실제 macOS와 Build 확인
- Homebrew와 개발 도구 진단
- Windows API Health Check
- OpenCode 설치·프로필 생성
- Windows Local Fast 연결
- Native Codex 프로필 분리
- Git worktree 테스트
- Mac Local Fallback 하나 설치
- 메모리 압력과 Swap 확인

### 금지

- Mac에 Open WebUI 또는 ComfyUI를 설치하지 않는다.
- 14B 이상 모델을 기본 폴백으로 설치하지 않는다.
- Windows 연결 실패를 이유로 자동 Cloud 전환하지 않는다.
- `.zshrc`를 백업과 Diff 없이 변경하지 않는다.

---

## 10. Git과 파일 안전

- 구축 저장소 자체 수정은 변경 범위를 먼저 설명한다.
- 실제 코딩 테스트는 별도 Git worktree에서 수행한다.
- 원본 작업 트리를 직접 수정하지 않는다.
- 동일 작업 트리를 여러 에이전트가 동시에 수정하지 않는다.
- `.env`, `config/local/`, Token, Credential, 로그 원문을 커밋하지 않는다.
- 자동 생성 파일을 추적하기 전에 사용 목적과 재현성을 확인한다.

---

## 11. 벤치마크 계약

`docs/30-agent-stack-benchmark-v3.md`와 `config/benchmark-v1.yml`을 따른다.

최소 비교 조건:

- 동일 업스트림 모델
- 동일 GGUF와 양자화
- 동일 Context
- 동일 Thinking
- 동일 채팅 템플릿
- 동일 OpenCode 버전
- 동일 Fixture

기본 쓰기 승인:

- Hard Fail 0
- Level 4 종단 성공률 80% 이상
- 테스트 통과율 85% 이상
- 허위 도구 호출 0
- 총점 80점 이상

TPS와 TTFT는 기록하지만 단독 승인 기준으로 사용하지 않는다.

---

## 12. 실패 처리

실패 시 다음 형식으로 기록한다.

```text
단계:
실행한 명령:
종료 코드:
핵심 오류:
추정 원인:
변경된 파일·설정:
롤백 상태:
다음 독립 단계 진행 가능 여부:
사용자 확인 필요 여부:
```

다음 상황에서는 중단하고 사용자에게 보고한다.

- CONFIDENTIAL 외부 전송 위험
- 실제 파일 삭제 필요
- 기존 데이터 손상 가능성
- 네트워크 후보가 여러 개이며 자동 판별 불가
- 설치 대상 공식 문서와 현재 환경이 충돌
- GPU 드라이버 변경 필요
- 유료 서비스 등록 필요
- 원본 작업 트리 변경 감지

---

## 13. 완료 보고 형식

각 플랫폼 작업이 끝나면 다음을 제공한다.

### 완료
- 설치·설정한 항목
- 실제 버전
- 실제 Endpoint
- 실제 Model ID
- 실제 Artifact SHA256
- 생성한 스크립트와 설정

### 검증
- Health Check
- Tool Calling
- Worktree 파일 수정
- 테스트 결과
- Git diff
- RAM·VRAM·Swap

### 미완료
- 실패 항목
- 실패 코드
- 원인
- 다음 작업

### 보안
- 실제 데이터 등급
- 외부 전송 발생 여부
- 자동 폴백 여부
- 비밀정보 Git 포함 여부

---

## 14. 첫 응답 규칙

Claude Code의 첫 응답은 다음 네 섹션만 제공한다.

1. 감지한 환경
2. 읽은 문서
3. Phase 1 실행 계획
4. 승인 필요한 항목

사용자 승인 전 설치 명령을 실행하지 않는다.
