# SQL

SQL은 데이터베이스를 조작하는 언어입니다. 데이터베이스 관리 시스템(DBMS)에 접속하여 명령어를 전달할 수 있습니다.  
DBMS에서는 여러 개의 데이터베이스를 관리할 수 있습니다. SQL에 명령어에는 대표적으로 DDL, DML이 있습니다.  
DDL(Data Definition Language) 은 데이터 정의어로 데이터베이스 스키마를 정의하거나 조작하기 위한 언어입니다.  
DML(Data Manipulation Language) 은 데이터 조작어로 데이터베이스 내부 데이터를 관리하기 위한 언어입니다.  
이제부터 어떠한 SQL 명령어가 있는지 알아보겠습니다.  

우선 데이터베이스를 관리하는 명령어를 알아보겠습니다.  

### SHOW : 데이터베이스 목록 표시

DBMS가 관리하고 있는 데이터베이스 목록을 보여줍니다. 

```
SHOW DATABASES;
```

### USE : 데이터베이스 선택

DBMS가 관리하고 있는 데이터베이스 중 어떤 것을 사용할지 선택합니다.

```
USE {데이터베이스 이름}
```

## DDL(Data Definition Language) 

### CREATE : 데이터베이스 생성

```
CREATE DATABASE {데이터베이스 이름};
```

### DROP : 데이터베이스 삭제

```
DROP DATABASE {데이터베이스 이름}
```

### CREATE TABLE : 테이블 생성

CREATE는 데이터베이스를 생성하는 명령어고 CREATE TABLE은 데이터베이스 안에서   
테이블을 생성하는 명령어입니다.
```
CREATE TABLE {데이터베이스 이름} (id INT, name VARCHAR(100));
```

### DROP TABLE : 테이블 삭제

DROP은 데이터베이스를 삭제하는 명령어고 DROP TABLE은 데이터베이스 안에서   
테이블을 삭제하는 명령어입니다.  
```
DROP TABLE {데이터베이스 이름}
```

## DML(Data Manipulation Language)

### SELECT : 원하는 행 가져오기, 레코드 취득

1. DB에 테이블이 있고 그 테이블 안에 컬럼들이 있다. Member 라는 Entity에 id, username, password, auth 라는 컬럼들이 있다고 하겠습니다.  

```
SELECT * FROM Member;
```
다음과 같이 작성을 하면 Member 안에 있는 데이터를 컬럼 별로 모두 가져올 수 있습니다.

2. Member의 컬럼 중 내가 원하는 컬럼만 가져올 수도 있습니다. 
```
SELECT id, username FROM Member;
```

3. WHERE를 이용하면 원하는 조건만 가져올 수 있습니다.
```
SELECT id, username FROM Member WHERE id > 10;
```
위와 같이 id가 10보다 큰 Member의 id, username 컬럼 값을 가져올 수 있습니다.


4. ORDER BY를 이용하면 원하는 순서로 데이터를 가져올 수 있습니다.  
```
SELECT id, username FROM Member ORDER BY id;
```
이렇게 하면 id 오름차순으로 데이터를 가져옵니다. 기본이 오름차순이고(ASC) 뒤에 DESC를 붙이면 내림차순으로 바꿀 수 있습니다.  

5. LIMIT, OFFSET을 이용하면 원하는 만큼만 데이터를 가져올 수 있습니다.

```
SELECT id, username FROM Member LIMIT 10 OFFSET 2;
```
OFFSET의 시작은 0이므로 3번째 행부터 데이터가 10개보다 크다면 그 이후에는 가져오지 않고 10개까지만 가져옵니다.  

6. AS를 이용하면 원하는 이름으로 가져올 수 있습니다.  

```
SELECT id AS MemberID, username AS MemberName FROM Member;
```  
컬럼명과 다른이름으로 가져오고 싶을 때는 다음과 같이 AS로 바꿀 수 있습니다.                     


## 알아두어야 할 연산자들 

1. BETWEEN A AND B, NOT BETWEEN A AND B

```
SELECT id, username FROM Member WHERE id BETWEEN 3 AND 10;
```
id가 3, 10 두 값 사이에 있는 Member만 가져옵니다.  


2. IN, NOT IN 

```
SELECT id, username FROM Member WHERE id IN (3,4,5,6,7);
```
id가 3,4,5,6,7 중 하나인 Member만 가져옵니다.  

3. LIKE 

```
SELECT id, username FROM Member WHERE username LIKE 'hello%';
```
%가 있는 것은 문자가 0개이상 있는 것을 표현합니다. 위와 같은 경우는 처음에는 hello로 시작하고 뒤에는 어떤 문자가 와도 상관없습니다.  

