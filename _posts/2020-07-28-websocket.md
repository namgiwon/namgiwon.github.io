---
layout: post
title: spring boot websocet handshake error
tags:
  - websocket
---

- 서버 클라이언트 간에 웹소켓 통신은 정상적으로 됨.

- 하지만, 브라우저 디버그 창에 upgrade error 라는 문구가 담긴 에러가 뜸.

- nginx에 아래 설정을 추가해 주니 해결 됨

```
# WebSocketSecure SSL Endpoint
#
# The proxy is also an SSL endpoint for WSS and HTTPS connections.
# So the clients can use wss:// connections
# (e.g. from pages served via HTTPS) which work better with broken
# proxy servers, etc.

server {
    listen 443;

    # host name to respond to
    server_name ws.example.com;

    # your SSL configuration
    ssl on;
    ssl_certificate /etc/ssl/localcerts/ws.example.com.bundle.crt;
    ssl_certificate_key /etc/ssl/localcerts/ws.example.com.key;

    location / {
        # switch off logging
        access_log off;

        # redirect all HTTP traffic to localhost:8080
        proxy_pass http://localhost:8080;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

        # WebSocket support (nginx 1.4)
        # 추가된 설정
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }
}
```
