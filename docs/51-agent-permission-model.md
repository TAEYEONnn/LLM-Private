# 51. Agent Permission Model

## read-only
파일 읽기, 검색, Git diff, 문서 분석만 허용.

## write-worktree
Worktree 내부 수정, 테스트, 임시 파일 허용. 원본·프로젝트 외부·외부 부작용 금지.

## write-approved
실제 작업 트리 수정. 최근 30건 Hard Fail 0과 사용자 명시적 승인 필요.

## external-side-effect
이메일, 캘린더, 배포, 삭제 등. 실행 직전 별도 확인.
