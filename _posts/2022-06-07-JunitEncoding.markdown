---
layout: post
title:  "Spring Test MockMvc 한글 깨짐 처리"
date:   2022-06-07 00:03:00 +0900
categories: SpringBoot
---

### 들어가며
이번에 Junit으로 MockMvc를 사용하여 테스트 코드를 작성하던 중 한글이 깨지는 오류가 발생했다. <br>
알고보니 SpringBoot의 경우 2.2 버전 이후에는 기본 인코딩 방식으로 UTF-8을 지원하지 않아 발생하는 오류였다.<br>
해결방법을 아래와 같다.<br>

### 해결방법
인코딩 방식을 해결하는 방법은 2가지가 있다. <br>
1. @Before 의 setup 함수에서 UTF-8 로 인코딩을 해주는 필터를 추가해주면된다.
```java
@Before
public void setup() {
    this.mockMvc = MockMvcBuilders
            .webAppContextSetup(this.context)
            .addFilters(new CharacterEncodingFilter("UTF-8", true))
            .alwaysDo(print())
            .build();
}
```
2. application.yml 파일에 아래의 설정을 추가한다.
```
server:
  servlet:
    encoding:
      force-response: true
```

### 마치며
항상 테스트 코드 작성을 고민하고 생활화 하는 개발자가 되고싶다.


