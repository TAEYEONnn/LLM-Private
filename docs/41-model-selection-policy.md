# 41. Model Selection Policy

## 순서
실제 작업 정의 → Fast 후보 → 동일 런타임 비교 → 승인 검증 → Fast 실패 과제만 Medium → 필요 시 Deep Experimental.

## 금지
최신 모델 자동 설치, 가장 큰 모델 자동 선택, 최대 컨텍스트 기본값, latest 태그, 한 번 성공으로 승인, 크기만으로 Tool Calling 추정.

## 기본값
Q4_K_M 또는 Q5 우선, 3비트 Experimental, 8K, Thinking Off, Temperature 최소, 한 번에 모델 하나.
