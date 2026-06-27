# 40. Model and Provider Registry

## 원칙
레지스트리는 실행 라우터가 아니라 구성의 단일 원본이다. 클라이언트 설정은 생성 스크립트로 만든다. 모델 정체성은 업스트림 저장소, Revision, 파일명, SHA256으로 관리한다.

## 모델 필드
alias, upstream_repository, upstream_revision, canonical_filename, canonical_sha256, architecture, total_parameters, active_parameters, quantization, chat_template_source, advertised/tested/approved_context_length, tool_calling, thinking, license, status.

## 공급자 필드
provider, endpoint, protocol, billing_mode, 가격, monthly_budget, auto_top_up, fallback_allowed, retention, training_allowed, ZDR, region, allowed_data_classes.

## 상태
candidate, approved-chat, approved-read-only, approved-write-worktree, approved-write, archived, rejected.
