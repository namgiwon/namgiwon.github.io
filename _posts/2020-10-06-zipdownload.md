---
layout: post
title: spring boot + vuejs zip 파일 다운로드
tags:
  - download
---

- 자바스크립트 코드

```javascript
api.generatorGiftCodes(data).then(function (res) {
  var fileURL = window.URL.createObjectURL(new Blob([res.data]));

  var fileLink = document.createElement("a");

  fileLink.href = fileURL;

  fileLink.setAttribute("download", "result.zip");

  document.body.appendChild(fileLink);

  fileLink.click();
});
```

- 서버 코드

```java
File target = FileUtil.makeGiftCode(normalGifts, kindGifts, recoveryGifts, deliveryGifts);
        Resource rs = null;
        HttpHeaders header = new HttpHeaders();
        if (target.exists()) {
            try {
                String mimeType = Files.probeContentType(Paths.get(target.getAbsolutePath()));

                if (mimeType == null) {
                    mimeType = "octet-stream";
                }
                rs = new UrlResource(target.toURI());
                header.add(HttpHeaders.CONTENT_DISPOSITION, "attachment; filename=\"" + FileUtil.encodeFileName("result.zip") + "\"");
                header.setCacheControl("no-cache");
                header.setContentType(MediaType.parseMediaType(mimeType));
            } catch (Exception e) {
                e.printStackTrace();
            }
        }

        return new ResponseEntity<>(rs, header, HttpStatus.OK);
```
