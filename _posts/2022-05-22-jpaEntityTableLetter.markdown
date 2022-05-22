---
layout: post
title:  "JPA Entity Table 대문자 인식 못하는 경우"
date:   2022-05-22 22:55 +0900
categories: JPA
---

### 들어가며
JPA를 통해서 Entity Class에 @Table (name = "T_REGIST_M01")로 테이블을 맵핑 시켰다.<br>
하지만 이상하게 log에 찍히는 내용을 보니 t_regist_m01 로 소문자로 인식하는것이 아닌가?? <br>
이와 같은 문제가 일어났을때 해결방법을 소개해본다.

```
@Data
@Entity
@Table(name = "T_REGIST_M01")
public class RegistEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "REGIST_ID", nullable = false)
    private Long registId;
    
    '''
    '''
    '''
```

### 문제원인
Spring Boot의 DB Physical Naming Strategy가 org.springframework.boot.orm.jpa.SpringNamingStrategy 으로<br>
설정되어있었다. SpringNamingStrategy에서는 Camel Case 대문자는 밑줄로 대체하고, 테이블은 소문자로 인식한다.<br>
자 그럼 문제의 원인을 파악했고 해결을 해보자.

### 문제해결
기존 Spring boot의 Physical Naming Strategy를 hibernate의 Physical Naming Strategy 변경하면 된다. <br>
application.yml 에 설정값을 추가해주면 된다.

```
  jpa:
    hibernate:
      naming:
        physical-strategy: org.hibernate.boot.model.naming.PhysicalNamingStrategyStandardImpl
```

### 마치며
더욱 더 어떠한 문제가 생겼을때 해결방법만을 생각하는것이 아닌 어떠한 원인 때문에 그런것인지 좀 더 깊게<br>
알아가며 문제를 해결하는 개발자가 될 것이다. 항상 성장하는 개발자가 되고싶다.
 




