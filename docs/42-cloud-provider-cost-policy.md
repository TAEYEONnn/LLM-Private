# 42. Cloud Provider and Cost Policy

작성 기준일: 2026-06-28

## 역할

초기 Workflow는 ChatGPT·Codex·Claude Code 등 현재 사용하는 제품을 그대로 활용한다.

특정 Cloud 모델을 전체 시스템의 기본 Planner로 강제하지 않는다. 제품 안에서 선택한 모델이나 Mode를 작업 로그에 기록한다.

선택적 후보:

- Claude Sonnet: 복잡한 계획·Vision·Review
- Claude Opus: 예외적 고난도 작업, 사용자 승인 필요
- Budget Cloud Executor: 반복량과 비용 문제가 확인된 뒤 Benchmark
- Ollama Cloud: 비교 후보일 뿐 초기 필수 아님
- Windows Local LLM: `LOCAL-NEED` Gate 승인 후

세부 Sequencing은 `docs/44-model-sequencing-and-cost-governance.md`를 따른다.

## 비용과 사용량

가격과 제품 정책은 변경될 수 있으므로 실행 당일 공식 문서를 확인한다.

기본 정책:

- 자동 충전 Off
- 자동 유료 승격 Off
- 자동 Local↔Cloud Fallback Off
- 유료 API Key 등록은 별도 승인
- 작업당 실제 Usage, 재시도, 사람 개입, 완료 여부 기록
- 구독형 제품과 PAYG API 비용을 분리해 기록

단가보다 완료 작업당 비용을 우선한다.

```text
완료 작업당 비용
=
직접 비용
+ 재시도
+ 사람 개입
+ 실패 복구
+ 운영·유지보수 시간
```

## 로컬 구축 전 측정

최소 2주 또는 작업 30건 동안 다음을 기록한다.

- Cloud 사용량 제한이 실제로 발생했는지
- 반복 작업이 얼마나 많은지
- Wiki 재사용으로 얼마나 절약했는지
- 외부 전송 불가 작업이 몇 건인지
- Local LLM으로 대체 가능한 정형 작업 비율

실제 문제가 확인되지 않으면 추가 Cloud Provider와 Local LLM을 설치하지 않는다.

## 데이터

- PUBLIC: 승인된 Cloud 사용 가능
- PRIVATE: 최소 Context만 전달하고 전송 범위 기록
- CONFIDENTIAL: 승인된 로컬 환경에서만 처리
