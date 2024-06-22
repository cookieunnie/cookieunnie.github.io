---
layout: post
categories : SQL Programmers
tags : SQL Programmers
use_math: true
title:  "[SQL] 프로그래머스 - 자동차 대여 기록에서 대여중/대여 가능 여부 구분하기"
published : false
---
# 문제 설명
다음은 어느 자동차 대여 회사의 자동차 대여 기록 정보를 담은 `CAR_RENTAL_COMPANY_RENTAL_HISTORY` 테이블입니다. `CAR_RENTAL_COMPANY_RENTAL_HISTORY` 테이블은 아래와 같은 구조로 되어있으며, `HISTORY_ID`, `CAR_ID`, `START_DATE`, `END_DATE` 는 각각 자동차 대여 기록 ID, 자동차 ID, 대여 시작일, 대여 종료일을 나타냅니다.

|Column name|Type|Nullable|
|---|---|---|
|HISTORY_ID|INTEGER|FALSE|
|CAR_ID|INTEGER|FALSE|
|START_DATE|DATE|FALSE|
|END_DATE|DATE|FALSE|

# 문제
`CAR_RENTAL_COMPANY_RENTAL_HISTORY` 테이블에서    
2022년 10월 16일에 대여 중인 자동차인 경우 '대여중' 이라고 표시하고,    
대여 중이지 않은 자동차인 경우 '대여 가능'을 표시하는 컬럼(컬럼명: `AVAILABILITY`)을 추가하여    
자동차 ID와 `AVAILABILITY` 리스트를 출력하는 SQL문을 작성해주세요.     
이때 반납 날짜가 2022년 10월 16일인 경우에도 '대여중'으로 표시해주시고     
결과는 자동차 ID를 기준으로 내림차순 정렬해주세요.

# 예시
예를 들어 `CAR_RENTAL_COMPANY_RENTAL_HISTORY` 테이블이 다음과 같다면

|HISTORY_ID|CAR_ID|START_DATE|END_DATE|
|---|---|---|---|
|1|4|2022-09-27|2022-09-27|
|2|3|2022-10-03|2022-10-04|
|3|2|2022-10-05|2022-10-05|
|4|1|2022-10-11|2022-10-16|
|5|3|2022-10-13|2022-10-15|
|6|2|2022-10-15|2022-10-17|
2022년 10월 16일에 대여 중인 자동차는 자동차 ID가 1, 2인 자동차이고, 대여 가능한 자동차는 자동차 ID가 3, 4이므로, '대여중' 또는 '대여 가능' 을 표시하는 컬럼을 추가하고, 자동차 ID를 기준으로 내림차순 정렬하면 다음과 같이 나와야 합니다.

|CAR_ID|AVAILABILITY|
|---|---|
|4|대여 가능|
|3|대여 가능|
|2|대여중|
|1|대여중|

# 내 답변
```sql
SELECT
    CAR_ID,
    MAX(CASE WHEN '2022-10-16' 
        BETWEEN DATE_FORMAT(START_DATE, '%Y-%m-%d') 
            AND DATE_FORMAT(END_DATE, '%Y-%m-%d')
        THEN '대여중'
        ELSE '대여 가능' END) AS AVAILABILITY
FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
GROUP BY CAR_ID
ORDER BY CAR_ID DESC
```
처음에는 YEAR, MONTH, DATE로 나눠서 2022-10-16을 체크하려고 했었는데,    
BETWEEN A AND B를 사용하는게 더 좋은 방법임을 알게되었다.     
실무에서는 자주 사용했었던 것 같은데 왜 바로 생각이 나지 않았을까       
기억해두자!

# 다른 답변 
```sql
SELECT
    CAR_ID,
    (CASE WHEN CAR_ID IN(
        SELECT CAR_ID
        FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
        WHERE '2022-10-16' 
            BETWEEN DATE_FORMAT(START_DATE, '%Y-%m-%d') 
            AND DATE_FORMAT(END_DATE, '%Y-%m-%d'))
    THEN '대여중'
    ELSE '대여 가능'
    END) AS AVAILABILITY
FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
GROUP BY CAR_ID
ORDER BY CAR_ID DESC
```
CASE 안에서 서브쿼리를 사용하는 방법도 괜찮은 것 같다. 


<br>
<br>
<br>
<br>
<br>

문제 링크 : [https://school.programmers.co.kr/learn/courses/30/lessons/157340](https://school.programmers.co.kr/learn/courses/30/lessons/157340)