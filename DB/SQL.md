## SELECT : 원하는 행 가져오기

1. DB에 테이블이 있고 그 테이블 안에 컬럼들이 있다. Member 라는 Entity에 id, username, password, auth 라는 컬럼들이 있다고 하자.  
```
SELECT * FROM Member;
```
다음과 같이 작성을 하면 Member 안에 있는 데이터를 컬럼 별로 모두 가져올 수 있다.

2. Member의 컬럼 중 내가 원하는 컬럼만 가져올 수도 있다.  
```
SELECT id, username FROM Member;
```

3. WHERE를 이용하면 원하는 조건만 가져올 수 있다.
```
SELECT id, username FROM Member WHERE id > 10;
```
위와 같이 id가 10보다 큰 Member의 id, username 컬럼 값을 가져올 수 있다.  


4. ORDER BY를 이용하면 원하는 순서로 데이터를 가져올 수 있다.
```
SELECT id, username FROM Member ORDER BY id;
```
이렇게 하면 id 오름차순으로 데이터를 가져온다. 기본이 오름차순이다(ASC) 뒤에 DESC를 붙이면 내림차순으로 바꿀 수 있다.

5. LIMIT을 이용하면 원하는 만큼만 데이터를 가져올 수 있다.

```
SELECT id, username FROM Member LIMIT 10;
```
이렇게 하면 데이터가 10개보다 크다면 그 이후에는 가져오지 않고 10개까지만 가져온다.

6. AS를 이용하면 원하는 이름으로 가져올 수 있다.  

```
SELECT id AS MemberID, username AS MemberName FROM Member;
```  
컬럼명과 다른이름으로 가져오고 싶을 때는 다음과 같이 AS로 바꿀 수 있다.                     


## 알아두어야 할 연산자들 

1. BETWEEN A AND B, NOT BETWEEN A AND B

```
SELECT id, username FROM Member WHERE id BETWEEN 3 AND 10;
```
id가 3, 10 두 값 사이에 있는 Member만 가져온다.


2. IN, NOT IN 

```
SELECT id, username FROM Member WHERE id IN (3,4,5,6,7);
```
id가 3,4,5,6,7 중 하나인 Member만 가져온다.

3. LIKE 

```
SELECT id, username FROM Member WHERE username LIKE 'hello%';
```
%가 있는 것은 문자가 0개이상 있는 것을 표현한다. 위와 같은 경우는 처음에는 hello로 시작하고 뒤에는 어떤 문자가 와도 상관없다.

```
SELECT id, username FROM Member WHERE username LIKE '_hello__';
```
_ 가 있는 것은 그 수만큼 문자가 있는 것을 표현한다. 위와 같은 경우는 맨 앞에 어떤 문자 하나가 있고 그 뒤에 hello가 온 다음 뒤에 문자 2개가 더 있다.


## 그룹 함수

그룹 함수는 조건에 따라 집계된 값을 가져온다. 그룹 함수에는 MAX, MIN, COUNT, SUM, AVG가 있다.  
```
SELECT MAX(id) FROM Member WHERE age Between 20 AND 30;
```
다음은 나이가 20에서 30사이인 사람들 중에서 가장 id가 큰 멤버의 id를 가져온다.  
같은 방식으로 MIN은 최소값을 COUNT는 NULL을 제외한 갯수를 SUM은 총합을 AVG는 평균을 가져온다.

> MAX와 GREATEST의 차이점 

MAX는 조건에 따라 (WHERE) 그룹으로 묶인 데이터 중에서 최대를 찾는 것이다. GREATEST는 
```
SELECT GREATEST(1,2,3);
```
다음과 같이 괄호 안에서 가장 큰 값을 찾을 때 사용한다. MIN, LEAST도 마찬가지이다.

## 그룹으로 묶기

1. GROUP BY           
```
SELECT City FROM Member GROUP BY City;
```  
City로 그룹을 만든다. 중복되는 City가 있어도 같은 하나로 묶여 한번만 가져온다.

```
SELECT Country, City FROM Member GROUP BY Country, City;
```   
여러 컬럼으로 그룹 지을 수도 있다. 이 경우 둘 다 같을 경우 하나의 그룹으로 묶인다.  

2. HAVING  

```
SELECT City, Count(*) AS Count FROM Member GROUP BY City HAVING Count <= 3;
```
HAVING은 그룹화 된 데이터를 걸러냅니다.

> WHERE과 HAVING의 차이점  
WHERE은 그룹화 하기 전 데이터를 HAVING은 그룹화 한 후 데이터를 집계할 때 사용한다.

3. DISTINCT

```
SELECT DISTINCT City FROM Member;
```  
중복된 값들을 제거한다. GROUP BY와 다른 점은 집계에 사용되지 않는다. 또한 정렬을 고려하지 않아  
GROUP BY 보다 빠르다

## 데이터의 추가, 삭제, 갱신

1. INSERT : 행 추가하기

SELECT가 데이터 검색을 위한 명령이라면 INSERT는 데이터를 추가하는 명령이다.  

```
INSERT INTO 테이블명(열1, 열2, ...) VALUES(값1, 값2, ...);
```
2. DELETE : 삭제하기  

DELETE 명령으로 행을 삭제할 수 있다. DELETE는 행 전체를 삭제하는 것이다. 행의 특정 열만 삭제하는 것은 불가능하다.   

```
DELETE FROM 테이블명 WHERE 조건식;
```
3. UPDATE : 갱신하기

UPDATE 명령으로 행의 셀 값을 갱신할 수 있다. 
```
UPDATE 테이블명 SET 열1 = 값1, 열2 = 값2, ... WHERE 조건식;
```
출처 : 얄팍한 코딩사전

