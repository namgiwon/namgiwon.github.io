---
layout: post
title: 스프링 부트 projection
tags:
  - projection
---

- 한 테이블에서 컬럼 두 세개만 필요한 객체를 생성해야 함.

- projection을 이용해서 한 테이블에서 특정 컬럼만 가져올 수 있는 기능을 구현할 수 있는 것을 확인함.

- 근데 데이터를 제대로 가져오지 않음.

- 필요한 컬럼이 coupon_id, name, store_name 이었는데 name을 제외한 두 개의 컬럼을 읽으려 하면 에러가 발생.

- projection 기능을 사용하기 위해서는 interface를 작성해야 되는데, 첨엔 아래와 같이 작성함.

```java
interface ICouponSummary {
    long getCouponId();

    String getMame();

    String getStoreName();
}
```

- name만 제대로 가져오는 것이 이상해서 아래와 같은 코드로 바꿔 봄.

```java
interface ICouponSummary {
    long getCoupon_id();

    String getMame();

    String getStore_name();
}
```

- 잘 동작함. 메서드 명명 규칙을 몰라서 생긴 일이었음.
