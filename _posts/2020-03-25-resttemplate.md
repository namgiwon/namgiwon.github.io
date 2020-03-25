---
layout: post
title: 스프링 부트 레스트 템플릿 예제
tags:
  - resttemplate
  - spring boot
---

* 카카오 좌표 API를 쓸 일이 있어서 resttemplate을 사용했는데, 결과 값이 계속 비어 있어서 좀 헤맸다.

* 아래 코드 사용했음.

```java
MultiValueMap<String, String> parameters = new LinkedMultiValueMap<>();
parameters.add("query", store.getAddress());
HttpHeaders headers = new HttpHeaders();
headers.add("Authorization", "KakaoAK " + KAKAO_REST_KEY);
HttpEntity<MultiValueMap<String, String>> request = new HttpEntity<>(parameters, headers);
String result = restTemplate.postForObject(new URI(KAKAO_GEOAPI_URL), request, String.class);
JSONObject geoInto = new JSONObject(result);
```
