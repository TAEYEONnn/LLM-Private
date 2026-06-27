# Local Configuration

이 디렉터리는 장비별 실제 연결값과 로컬 경로를 저장한다.

Git에 커밋하지 않는 파일 예시:

- `windows.local.yml`
- `macos.local.yml`
- `.env.local`

## macOS 예시

```yaml
windows:
  host: 192.168.0.15
  llm_endpoint: http://192.168.0.15:11434/v1
  open_webui_url: http://192.168.0.15:3000
  comfyui_url: http://192.168.0.15:8188
```

## Windows 예시

```yaml
network:
  private_ipv4: 192.168.0.15
  allowed_mac_ipv4: 192.168.0.20
```

실제 값은 자동 탐지 후 Health Check가 통과한 경우에만 확정한다. 예제 주소를 그대로 사용하지 않는다.
