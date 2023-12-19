# Programmers-sql
- Mysql을 이용하여 문제를 진행하였습니다.

## JOIN >
<hr>
Q. '경제' 카테고리에 속하는 도서들의 도서 ID(BOOK_ID), 저자명(AUTHOR_NAME), 출판일(PUBLISHED_DATE) 리스트를 출력하는 SQL문을 작성해주세요.
결과는 출판일을 기준으로 오름차순 정렬해주세요.
````
-- 코드를 입력하세요
SELECT BOOK_ID, AUTHOR_NAME, DATE_FORMAT(PUBLISHED_DATE,'%Y-%m-%d') as PUBLISHED_DATE
from book as B
join author as A
on B.author_id =A.author_id
where category like '%경제%'
order by PUBLISHED_DATE;
````

Q. ANIMAL_INS 테이블은 동물 보호소에 들어온 동물의 정보를 담은 테이블입니다. ANIMAL_INS 테이블 구조는 다음과 같으며, ANIMAL_ID, ANIMAL_TYPE, DATETIME, INTAKE_CONDITION, NAME, SEX_UPON_INTAKE는 각각 동물의 아이디, 생물 종, 보호 시작일, 보호 시작 시 상태, 이름, 성별 및 중성화 여부를 나타냅니다.
ANIMAL_OUTS 테이블은 동물 보호소에서 입양 보낸 동물의 정보를 담은 테이블입니다. ANIMAL_OUTS 테이블 구조는 다음과 같으며, ANIMAL_ID, ANIMAL_TYPE, DATETIME, NAME, SEX_UPON_OUTCOME는 각각 동물의 아이디, 생물 종, 입양일, 이름, 성별 및 중성화 여부를 나타냅니다. ANIMAL_OUTS 테이블의 ANIMAL_ID는 ANIMAL_INS의 ANIMAL_ID의 외래 키입니다.

천재지변으로 인해 일부 데이터가 유실되었습니다. 입양을 간 기록은 있는데, 보호소에 들어온 기록이 없는 동물의 ID와 이름을 ID 순으로 조회하는 SQL문을 작성해주세요.
````
Select O.animal_id, O.name
from animal_ins as I 
right join animal_outs as O
on I.animal_id = O.animal_id
WHERE I.ANIMAL_ID IS NULL
````
A. Right JOin 오른쪽의 있는 모든 값을 출력 가능
문제에서 나간기록은 있는데 들어온 기록이 없는 동물을 출력하라고함
그래서 right join을 이용해서 outs에 있는 동물을 검색한 후 
조건절에 in에는 없는 조건식을 준다.

## GROUP BY >
<hr>

Q. 동물 보호소에 들어온 동물 이름 중 두 번 이상 쓰인 이름과 해당 이름이 쓰인 횟수를 조회하는 SQL문을 작성해주세요. 이때 결과는 이름이 없는 동물은 집계에서 제외하며, 결과는 이름 순으로 조회해주세요.
````
SELECT NAME, COUNT(NAME) COUNT
FROM ANIMAL_INS
WHERE NAME IS NOT NULL 
GROUP BY NAME
HAVING COUNT(NAME) >1
ORDER BY NAME;
````

⭐⭐⭐
<br>
[년, 월, 성별 별 상품 구매 회원 수 구하기]<br>
Q. USER_INFO 테이블과 ONLINE_SALE 테이블에서 년, 월, 성별 별로 상품을 구매한 회원수를 집계하는 SQL문을 작성해주세요. 결과는 년, 월, 성별을 기준으로 오름차순 정렬해주세요. 이때, 성별 정보가 없는 경우 결과에서 제외해주세요.

````
SELECT YEAR(SALES_DATE) YEAR
    , MONTH(SALES_DATE) MONTH
    , GENDER
    , COUNT(DISTINCT O.USER_ID) USERS
FROM USER_INFO U JOIN ONLINE_SALE O
ON U.USER_ID = O.USER_ID
GROUP BY YEAR(SALES_DATE), MONTH(SALES_DATE), GENDER
HAVING GENDER IS NOT NULL
````

주의할점 ! 
SALES_DATE와 성별 별로 구분 지었기 때문에 3가지중 날짜가 다르거나 상품이 다를 경우, 같은 user id라도 여러개의 판매 데이터가 존재할 수 있습니다. 중복 제거를 위해서 DISTINCT를 사용하여 풀기

⭐⭐⭐⭐⭐<br>
[입양 시각 구하기(2)]<br>
Q.보호소에서는 몇 시에 입양이 가장 활발하게 일어나는지 알아보려 합니다. 0시부터 23시까지, 각 시간대별로 입양이 몇 건이나 발생했는지 조회하는 SQL문을 작성해주세요. 이때 결과는 시간대 순으로 정렬해야 합니다.

````
-- 코드를 입력하세요
SET @HOUR = -1;
SELECT (@HOUR := @HOUR +1) AS HOUR,
    (SELECT COUNT(HOUR(DATETIME)) 
    FROM ANIMAL_OUTS 
    WHERE HOUR(DATETIME)=@HOUR) AS COUNT 
    FROM ANIMAL_OUTS
WHERE @HOUR < 23;
````

