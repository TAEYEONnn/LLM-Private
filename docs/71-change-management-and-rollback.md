# 71. Change Management and Rollback

모델, 양자화, 템플릿, 런타임, 하네스, Parser, 컨텍스트, 드라이버, MCP 변경은 모두 회귀 대상이다.

Candidate 등록 → 현재 설정 백업 → 회귀 세트 → Hard Fail → 승인 → Canary → Alias 변경 → 기존 Stack 보존 → 문제 시 롤백.

회귀 세트: T00, T01, T03, T05, T06, T10, T11.
