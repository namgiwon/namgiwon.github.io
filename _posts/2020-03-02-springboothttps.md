---
layout: post
title: 스프링 부트 https 설정
tags:
  - spring boot
  - https
  - ssl
---

* 키스토어 생성

```bash
keytool -genkey -alias (키스토어의 별칭) -keyalg RSA -keystore (생성할 키스토어의 파일명).jks
```

* 인증서 생성

```bash
keytool -export -alias (키스토어의 별칭) -keystore (키스토어의 파일명) -rfc -file (생성할 인증서 파일이름).cer
```

* 트러스트 스토어 생성

```bash
keytool -import -alias (Trust-Store의 별칭) -file (인증서 파일명) -keystore (생성할 Trust-Store 파일명).ts
```

* application.yml에 아래 코드 작성

```yaml
server:
  port: 8090 #Https port
  ssl:
    enabled: true
    key-store: /home/test/keystore.jks # 키스토어 경로.
    key-store-password: passwd  # 키스토어 암호.
    key-password: passwd # 키 암호.
    key-alias: mhlab # 키스토어 별칭.
    trust-store: /home/test/keystore.ts # 트러스트스토어 경로
    trust-store-password: passwd # 트러스트스토어 비밀번호
```
