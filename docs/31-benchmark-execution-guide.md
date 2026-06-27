# 31. Benchmark Execution Guide

## 사전
1. 저장소 Clean 확인
2. 기준 Commit 기록
3. Worktree 생성
4. Stack Manifest 저장
5. Runtime Health Check
6. Provider/Endpoint/Model 출력
7. 데이터 등급·도구 확인
8. 자동 폴백·자동 충전 비활성화 확인

## 실행 중
요청, 원본 응답, Tool Call/Result, stdout/stderr, Exit Code, RAM/VRAM, TTFT/TPS, Retry, 사람 개입, 실제 Provider/Model을 기록한다.

## 실행 후
테스트, Git diff, 원본 변경 검사, 경로 밖 변경 검사, 비밀 노출 검사, 실패 코드, 점수, 승인 상태를 기록한다.

벤치마크 중 JSON Repair는 금지한다.
