---
layout: post
title:  "Junit4, Junit5 차이"
date:   2022-06-16 22:17:00 +0900
categories: SpringBoot
---

### 들어가며
Junit이란 단위테스트를 지원하는 Testing 프레임워크다. Java에서는 Junit이라고 불리지만 <br>
이런 Testing 할 수 있는 프레임워크를 xUnit이라고 부른다. JUnit 4 버전에서는 단일 jar 만 <br>
사용되었었는데 JUnit 5 로 넘어오면서 여러개의 jar 로 분리되었다고 한다. 그럼 제목에서 작성한대로 <br>
Junit4, Junit5의 차이점을 알아보자.

### Junit4 vs Junit5

1. java 버전
 - Junit4 : java5 이상
 - Junit5 : java8 이상

2. Spring Boot 제공 버전
 - Junit4 : Spring Boot 2.1 이전까지 기본 제공
 - Junit5 : Spring Boot 2.2 이후부터 기본 제공

3. Annotations

| JUnit 5     | JUnit 4      | 특징   |
|-------------|--------------|------|
| @BeforeAll  | @BeforeClass | 클래스에 포함된 모든 테스트가 수행하기 전에 실행 |
| @AfterAll   | @AfterClass  | 클래스에 포함된 모든 테스트가 수행한 후에 실행 |
| @BeforeEach | @Before      | 클래스에 포함된 각각의 테스트가 수행하기 전에 실행 |
| @AfterEach  | @After       | 클래스에 포함된 각각의 테스트가 수행한 후에 실행 |
| @Disable    | @Ignore      | 테스트 클래스 또는 메소드 distable 처리| 
| @DisplayName|              | 테스트 클래스 또는 메소드의 사용자 정의 이름을 표시 |


### 마무리하며(주의점)
JUnit5에서는 @SpringBootTest 를 할때, DI를 @Autowired를 해야함.<br>
이유는 Junit5부터는 DI를 스스로 지원하기 때문에 @RequiredArgsContrucotr 등<br>
lombok 어노테이션을 이용하거나, 생성자로 주입하면 에러 발생함.

 










