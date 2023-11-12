# Programmers-sql

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

⭐(다시풀어보면 좋을 문제)⭐
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







