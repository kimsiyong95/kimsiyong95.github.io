---
layout: post
title:  "영속성 컨텍스트란?"
date:   2022-05-30 23:23:29 +0900
categories: JPA
---

### 들어가며
JPA를 공부하면서 영속성이라는 말을 들어봤는데 실제 어떤 구조로 돌아가고 1차 캐시에 어떤식으로 저장 되는지<br>
너무 궁금해서 내용을 정리해봤다. 이번 기회에 영속성 컨텍스트에 대한 개념을 완벽히 잡고 갈 것이다.

### 영속성 컨텍스트란?
<img src="/public/img/permanenceContext1.png"  width="600" height="400"/><br>
영속성 컨텍스트는 엔티티를 영구히 저장하는 환경이라는 뜻이고, 더 나아가 애플리케이션과 데이터 베이스 사이에서<br>
객체를 1차 캐시에 보관하는 논리적인 개념이다.

### 영속성 생명주기
<img src="/public/img/permanenceContext2.png"  width="600" height="400"/><br>
##### 비영속
영속성 컨텍스트와 전혀 관계가 없는 상태 : 한 마디로 객체에 값만 넣어논 상태
```java
Member member = new Member();
member.setId("user1");
member.setUsername("김시용");
```

##### 영속
영속성 컨텍스트에 저장된 상태
```java
Member member = new Member();
member.setId("user1");
member.setUsername("김시용");

EntityManager em = entityManagerFactory.createEntityManager();
em.persist(member); // db에는 저장되지 않고  1차 캐시 (영속성) 안으로 맵핑 시켜놓는다. 
                    // 실제 데이터를 저장하기 위해서 트랜잭션 커밋, flush 호출 시 쿼리 호출
```

##### 준영속
영속성 컨텍스트에 저장되었다가 분리된 상태
```java
Member member = new Member();
member.setId("user1");
member.setUsername("김시용");

EntityManager em = entityManagerFactory.createEntityManager();
em.persist(member);
em.detach(member); 
```

##### 삭제
엔티티를 영속성 컨텍스트와 데이터베이스에서 삭제한다.

```java
em.remove(member);
```

### 영속성 컨텍스트의 특징
영속성 컨텍스트는 엔티티를 식별자 값으로 구분한다. 따라서 영속 상태는 식별자 값이 반드시 있어야 한다. <br>
그리고 영속성 컨텍스트의 장점은 다음과 같다.
1. 동일성을 보장한다. (같은 id로 2번 조회한 각 각의 객체는 서로 동일하다.)
2. 1차 캐시 지원 (같은 id로 2번 조회 하면 최초 db접근 -> 1차 캐시 접근)
3. 쓰기지연을 지원한다. (트랜잭션 커밋이나 플러쉬가 되기 전 쓰기지연 저장소에 쿼리를 모아놓고 종료되었을때 한 번에 날라간다.)
4. 변경감지를 지원한다. (트랜잭션 커밋이나 플러쉬가 되기 전 1차캐시에 올라간 데이터가 값이 바뀌면 변경감지를 하여 update 쿼리를 만든다.)


### 마치며
이번에는 영속성 컨텍스트에 대한 구조와 특징을 알아보았고, 다음에는 JPQL에 대해 리뷰 해보려고한다.<br>
앞으로도 더 열심히 공부해서 보 다 성장하는 개발자가 되고싶다.