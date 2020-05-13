---
layout: post
title: letsencrypt 인증서 등록
tags:
  - letsencrypt
  - 인증서
---

## amazon linux 1에서는 certbot 사용하기가 힘듦

- linux1 에서는 인증서 설치까지는 되는데, 인증서 갱신을 수행하면 에러가 발생한다.

- 이런 저런 시도를 해 봤으나 답이 보이지 않아 linux 2 인스턴스를 새로 생성했다.

- 아래 명령어를 통해 certbot을 설치하자.

```bash
curl -O http://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

sudo yum install epel-release-latest-7.noarch.rpm

sudo yum install certbot

sudo certbot
```

1. standalone 방식으로 인증서 생성. 도메인 인증을 위해 80번 포트를 이용하는 것 같으니 nginx는 잠시 내려놓자.

```bash
sudo certbot certonly --standalone -d domain.com
```

2. webroot 방식으로 인증서 생성. 웹서버 재시작 필요 없음

```bash
sudo certbot certonly --webroot -w /usr/share/nginx/html -d domain
```

- 위 명령어 수행하면 아래와 같은 결과가 출력됨

```bash
IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/www.thankyoureward-rest-api-server.com/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/www.thankyoureward-rest-api-server.com/privkey.pem
   Your cert will expire on 2020-08-03. To obtain a new or tweaked
   version of this certificate in the future, simply run
   letsencrypt-auto again. To non-interactively renew *all* of your
   certificates, run "letsencrypt-auto renew"
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le
```

- 인증서 생성 후 아래 설정을 nginx.conf 파일에 추가

```bash
server {
    listen       443 default_server ssl;
    server_name  www.thankyoureward-rest-api-server.com;

    ssl_certificate      /etc/letsencrypt/live/domain/fullchain.pem;
    ssl_certificate_key  /etc/letsencrypt/live/domain/privkey.pem;

    ssl_session_cache    shared:SSL:1m;
    ssl_session_timeout  5m;

    location / {
    }
}
```

- nginx 재시작
