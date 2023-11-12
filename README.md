# Programmers-sql

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