주의할점 ! 
set 변수를 이용하여 문제 해결한다. 데이터에 존재하지않는 시간들까지 출력해야하기 때문에 SET 변수로 특정값을 넣어 0~23시까지 반복하여 출력하도록 한다.
[https://velog.io/@gpals4431/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4%EC%9E%85%EC%96%91-%EC%8B%9C%EA%B0%81-%EA%B5%AC%ED%95%98%EA%B8%B02%EC%99%80-SET%EB%B3%80%EC%88%98]

⭐⭐⭐<br>
[가격대 별 상품 개수 구하기]<br>
Q. PRODUCT 테이블에서 만원 단위의 가격대 별로 상품 개수를 출력하는 SQL 문을 작성해주세요. 이때 컬럼명은 각각 컬럼명은 PRICE_GROUP, PRODUCTS로 지정해주시고 가격대 정보는 각 구간의 최소금액(10,000원 이상 ~ 20,000 미만인 구간인 경우 10,000)으로 표시해주세요. 결과는 가격대를 기준으로 오름차순 정렬해주세요.
````
SELECT FLOOR(PRICE/10000)*10000 AS `PRICE_GROUP`
    , COUNT(*) AS PRODUCTS
FROM PRODUCT
GROUP BY `PRICE_GROUP`
ORDER BY `PRICE_GROUP`;
````
<HR>

## ISNULL >
<HR>
[경기도 위치한 식품창고 목록 출력]
Q. FOOD_WAREHOUSE 테이블에서 경기도에 위치한 창고의 ID, 이름, 주소, 냉동시설 여부를 조회하는 SQL문을 작성해주세요. 이때 냉동시설 여부가 NULL인 경우, 'N'으로 출력시켜 주시고 결과는 창고 ID를 기준으로 오름차순 정렬해주세요.

````
SELECT WAREHOUSE_ID,    
    WAREHOUSE_NAME,
    ADDRESS,
    IFNULL(FREEZER_YN, 'N') AS FREEZER_YN
FROM FOOD_WAREHOUSE
WHERE ADDRESS LIKE '%경기도%'
ORDER BY  WAREHOUSE_ID 
````



## SELECT >
<hr>

Q. 동물 보호소에 들어온 모든 동물의 정보를 ANIMAL_ID순으로 조회하는 SQL문을 작성해주세요. SQL을 실행하면 다음과 같이 출력되어야 합니다.

-- 코드를 입력하세요

    SELECT * FROM ANIMAL_INS
    ORDER BY ANIMAL_ID;

정답!

Q. 동물 보호소에 들어온 동물 중 아픈 동물1의 아이디와 이름을 조회하는 SQL 문을 작성해주세요. 이때 결과는 아이디 순으로 조회해주세요.

-- 코드를 입력하세요
````
SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS 
WHERE INTAKE_CONDITION LIKE '%SICK%'
ORDER BY ANIMAL_ID;
````

Q. 동물 보호소에 들어온 동물 중 젊은 동물1의 아이디와 이름을 조회하는 SQL 문을 작성해주세요. 이때 결과는 아이디 순으로 조회해주세요.
````
SELECT ANIMAL_ID, NAME 
FROM ANIMAL_INS 
WHERE NOT INTAKE_CONDITION LIKE'%AGED%'
ORDER BY ANIMAL_ID;
````

Q. 동물 보호소에 가장 먼저 들어온 동물의 이름을 조회하는 SQL 문을 작성해주세요.
````
SELECT NAME FROM 
    (SELECT NAME FROM ANIMAL_INS ORDER BY DATETIME)
WHERE ROWNUM=1
````

Q. USER_INFO 테이블에서 2021년에 가입한 회원 중 나이가 20세 이상 29세 이하인 회원이 몇 명인지 출력하는 SQL문을 작성해주세요.
````
SELECT COUNT(*) USER FROM USER_INFO
WHERE JOINED LIKE '%2021%' AND AGE BETWEEN 20 AND 29;
````

## SUM, MAX, MIN >
<hr>

Q. PRODUCT 테이블에서 판매 중인 상품 중 가장 높은 판매가를 출력하는 SQL문을 작성해주세요. 이때 컬럼명은 MAX_PRICE로 지정해주세요.
````
SELECT MAX (PRICE) MAX_PRICE
FROM PRODUCT;
````

Q. 동물 보호소에 들어온 동물의 이름은 몇 개인지 조회하는 SQL 문을 작성해주세요. 이때 이름이 NULL인 경우는 집계하지 않으며 중복되는 이름은 하나로 칩니다.
````
SELECT COUNT(name) COUNT FROM (select name from ANIMAL_INS group by name);
````

Q. REST_INFO 테이블에서 음식종류별로 즐겨찾기수가 가장 많은 식당의 음식 종류, ID, 식당 이름, 즐겨찾기수를 조회하는 SQL문을 작성해주세요. 이때 결과는 음식 종류를 기준으로 내림차순 정렬해주세요.
````
SELECT FOOD_TYPE, REST_ID, REST_NAME, FAVORITES
FROM REST_INFO 
WHERE (FOOD_TYPE, FAVORITES) IN (SELECT FOOD_TYPE, MAX(FAVORITES)
                                 FROM REST_INFO GROUP BY FOOD_TYPE)
ORDER BY FOOD_TYPE DESC;
````
