---
layout: post
title:  "트랜잭션의 격리수준"
date:   2022-08-04 20:33:00 +0900
categories: Spring
---

### 트랜잭션
트랜잭션은 데이터의 정합성을 보장받기 위한 기능입니다. 이러한 트랜잭션은 ACID를 보장받아야합니다. <br>
- 원자성(Atomicity) : 트랜잭션 내에서 실행한 작업은 모두 성공하거나 모두 실패해야합니다.
- 일관성(Consistency) : 모든 트랜잭션은 일관성있는 데이터베이스 상태를 유지해야합니다.
- 격리성(Isolation) : 동시에 실행되는 트랜잭션이 서로에게 영향을 미치지 않도록 격리해야합니다.
- 지속성(Durability) : 트랜잭션을 성공적으로 끝내면 그 결과는 항상 기록되어야합니다.

<br>
여기서 트랜잭션은 기본적으로 원자성, 일관성, 지속성을 보장하지만, 격리성은 완벽히 보장해주지 않습니다. <br>
그래서 ANSI 표준은 트랜잭션 격리 수준을 4단계로 나누어 정의합니다.

### 격리수준
- READ UNCOMMITTED(1) 
    - DIRTY READ, NON-REPEATABLE READ, PHANTOM READ 문제가 발생
- READ COMMITTED(2)
    - NON-REPEATABLE READ, PHANTOM READ 문제가 발생
- REPEATABLE READ(3)
    - PHANTOM READ 문제가 발생하지만 MYSQL 5.5 부터 InnoDB 엔진에서부터는 발생하지 않음 
- SERIALIZABLE(4)

##### DIRTY READ : 트랜잭션에서 처리한 작업(COMMIT/ROLLBACK)이 완료되지 않았음에도 불구하고, 다른 트랜잭션에서 볼 수 있게 되는 현상을 더티리드(Dirty Read)라고 합니다.

##### NON-REPEATABLE READ : 하나의 트랜잭션 내에서 동일한 SELECT 쿼리를 실행했을 때, 항상 같은 결과를 보장해야 한다는 REPEATABLE READ 정합성에 어긋나는 것을 말한다.

##### PHANTOM READ : SELECT ... FOR UPDATE 쿼리와 같은 쓰기 잠금을 거는 경우, 다른 트랜잭션에서 수행한 변경 작업에 의해 레코드가 보였다가 안 보였다가 하는 현상을 말한다.

<br>
위에서 부터 순서대로 격리레벨이 가장 낮고 SERIALIZABLE은 격리레벨이 가장 높습니다.<br>
격리레벨이 높을수록 동시성을 보장받지만 무조건 높다고 좋지 않습니다. 가장 높은 SERIALIZABLE은<br>
한 트랜잭션이 읽고 쓰는 레코드를 다른 트랜잭션에서 절대 접근이 불가능해 성능이 떨어집니다.<br>























