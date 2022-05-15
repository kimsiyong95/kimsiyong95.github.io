---
layout: post
title:  "Filter, Interceptor, AOP 차이점"
date:   2022-05-15 22:35:00 +0900
categories: Spring
---

### 들어가며
회사에서 지인들과 얘기를 나누는 도중 Filter와 Interceptor, AOP에 대한 얘기가 나왔다. <br>
나는 당연히 셋 다 Spring Context 영역에서 실행되는 줄 알았는데 아니였고, 누가 먼저 실행되는지에 대해 잘 모르겠더라... 정말 무안했다 <br>
그래서 내가 기초가 부족하다는걸 깨닫고 기초를 튼튼히 하기위해 글을 남겨본다. <br>
스프링 프레임워크에서 사용하는 Filter, Interceptor, AOP는 요청 전이나, 요청 후에 추가적인 기능이 필요할 때 사용되는 기능들이다.<br>
그렇다면 세 가지에 대해서 알아보자.

### 전체 흐름
![alt text](/public/img/filter.png)


### Filter
Filter 같은 경우는 Web.xml 이나 Web Context에 설정한다. 최근 egovframe은 EgovWebApplicationInitializer에 등록하면 될 것 같다. <br>

일반적으로 인코딩 변환 처리, XSS방어 등의 요청에 대한 처리로 사용된다. <br>

[ Filter 실행메서드 ]

ㆍinit() - 필터 초기화

ㆍdoFilter() - 전/후 처리

ㆍdestroy() - 필터 종료

### Interceptor
Interceptor 같은 경우는 Spring Context에 설정한다. 즉 내부에서 작동하기 때문에 Spring에 등록된 빈 객체에 접근이 가능하다. <br>

일반적으로는 로그인 체크, 권한체크, 프로그램 실행시간 계산작업 로그확인 처리로 사용된다. <br>

[ Interceptor 실행메서드 ]

ㆍpreHandler() - 컨트롤러 메서드 실행되기 전

ㆍpostHanler() - 컨트롤러 메서드 실행직 후 view 렌더링 전

ㆍafterCompletion() - view가 렌더링 되고 난 후

### AOP
AOP 같은 경우도 역시 Spring context 영역에 포함되어 있다. AOP는 관점 지향 프로그래밍이라고 말한다.<br>
Interceptor와 Filter같은 경우는 url 로 기능을 걸어놓지만 aop는 메서드명, 어노테이션 등 다양하게 걸어 놓을 수 있다.<br>
일반적으로 로깅, 트랜잭션, 에러 처리에 사용된다.<br>

[ AOP의 PointCut ]

ㆍ@Before: 대상 메소드의 수행 전

ㆍ@After: 대상 메소드의 수행 후

ㆍ@After-returning: 대상 메소드의 정상적인 수행 후

ㆍ@After-throwing: 예외발생 후

ㆍ@Around: 대상 메소드의 수행 전, 후

### 마치며
Web개발을 진행하면서 공통기능을 개발해야 할 시점이 올 때 상황에 맞는 기능을 선택한다면 좀 더 효율적인 개발을 진행할 수 있을 것이다. <br>
명확한 차이점을 구분하고, 퀄리티 높은 개발을 진행해보자. 어제보다 오늘 더 성장하는 개발자가 되고싶다.

### 참고
<a href="https://goddaehee.tistory.com/154?category=173020">https://goddaehee.tistory.com/154?category=173020</a>