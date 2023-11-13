# Programmers-sql
- Mysql을 이용하여 문제를 진행하였습니다.

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
[년, 월, 성별 별 상품 구매 회원 수 구하기]
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

⭐⭐⭐⭐⭐
[입양 시각 구하기(2)]
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

⭐⭐⭐
[가격대 별 상품 개수 구하기]
Q. PRODUCT 테이블에서 만원 단위의 가격대 별로 상품 개수를 출력하는 SQL 문을 작성해주세요. 이때 컬럼명은 각각 컬럼명은 PRICE_GROUP, PRODUCTS로 지정해주시고 가격대 정보는 각 구간의 최소금액(10,000원 이상 ~ 20,000 미만인 구간인 경우 10,000)으로 표시해주세요. 결과는 가격대를 기준으로 오름차순 정렬해주세요.



