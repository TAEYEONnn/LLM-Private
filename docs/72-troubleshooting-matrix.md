# 72. Troubleshooting Matrix

| 증상 | 우선 확인 | 가능 원인 |
|---|---|---|
| 느림 | Thinking, Context, Offload | 긴 추론·KV Cache |
| Tool Call 없음 | Template, Parser | 포맷 불일치 |
| JSON 깨짐 | 원본 응답, Schema | 모델 형식 실패 |
| 파일 못 찾음 | Workdir, MCP Scope | 경로 오류 |
| 이전 모델 사용 | Session Pin | 세션 캐시 |
| Cloud 전환 | Fallback 로그 | 자동 폴백 |
| VRAM 미반환 | Runtime Process | 언로드 실패 |
| Mac 연결 실패 | TCP/HTTP | 방화벽·Public 프로필 |
| RAG 오답 | 출처·인덱스 | 오래된 인덱스 |
| 테스트 반복 | Tool 로그 | Loop·결과 활용 실패 |