```
SELECT id, username FROM Member WHERE username LIKE '_hello__';
```
_ 가 있는 것은 그 수만큼 문자가 있는 것을 표현한다.     
위와 같은 경우는 맨 앞에 어떤 문자 하나가 있고 그 뒤에 hello가 온 다음 뒤에 문자 2개가 더 있습니다.  

4. IS NULL

age = NULL 이 아니라 IS NULL을 사용합니다

```
SELECT * FROM Member WHERE age IS NULL;
```

## 그룹 함수

그룹 함수는 조건에 따라 집계된 값을 가져옵니다. 그룹 함수에는 MAX, MIN, COUNT, SUM, AVG가 있습니다.  
```
SELECT MAX(id) FROM Member WHERE age Between 20 AND 30;
```
다음은 나이가 20에서 30사이인 사람들 중에서 가장 id가 큰 멤버의 id를 가져옵니다.  
같은 방식으로 MIN은 최소값을 COUNT는 NULL을 제외한 갯수를 SUM은 총합을 AVG는 평균을 가져옵니다.   

> MAX와 GREATEST의 차이점 

MAX는 조건에 따라 (WHERE) 그룹으로 묶인 데이터 중에서 최대를 찾는 것입니다. GREATEST는 
```
SELECT GREATEST(1,2,3);
```
위와 같이 괄호 안에서 가장 큰 값을 찾을 때 사용합니다. MIN, LEAST도 마찬가지입니다.  

## 그룹으로 묶기

1. GROUP BY           
```
SELECT City FROM Member GROUP BY City;
```  
City로 그룹을 만듭니다. 중복되는 City가 있어도 같은 하나로 묶여 한번만 가져옵니다.   

```
SELECT Country, City FROM Member GROUP BY Country, City;
```   
여러 컬럼으로 그룹 지을 수도 있습니다. 이 경우 둘 다 같을 경우 하나의 그룹으로 묶입니다.   

2. HAVING  

```
SELECT City, Count(*) AS Count FROM Member GROUP BY City HAVING Count <= 3;
```
HAVING은 그룹화 된 데이터를 걸러냅니다.  

> WHERE과 HAVING의 차이점  
WHERE은 그룹화 하기 전 데이터를 HAVING은 그룹화 한 후 데이터를 집계할 때 사용합니다.  

3. DISTINCT

```
SELECT DISTINCT City FROM Member;
```   
중복된 값들을 제거합니다. GROUP BY와 다른 점은 집계에 사용되지 않습니다.   
또한 정렬을 고려하지 않아 GROUP BY 보다 빠릅니다.  

## 데이터의 추가, 삭제, 갱신

1. INSERT : 행 추가하기

SELECT가 데이터 검색을 위한 명령이라면 INSERT는 데이터를 추가하는 명령입니다.   
테이블에 레코드를 추가할 때 사용합니다.  

```
INSERT INTO 테이블명(열1, 열2, ...) VALUES(값1, 값2, ...);
```
2. DELETE : 삭제하기  

DELETE 명령으로 행을 삭제할 수 있다. DELETE는 행 전체를 삭제하는 것이다. 행의 특정 열만 삭제하는 것은 불가능하다.     
DELETE 문장은 WHERE와 조합하여 삭제할 대상 레코드를 지정하는 경우가 많습니다.  

```
DELETE FROM 테이블명 WHERE 조건식;
```
3. UPDATE : 갱신하기

UPDATE 명령으로 행의 셀 값을 갱신할 수 있다. UPDATE 문장은 WHERE와 조합하여 사용하는 경우가 많습니다. 
```
UPDATE 테이블명 SET 열1 = 값1, 열2 = 값2, ... WHERE 조건식;
```

## JOIN

테이블을 결합해서 데이터를 가져올 수도 있습니다. 테이블의 결합 종류에는 내부결합과 외부결합이 있습니다.  

### INNER JOIN

내부조인은 키가 되는 컬럼 값이 테이블 간에 일치하는 레코드만을 결합하여 가져옵니다.  
즉 두 테이블 모두에 있을 때만 가져옵니다.  
```
SELECT * FROM users INNER JOIN items ON users.item_id = items.id;
```

### LEFT JOIN, RIGHT JOIN

외부 조인은 결합주체 테이블의 데이터와 키가 되는 컬럼의 값이 일치하는 결합객체의 데이터를  
결합하여 가져옵니다. 즉 결합 주체 테이블에만 있으면 결합 객체의 컬럼 값은 존재하지 않으므로  
결과에는 표시되지 않습니다.   

```
SELECT * FROM users LEFT JOIN items ON users.item_id = items.id;
```

출처 : 그림으로 배우는 데이터베이스(사카가미 코오다이), 얄팍한 코딩사전(인프런)

