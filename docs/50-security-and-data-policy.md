# 50. Security and Data Policy

## 데이터
PUBLIC은 승인된 외부 API 허용. PRIVATE는 기본 로컬. CONFIDENTIAL은 로컬만 허용.

## 비밀정보
.env, API Key, OAuth Token, 비밀번호, 인증 파일, 쿠키는 Git·일반 로그·백업 ZIP에서 제외한다.

## 로그
원문 기본 비활성화, 벤치마크 Fixture만 예외. Key 마스킹. 일반 로그 7일, 보안 이벤트 30일, 벤치마크 장기 보존.

## 외부 부작용
이메일, 캘린더, 배포, 삭제, 외부 DB 쓰기, 결제는 항상 별도 승인한다.
