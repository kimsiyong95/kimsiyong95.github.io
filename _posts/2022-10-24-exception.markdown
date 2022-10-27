---
layout: post
title:  "체크 예외와 언체크 예외(Checked, Unchecked Exception)"
date:   2022-10-23 20:33:00 +0900
categories: Spring
---

### 체크 예외와 언체크 예외 그리고 에러(작성중)
java는 크게 체크 예외와 언체크 예외 그리고 에러로 3가지로 나뉘어집니다.<br>
구조를 보게되면 아래의 그림과 같습니다.<br>

<img src="/public/img/exception.png"  width="800" height="600"/><br>

##### 에러란?
개발자에 의해 만들어지는것이 아닌 시스템이 비정상적인 상황이 발생했을 경우입니다.<br>
예를들면 OutofMemoryError나 StackOverflowError 같은 상황을 말합니다.

##### 예외란?
예외는 코딩으로 인해 생기는 문제가 만들어진 상황입니다. 대표적으로 NullPointerException<br>
이 있습니다. 그 외로 여러가지 예외가 있고 특히 예외는 2가지로 나눌수 있습니다<br>
 - Checked Exception
 - UnChecked Exception

RuntimeException의 하위 클래스들이 UnChecked Exception 이고 Exception 클래스의<br>
하위 클래스들을 Checked Exception 이라고 합니다.

























