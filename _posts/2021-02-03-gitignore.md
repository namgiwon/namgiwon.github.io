---
layout: post
title: .gitignore에 원래 있던 폴더 및 파일 추가 했을 때 적용 방법
tags:
  - gitignore
---

- 프로젝트를 생성할 때 .gitignore에 포함되지 않았던 폴더나 파일을 .gitignore에 추가하면 바로 적용이 되지 않음.

- 폴더나 파일의 캐시를 지운 후 .gitignore를 push해야 정상적으로 적용 됨.

```bash
git rm --cached src/config/ -r
```
