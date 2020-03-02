---
layout: post
title: ModelAttribute에 Multipart 적용 시 버그
tags:
  - spring boot
  - ModelAttribute
  - Multipart
---

## Form데이터 전송할 때 파일 데이터 누락시 발생하는 에러

* 아래와 같이 컨트롤러에 메서드가 작성되어 있을 때, 클라이언트에서 Multipart에 파일을 담지 않고 전송이 되면 에러가 발생함

* 파일이 첨부되어 있지 않으면 DTO의 해당 파라미터가 null로 잡혀야 하는데 빈 스트링 ("")으로 전달되기 때문에 발생

```java
@PreAuthorize("hasRole('ADMIN')")
@PostMapping(value = "/api/v1/store")
public Response insert(@ModelAttribute("storeDto") StoreDto storeDto) throws Exception {
    return storeService.save(storeDto);
}
```

* 아래 코드를 해당 컨트롤러에 추가해 줘야 함

```java
@InitBinder
public void initBinder(WebDataBinder binder) {
    binder.registerCustomEditor(MultipartFile.class, "[파일 필드 이름]",new StringTrimmerEditor(true));
}
```
