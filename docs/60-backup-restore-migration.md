# 60. Backup, Restore and Migration

## 포함
config, scripts, docs, 레지스트리, Manifest, 벤치마크, Open WebUI DB, RAG 메타데이터, ComfyUI Workflow, Custom Node Manifest, 프로젝트 메모리.

## 제외
API Key, OAuth Token, 비밀번호, 인증 파일, 쿠키, 캐시, 원문 로그, 가상환경, node_modules, Docker Image, WSL 전체 디스크, 대형 모델 파일.

## 마이그레이션
설정·문서·Workflow는 복사하고 바이너리·가상환경·드라이버·토큰은 재설치한다.

목표: 대형 모델 다운로드를 제외하고 새 장비에서 1시간 이내 재구성.
