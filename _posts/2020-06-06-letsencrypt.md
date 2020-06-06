---
layout: post
title: letsencrypt 인증서 업데이트 실패
tags:
  - letsencrypt
---

- letsencrypt를 통해 인증서를 받고, certbot을 통해 매일 인증서를 업데이트하도록 설정해 놓음.

- 첫 한달간 문제가 발생하지 않았기 때문에 인증서 이슈는 종료된 걸로 판단함.

- 간만에 생각이 나서 인증서 업데이트 로그를 보니 에러가 발생하며 업데이트가 되지 않고 있었음. 에러 로그는 아래와 같음

```bash
IMPORTANT NOTES:
 - The following errors were reported by the server:

   Domain: server.com
   Type:   connection
   Detail: Fetching
   http://server.com/.well-known/acme-challenge/DnYvZ2yxlfMALl-I86mf_HbnyZBUDsHLPnb1hP5GUak:
   Connection refused

   To fix these errors, please make sure that your domain name was
   entered correctly and the DNS A/AAAA record(s) for that domain
   contain(s) the right IP address. Additionally, please check that
   your computer has a publicly routable IP address and that no
   firewalls are preventing the server from communicating with the
   client. If you're using the webroot plugin, you should also verify
   that you are serving files from the webroot path you provided.
```

- certbot에서 인증서 업데이트를 위해 nginx의 webroot에 특정한 파일을 만들고 커넥션 체크를 하는 것 같은데 (http:/server.com/.well-known/acme-challenge/DnYvZ2yxlfMALl-I86mf_HbnyZBUDsHLPnb1hP5GUak), 여기에 접속이 안되니 에러가 발생하는 것으로 파악.

- nginx의 설정을 조금 수정함.

```bash
server {
        listen       80; // 추가된 설정
        listen       443 default_server ssl;
        server_name  server.com;
        root         /usr/share/nginx/html;

        include /etc/nginx/conf.d/service-url.inc;

        ssl_certificate      /server.com/fullchain.pem;
        ssl_certificate_key  /server.com/privkey.pem;

        ssl_session_cache    shared:SSL:1m;
        ssl_session_timeout  5m;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        location / {
                proxy_pass $service_url;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header Host $http_host;
        }

        // 추가된 설정
        location ~ /\.well-known/acme-challenge {
                root /usr/share/nginx/html;
        }
}
```
