# 41. Model Selection Policy

## 순서
실제 작업 정의 → 사전 선택 Fast 후보 → 동일 런타임 비교 → 승인 검증 → Fast 실패 과제만 Medium → 필요 시 Deep Experimental.

## 초기 Fast 후보

### Primary
- Qwen2.5-Coder-7B-Instruct Q4_K_M
- Ollama ID: `qwen2.5-coder:7b-instruct-q4_K_M`
- 코드 설명, 수정, 테스트 복구용

### Agentic Comparison
- Qwen3.5-9B Q4_K_M
- Ollama ID: `qwen3.5:9b-q4_K_M`
- Tool Calling, 장기 지시, 구조화 출력 비교용

### Mac Fallback Comparison
- Gemma 3 4B 또는 별도 승인된 4B급 모델
- Windows 코딩 기본 모델로 사용하지 않음

## Artifact 확정
- GGUF 저장소, 파일명, 분할 여부, SHA256은 설치 전에 추측하지 않는다.
- 실제 다운로드 후 파일명과 SHA256을 계산한다.
- 확정 전에는 `artifact.state: unresolved`를 유지한다.
- 확정값이 바뀌면 Stack ID와 승인 상태를 분리한다.

## 금지
최신 모델 자동 설치, 가장 큰 모델 자동 선택, 최대 컨텍스트 기본값, `latest` 태그, 한 번 성공으로 승인, 모델 크기만으로 Tool Calling 추정, 사용자 승인 전 후보 외 모델 다운로드.

## 기본값
Q4_K_M 또는 Q5 우선, 3비트 Experimental, Context 8192, Thinking Off, Temperature 0, 한 번에 모델 하나.
