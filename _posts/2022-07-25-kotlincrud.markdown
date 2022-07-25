---
layout: post
title:  "코틀린으로 CRUD 유의점"
date:   2022-07-25 14:10:00 +0900
categories: Kotlin
---

### 코틀린으로 CRUD 유의점

##### Spring Data Jpa
코틀린에서는 findById 등 리턴 타입이 Optional인 메소드를 사용 할 때 Nullable 타입으로 변환해주지 않는다. <br>
그래서 Optional를 처리하기 위해서는 OrNull을 붙인 확장함수를 사용하면 해결이 가능하다 ex) findByIdOrNull() <br>
그 후에 엘비스 연산자를 사용하여 처리한다.

##### DTO
코틀린은 getter setter를 사용 할 때 따로 만들어주지 않아도 알아서 만들어준다. 하지만 private 접근제어자를 사용하면 <br>
getter setter를 만들어주지 않아 값을 읽거나 쓸 수가 없다.

##### Entity
변수를 선언 할 때 var, val이 있다. 가급적 var 보다는 val를 사용하고 update가 일어날수있는 변수는 var 로 선언하자












