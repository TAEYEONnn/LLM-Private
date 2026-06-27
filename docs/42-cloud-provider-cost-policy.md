# 42. Cloud Provider and Cost Policy

작성 기준일: 2026-06-27

## 역할

Cloud Provider는 Local LLM을 자동 대체하지 않는다. 작업 시작 전에 사용자가 명시적으로 선택한다.

- 기본 Planner·Vision·Reviewer: Claude Sonnet 4.6
- 예외 Planner·Reviewer: Claude Opus 4.8, 사용자 승인 필요
- Budget Cloud Executor: GLM 계열·OpenRouter·OpenCode Zen 후보, Benchmark 후 승인
- Native Codex: 기준선 비교
- Ollama Cloud Free: 비교용

세부 Sequencing은 `docs/44-model-sequencing-and-cost-governance.md`를 따른다.

## 비용

- 초기 Cloud PAYG 월 하드 캡: $10
- 자동 충전: Off
- 자동 폴백: Off
- 자동 Sonnet→Opus 승격: Off
- 공급자·모델·작업 예산을 실행 전에 고정
- 작업당 실제 Token, 비용, 재시도, 사람 개입, 완료 여부 기록

2026-06-27 Claude API 기준:

- Sonnet 4.6: 입력 $3 / 출력 $15 per MTok
- Opus 4.8: 입력 $5 / 출력 $25 per MTok

단가보다 완료 작업당 비용을 우선한다.

```text
완료 작업당 비용
= API 비용 + 재시도 + 사람 개입 + 실패 복구
```

2개월 연속 PAYG가 적절한 구독료보다 높을 때 구독형을 검토한다. 연간 선결제는 금지한다.

## 데이터

- PUBLIC: 승인된 Cloud 사용 가능
- PRIVATE: 최소 Context만 명시적 승인 후
- CONFIDENTIAL: Local only

공식 참고:

- https://platform.claude.com/docs/en/about-claude/models/overview
