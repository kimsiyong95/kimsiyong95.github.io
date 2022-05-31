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
em.detach(member); 
```

##### 삭제
엔티티를 영속성 컨텍스트와 데이터베이스에서 삭제한다.

```java
Member member = new Member();
member.setId("user1");
member.setUsername("김시용");

EntityManager em = entityManagerFactory.createEntityManager();
em.remove(member);
```



