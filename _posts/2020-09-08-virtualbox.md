---
layout: post
title: 버추얼박스 네트워크 (브릿지에 어댑터) 설정 안 되는 현상
tags:
  - virtualbox
---

- 가상머신과 로컬 네트워크에 있는 다른 사람의 머신과 통신을 하기 위해 브릿지에 어댑터 네트워크를 적용하려 했으나 목록에 보이지 않음.

- 아래 방법을 사용하여 해결 함.

1. 제어판 / 네트워크 및 인터넷 > 네트워크 상태 및 작업 보기 > 연결 된 인터넷 클릭
2. 속성 > 설치 > 서비스 클릭
3. 디스크 있음 클릭 후 C:\Program Files\Oracle\VirtualBox\drivers\network\netlwf\VBoxNetLwf.inf 선택
