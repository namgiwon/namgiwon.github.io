---
layout: post
title: s3와 cloudfront 연결
tags:
  - cloudfront
  - s3
---

- s3 버킷을 생성한 후 cloudfront와 연결했음.
- 그런데 s3에 업로드한 오브젝트에 접근이 불가능했음. 물론 Block all public access 상태였지만, 이전에 설정할 땐 이렇게 해도 직접 s3에 접근하는 것은 가능했지만, cloudfront를 통해 접근하는 데는 문제가 없었음. 그런데 안됨.
- 기존에 운영 중인 aws 계정과 새로 만든 aws 계정의 다른점을 살펴보니 iam 설정 여부였음.
- 그래서 iam 그룹과 사용자를 만든 후, s3fullaccess 권한을 부여하니 브라우저를 통해 cloudfront에 있는 오브젝트에 접근이 가능했음.
