---
layout: post
title: 스프링 부트, Mysql, hibernate 좌표 데이터 연결
tags:
  - geometry
  - hibernate
---

* application.yml에 generate-ddl이 true로 되어 있으면 좌표 컬럼을 tinyblob으로 생성함. 따라서 컬럼 타입을 geometry로 하기 위해서는 테이블을 직접 생성해야 함

* build.gradle에 아래 의존 관계 추가

```java
compile group: 'org.hibernate', name: 'hibernate-spatial', version: '5.4.12.Final'
compile group: 'com.vividsolutions', name: 'jts', version: '1.13'
```

```java
import com.vividsolutions.jts.geom.Point;
private Point geoLocation;
```
