---
layout: post
title: 리눅스 백그라운드 프로세스 PID
tags:
  - pid
---

- pid 얻기

```bash
ps aux | grep thankyoureward | grep -v grep | awk '{print $2}'
```

- pid 획득 후 kill

```bash
kill -9 `ps aux | grep thankyoureward | grep -v grep | awk '{print $2}'`
```
