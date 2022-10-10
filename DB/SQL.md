# SELECT : 원하는 정보 가져오기

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
위와 같이 id가 10보다 큰 Member의 username, password 컬럼 값을 가져올 수 있다.  


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


# 알아두면 편한 연산자들 

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
SELECT id, username FROM Member WHERE username LIKE '_hello__');
```
_ 가 있는 것은 그 수만큼 문자가 있는 것을 표현한다. 위와 같은 경우는 맨 앞에 어떤 문자 하나가 있고 그 뒤에 hello가 온 다음 뒤에 문자 2개가 더 있다.


# 그룹 함수

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




