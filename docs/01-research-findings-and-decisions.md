# 01. Research Findings and Decisions

## 발견
- Ollama Cloud는 절대 한도가 불명확해 지속적 에이전트 사용 예측이 어렵다.
- Ollama는 편리하지만 템플릿·저장 형식·엔진 추상화 문제가 있을 수 있다.
- llama.cpp는 CPU·GPU 혼합 오프로딩과 세부 제어가 강점이다.
- 로컬 에이전트는 채팅보다 도구 선택, JSON, 파싱, 결과 활용, 멀티턴 상태 유지에서 실패하기 쉽다.
- 200K 이상 컨텍스트보다 8K~16K 안정성이 중요하다.
- 로컬과 클라우드는 대체 관계보다 역할 분담 관계다.

## 결정
1. Ollama 고정 해제
2. API 규격 통일
3. Ollama와 llama.cpp A/B 테스트
4. LM Studio는 진단 도구
5. OpenCode 기본 하네스
6. Native Codex 기준선
7. Cloud PAYG 예산 상한
8. Worktree 쓰기 기본
9. TPS보다 종단 성공률 우선
10. Deep 모델은 Experimental
