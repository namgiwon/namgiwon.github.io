---
layout: post
title: 카카오톡 키 해쉬 등록 시 삽질 사항
tags:
  - spring boot
  - https
  - ssl
---

* openssl을 먼저 설치해야 함. 설치하지 않으면 키 해쉬를 얻을 수 없음. 한글이 깨짐.
* 이 것 때문에 한 시간 삽질했다. (파이프로 연결된 명령어 생략하면 안 됨)

```bash
keytool -exportcert -alias androiddebugkey -keystore C:\Users\namgiwon\.android\debug.keystore -storepass android -keypass android | openssl sha1 -binary | openssl base64
```
