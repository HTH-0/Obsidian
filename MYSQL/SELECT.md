[[MYSQL]]
## **SELECT 산술식;**

## **SELECT * FROM 데이터베이스;**
>> USE 데이터베이스;   이것도 가능
>> 워크벤치에서 해당 부분을 더블클릭해도 가능
>> \*는 모든 속성을 포함한다.

## **SELECT * FROM 데이터베이스.테이블;**
>>SELECT population FROM city;
>>SELECT name, population FROM city;
>>SELECT name AS '도시명' FROM city;
>>> 조회 시 속성 이름을 다른 이름으로 변경하고 싶을 때
>>> 
>>> SELECT id '아이디', name AS 도시명 FROM city; 이렇게 적어도 된다, ' '도 안붙여도 된다.
>>
>>> 특수문자, 띄어쓰기를 넣으려면 '특수문자' 이렇게 표기해야함.

## **WHERE**
>>SELECT 시 조건을 달아야 할 경우에 사용
>
>>SELECT * FROM city WHRER countrycode = 'kor';
>>SELECT * FROM city where countrycode = 'kor' or CountryCode = 'afg'; 
>>SELECT * FROM city where countrycode = 'kor' and population >= 100000;
>> 	OR, AND 뒤에 한번 더 반복 해줘야 한다.



## **LIMIT**
>>SELECT * FROM city LIMIT 1;
>>	최상위 1개만 조회
>>SELECT * FROM city LIMIT 5, 3;
>>	위에서 5번째 데이터부터 3개 조회
>>SELECT * FROM city LIMIT 5 OFFSET 3;
>>	위에서 3번째 데이터부터 5개 조회

## **DISTINCT**
>> 중복을 제거
>> SELECT DISTINCT countrycode FROM city;
>> 	중복되는 데이터들을 없앤다.
>> SELECT DISTINCT countrycode, name FROM city;
>> 	두 개 다 중복된 데이터를 제거한다. (데이터의 수가 더 많아진다는 말)

## **ORDER BY**
>>정렬하기, 기본적으로 오름차순 정렬
>>SELECT * FROM city ORDER BY Population; 
>>	(뒤에 ASC가 생략되어 있다)
>>	오름차순 정렬
>>SELECT * FROM city ORDER BY Population DESC;
>>	뒤에 DESC 붙인다면
>>	내림차순 정렬
>>