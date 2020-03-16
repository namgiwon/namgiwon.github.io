---
layout: post
title: 스프링 부트 데이터 트랜잭션 매니저 등록
tags:
  - spring boot
  - dataSource
  - transaction
  - TransactionManager
---

## hiakri DB Pool 사용 전제

* 근데 히카리는 일본어로 빛 광자(光)를 훈독했을 때 나는 소리가 아닌가?

1. 메인 클래스에 @EnableTransactionManagement 어노테이션 선언

2. 아래 클래스를 작성

```java
package io.floody.tr.config;

import com.zaxxer.hikari.HikariDataSource;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.boot.jdbc.DataSourceBuilder;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Primary;
import org.springframework.jdbc.datasource.DataSourceTransactionManager;
import org.springframework.transaction.annotation.EnableTransactionManagement;

import javax.sql.DataSource;

@Configuration
@EnableTransactionManagement

public class DataSourceConfig {

    @Bean(name = "dataSource")
    @Primary
    @ConfigurationProperties("spring.datasource.hikari")
    public DataSource dataSource() {
        return DataSourceBuilder.create()
                .type(HikariDataSource.class)
                .build();
    }


    @Bean(name = "transactionManager")
    @ConfigurationProperties("spring.datasource.hikari")
    public DataSourceTransactionManager txManager() {
        return new DataSourceTransactionManager(dataSource());
    }
}

```

3. 설정 파일  application.yml을 아래와 같은 형식으로 변경

```bash
datasource:
    hikari:
      connection-timeout: 10000
      maximum-pool-size: 100
      jdbc-url: jdbc:mysql://localhost:3306/floody?characterEncoding=UTF-8&serverTimezone=Asia/Seoul
      username: root
      password: nam
```
