---
layout: post
title: Oracle 날짜 관련 쿼리
date: 2022-03-02 19:20:23 +0900
category: DBMS
---
# 참고 함수 및 포맷
{% highlight ruby %}
# REF FUNCTION
TO_CHAR(TARGET, FORMAT) : 숫자 또는 날짜를 지정한 포맷에 맞게 변환
TO_DATE(TARGET)         : 문자를 DATE로 변환

# FORMATS
'YYYY'  :   4자리 년도로 표시 
'YY'    :   끝의 2자리 연도로 표시 ('2019' -> '19')
'YEAR'  :   연도를 알파뱃으로 표시 
'Q'     :   해당월의 분기
'MM'    :   달을 두자리 숫자로 표시
'MON'   :   달을 알파뱃 약어로 표시 
'MONTH' :   달을 알파뱃으로 표시
'W'     :   해당 월의 주차
'WW'    :   현재 년도의 1일부터 7일단위의 주차
'IW'    :   날짜가 해당한 주가 하루라도 다음해인 경우에는 그달의 첫주로 한다
'DD'    :   일자를 두자리 숫자로 표시
'DAY'   :   일자에 해당하는 요일
'DY'    :   일자에 해당하는 요일의 약어
'HH'    :   12시간으로 표시 (1 - 12)
'HH24'  :   24시간으로 표시 (0 - 23)
'MI'    :   분을 표시
'SS'    :   초를 표시
'AM, PM':   오전, 오후를 표시
{% endhighlight %}
# 날짜 계산
{% highlight ruby %}
# EX) SYSDATE = "2019-08-27 13:24:00"

# 특정 날짜와 현재 날짜와의 차이를 NUMBER 형으로 반환
#   = 0.0500205346475507765830346475507765830346
SELECT MONTHS_BETWEEN(SYSDATE, TO_DATE('2019-08-26'), 'YYYYMMDD') FROM DUAL; 

# 특정 날짜에 N 개월을 더하기
#   = 2019-12-27 13:13:51
SELECT ADD_MONTHS(SYSDATE, 4) FROM DUAL;

# 특정 날짜의 다음주 특정 요일 구하기
#   ! 시스템 장소 정보에 따라 한국일 경우 두번째 인자를 한글로, 영어권일 경우 영어로해야
#     에러가 발생하지 않는다.
#   = 2019-09-02 13:26:27
SELECT NEXT_DAY(SYSDATE, '월요일') FROM DUAL;

# 특정 날짜의 반올림 EX) 특정 날짜의 시간이 오후면 반올림
#   = 2019-08-28 00:00:00
SELECT ROUND(SYSDATE, 'DD') FROM DUAL;

# 전년도의 12월 31일
SELECT TRUNC(SYSDATE, 'IY') FROM DUAL;

# 같은 년도의 첫날
SELECT TRUNC(SYSDATE, 'YY') FROM DUAL;

# 같은 년도의 마지막날
#   = 2019-12-31 00:00:00
SELECT ADD_MONTHS(TRUNC(SYSDATE, 'YEAR'), 12) - 1 FROM DUAL;

# 분기의 첫달 1일
SELECT TRUNC(SYSDATE, 'Q') FROM DUAL;

# 특정 날짜가 속한 달의 첫날 
#   = 2019-08-01 00:00:00
SELECT TRUNC(SYSDATE, 'MONTH') FROM DUAL;

# 특정 날짜가 속한 달의 마지막 날
#   = 2019-08-31 13:23:53
SELECT LAST_DAY(SYSDATE) FROM DUAL;

# 특정 달의 일수
#   = 31
SELECT CAST(TO_CHAR(LAST_DAY(SYSDATE), 'DD') AS INT) FROM DUAL;

# YYYY-MM-01 표시
SELECT TRUNC(SYSDATE, 'MM') FROM DUAL;

# YYYY-MM-DD 표시
SELECT TRUNC(SYSDATE, 'DD') FROM DUAL;

# 분기 계산
SELECT TRUNC(SYSDATE, 'Q') 'N분기' FROM DUAL;

# 주차 구하기 
# 일요일 기준
SELECT TRUNC((TRUNC(SYSDATE) - TRUNC(TRUNC(SYSDATE, 'YY'), 'D')) / 7) + 1 
# 월요일 기준
SELECT TRUNC((TRUNC(SYSDATE) - TRUNC(TRUNC(SYSDATE, 'YY'), 'IW')) / 7) + 1
{% endhighlight %}
# 출저
<a href="https://m.blog.naver.com/PostView.nhn?blogId=tsum3000&amp;logNo=80085482543&amp;proxyReferer=http%3A%2F%2Fwww.google.com%2Furl%3Fsa%3Dt%26rct%3Dj%26q%3D%26esrc%3Ds%26source%3Dweb%26cd%3D2%26ved%3D2ahUKEwjrzp7Ix5jkAhXYAYgKHedECDwQFjABegQIAhAB%26url%3Dhttp%253A%252F%252Fm.blog.naver.com%252Ftsum3000%252F80085482543%26usg%3DAOvVaw2cxD2EDcWalzKEo5FBas6L">오라클 TRUNC 날짜 기능</a>
<a href="https://rocabilly.tistory.com/273">SYSDATE 기준 일자관련 다양한 표현2</a>
<a href="https://applejara.tistory.com/445">날짜 함수 및 날짜 구하기</a>
<a href="https://m.blog.naver.com/PostView.nhn?blogId=afungy&amp;logNo=100154103724&amp;proxyReferer=http%3A%2F%2Fwww.google.com%2Furl%3Fsa%3Dt%26rct%3Dj%26q%3D%26esrc%3Ds%26source%3Dweb%26cd%3D1%26ved%3D2ahUKEwiVoo2rwpjkAhWVfd4KHUPIBD0QFjAAegQIABAB%26url%3Dhttp%253A%252F%252Fm.blog.naver.com%252Fafungy%252F100154103724%26usg%3DAOvVaw1SzHFbfGYZ6neJ25mpRPgZ">오라클 날짜계산</a>
<a href="http://www.gurubee.net/article/48729">년 주차 구하기</a>