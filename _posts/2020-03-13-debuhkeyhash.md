---
layout: post
title: 카카오톡 키 해쉬 등록 시 삽질 사항
tags:
  - android
  - kakaotalk
---

- openssl을 먼저 설치해야 함. 설치하지 않으면 키 해쉬를 얻을 수 없음.
- 이 것 때문에 한 시간 삽질했다. (파이프로 연결된 명령어를 생략해서 생긴 결과...)
- 공식 홈페이지 설명대로 아래 명령어를 다 실행해야 함.

```bash
keytool -exportcert -alias androiddebugkey -keystore C:\Users\namgiwon\.android\debug.keystore -storepass android -keypass android | openssl sha1 -binary | openssl base64
```
