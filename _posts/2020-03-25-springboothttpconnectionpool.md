---
layout: post
title: 스프링 부트 http connection pool 사용법
tags:
  - spring boot
  - http
---

1. 빈 등록

```java
package io.floody.tr.config;

import org.apache.http.client.HttpClient;
import org.apache.http.impl.client.HttpClientBuilder;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.http.client.HttpComponentsClientHttpRequestFactory;
import org.springframework.web.client.RestTemplate;

@Configuration
public class HttpConnectionPoolConfig {

    @Bean(name="restTemplateClient")
    public RestTemplate restClient() {
        HttpComponentsClientHttpRequestFactory httpRequestFactory = new HttpComponentsClientHttpRequestFactory();
        httpRequestFactory.setConnectTimeout(10000);
        httpRequestFactory.setReadTimeout(15000);
        httpRequestFactory.setConnectionRequestTimeout(10000);
        HttpClient httpClient = HttpClientBuilder.create()
                .setMaxConnTotal(150)
                .setMaxConnPerRoute(50)
                .build();
        httpRequestFactory.setHttpClient(httpClient);

        return new RestTemplate(httpRequestFactory);
    }
}
```

2. 사용

```java
@RestController
@RequiredArgsConstructor
public class CustomerController {

    @Qualifier("restTemplateClient")
    private final RestTemplate restTemplate;
```
