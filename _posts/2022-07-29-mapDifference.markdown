---
layout: post
title:  "HashTable, HashMap, conCurrentHashMap 차이점"
date:   2022-07-29 11:00:00 +0900
categories: Java
---

### HashTable

<img src="/public/img/hashTable2.png"  width="600" height="300"/><br>

HashTable 데이터를 이용한 메서드는 synchronized 로 선언 되어 있습니다. <br>

한 마디로 호출전 쓰레드간 동기화 락을 걸어 멀티쓰레드 환경에서 data의 무결성을 보장해주고, <br>

데이터를 안전하게 추가, 삭제를 할 수 있습니다. 이 를 thread-safe 하다고 말합니다. <br>

하지만 이 동기화 락은 시간이 오래걸리는 동작이기 때문에 HashMap에 비해 속도가 매우 느립니다.<br>


### HashMap

반대로 HashMap은 synchronized 로 되어 있지 않기 때문에 멀티 쓰레드 환경에서 여러 쓰레드가 <br>

동시에 객체의 데이터 메소드를 이용하면 무결성이 깨지고 심각한 오류가 발생 할 수 있습니다.  <br>

말 그대로 동기화를 보장 받지 못합니다. 하지만 락을 걸지 않기 때문에 속도가 빠른것이 장점입니다. <br>


### ConCurrentHaspMap

HashMap의 멀티쓰레드 환경에서 나오는 문제점을 보완하려고 나온 자료구조다. HashMap에서 보장 받지 못하는 <br>

안정성을 ConCurrentHaspMap에서 받을수 있다. 또한 ConCurrentHashMap을 그대로 사용해도 좋지만 요즘은 <br>

Map map = Collections.synchronizedMap(new HashMap()); map 자체를 동기화 시키는 함수를 사용해서 <br>

처리하는 경우도 있다.













