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

- 인증서를 강제 갱신.

```bash
sudo certbot renew --force-renewal --no-self-upgrade
```

- --no-self-upgrade 옵션이 없으면 인증서 갱신 스크립트를 수행하면서 업데이트를 진행하는데, 이 때 사용자 입력을 받아야 되기 때문에 크론탭에 등록해서 사용하려면 이 옵션을 추가해야 함.

- standalone 방식으로 인증서를 설치했을 경우 인증서를 갱신할 때 웹서버를 잠시 종료해야 함.

- 인증서 삭제

```bash
sudo certbot delete --cert-name {인증서 이름}
```
