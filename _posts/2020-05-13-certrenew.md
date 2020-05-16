---
layout: post
title: letsencrypt 인증서 갱신
tags:
  - 인증서
---

- 유효기간이 20일 미만으로 남은 인증서 갱신.

```bash
sudo certbot renew --no-self-upgrade
```

## 인증서를 강제 갱신.

```bash
sudo certbot renew --force-renewal --no-self-upgrade
```

- --no-self-upgrade 옵션이 없으면 인증서 갱신 스크립트를 수행하면서 업데이트를 진행하는데, 이 때 사용자 입력을 받아야 되기 때문에 크론탭에 등록해서 사용하려면 이 옵션을 추가해야 함.

- standalone 방식으로 인증서를 설치했을 경우 인증서를 갱신할 때 웹서버를 잠시 종료해야 함.

## 인증서 삭제

```bash
sudo certbot delete --cert-name {인증서 이름}
```

## 크론탭 등록

- 아래 코드를 작성하고 파일을 생성하자.

```bash
#!/bin/bash
sudo certbot renew --force-renewal --no-self-upgrade
sudo systemctl reload nginx
echo "Renew cert date : $(date)"  >> /home/ec2-user/data/renew_cert.log
```

- sudo vi /etc/crontab 명령어를 실행하여 아래 항목을 추가한다.

```bash
# 매일 새벽 다섯 시, 오후 다섯 시에 인증서 갱신
15      5,17    *       *       *       root /home/ec2-user/data/renew_cert.sh
```

- 크론탭 서비스 재시작

```bash
sudo systemctl restart crond
```
