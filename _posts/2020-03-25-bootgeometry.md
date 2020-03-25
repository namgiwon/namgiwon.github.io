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

* application.yml에 아래 dialect 적용

```java
hibernate.dialect: org.hibernate.spatial.dialect.mysql.MySQL56InnoDBSpatialDialect
```

* 아래 패키지 사용

```java
import com.vividsolutions.jts.geom.Point;
private Point geoLocation;
```

* 아래 유틸 클래스 작성

```java
package io.floody.tr.util;

import com.vividsolutions.jts.geom.Geometry;
import com.vividsolutions.jts.io.WKTReader;
import com.vividsolutions.jts.io.ParseException;

import java.util.Map;

public class GeometryUtil {

    private static WKTReader wktReader = new WKTReader();

    public static Geometry wktToGeometry(String wellKnownText) {
        Geometry geometry = null;
        try {
            geometry = wktReader.read(wellKnownText);
        } catch (ParseException e) {
            e.printStackTrace();
        }

        return geometry;
    }

    public static String makePointWKT(Map<String, String> point) {
        return  "POINT(" + point.get("latitude") + " " + point.get("longitude") + ")";
    }

}
```

* 클라이언트 클래스에서는 아래와 같이 사용

```java
String pointWKT = GeometryUtil.makePointWKT(geoInfo);
Geometry geoPoint = GeometryUtil.wktToGeometry(pointWKT);
storeRepository.savePoint(geoPoint.toString(), store.getId());
```

* 도메인 전체를 업데이트 하는 것이 아니라 geometry 타입 컬럼 하나만 업데이트 하는 방식으로 적용함

```java
public interface StoreRepository extends JpaRepository<Store, Long> {

    final String UPDATE_POINT_QUERY = "UPDATE TR_STORE_INFO SET GEO_LOCATION = ST_GEOMFROMTEXT(:point, 4326) where store_id = :id";

    @Modifying
    @Transactional
    @Query(nativeQuery = true, value = insertPointQuery)
    int savePoint(@Param("point") String point, @Param("id") Long id);
}
```
