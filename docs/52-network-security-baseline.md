# 52. Network Security Baseline

Private LAN만 사용하고 Public 프로필과 인터넷 포트포워딩을 차단한다. Mac 단일 사설 IP 허용을 우선한다.

포트: Ollama 11434, LM Studio 1234, Open WebUI 3000, ComfyUI 8188.

TCP와 HTTP Health Check, 방화벽, VPN/VLAN, localhost 바인딩을 검증한다.

Open WebUI 로그인은 Ollama/llama.cpp API 자체 인증을 대신하지 않는다.
