## Ch 04. 테이터베이스 모델링

데이터베이스 모델링이란 현실 세계의 작업, 사물들을 DBMS의 데이터베이스 테이블로 옮기는 과정이다.

### MySQL Workbench 모델링 툴 실습

- 다이어그램 작성 : Menu → File → New Model
- 다이어그램 저장 : Menu → Save Model
- 다이어그램 → 데이터베이스 테이블로 변환 : Menu → Database → Foward Engineer
- 데이터베이스 → 다이어그램으로 변환 : Menu → Database → Reverse Engineer

## Ch 06. SQL 기본

`show table status` : 테이블의 정보 조회

`show tables` :  테이블의 이름만 간단히 조회

## select

열 이름 별칭 지정하여 조회

`select first_name as 이름, gender 성별, hire_date '회사 입사일' from employees;`

- as 생략 가능
- 별칭에 공백이 있다면 ''로 감싼다.

참고 : MySQL 문자열

일부 DBMS는 char, varchar는 영문자 기준으로 한글자당 1byte를 할당하고, nchar와 nvarchar는 유니코드를  기준으로 2byte 할당한다. 그래서 영문자는 char, varchar를 사용하고, 한글은 nchar, nvarchar를 사용해야 한다. 하지만 MySQL 8.0은 char와 varchar 모두 utf-8을 사용하여 영문/숫자/기호를 사용하면 1byte, 한글/중국어/일본어 등을 입력하면 3byte가 자동으로 할당되기 때문에 nchar, nvarvhar를 사용할 필요가 없다.

그래서 char(10)을 사용하면 영문, 한글 구분없이 10글자까지 입력할 수 있다.

### between A and B

키가 180~183인 사람을 조회

`select * from usertbl where height between 180 and 183;`

### in()

지역이 '경남', '전남', '경북'인 사람의 정보를 조회

`select * from usertbl where height between 180 and 183;`

### 문자열 검색 : like

이름이 김으로 시작하는 사람 조회

`select name, height from usertbl where name like '김%';`

- % : 무엇이든 허용
- _ : 글자 1개 매치

**참고** 

%, _ 가 검색할 문자열의 제일 앞에 들어가면 MySQL 성능에 악영향을 끼치게된다. 예를 들어 name 열을 '%용' 등으로 검색하면, name 열에 인덱스가 있더라도 인덱스를 사용하지 않고 전체 데이터를 검색한다.

### ANY/ALL/SOME 그리고 서브쿼리

```sql
select name, height from usertbl where height >= any(select height from usertbl where addr = '경남');

select name, height from usertbl where height >= all(select height from usertbl where addr = '경남');

select name, height from usertbl where height = any(select height from usertbl where addr = '경남');
```

- any 또는 some : 서브쿼리의 여러 개의 결과 중 한가지만 만족하면됨.
- all : 서브쿼리의 여러 개의 결가를 모두 만족해야함.
- `= any(서브쿼리)` == `in(서브쿼리)`

### ORDER BY

```sql
select name, height from usertbl order by name asc, height desc;
select name, height from usertbl order by name, height desc;
```

- asc(오름차순)은 디폴트 값이므로 생략 가능.
- order by 절은 select, from, where, group by, having 중에서 제일 뒤에 와야한다.
- **order by 절은 MySQL 의 성능에 상당한 영향을 줄 수 있기 때문에, 꼭 필요한 경우가 아니라면 사용하지 말자.**

### DISTINCT - 중복 제거

```sql
select distinct from usertbl;
```

### LIMIT - 출력 개수 제한

```sql
select * from userTbl limit 5;

-- 0부터 시작해서 5개 출력
select * from userTbl limit 0, 5;
select * from userTbl limit 5 offset 0;
```

### CREATE TABLE ... SELECT - 테이블 복사

- PK, FK 등의 제약조건은 복사되지 않는다.

```sql
-- member 테이블의 내용을 member_copy 테이블에 복사
create table member_copy (select * from member);

create table member_name (select name from member);
```

### GROUP BY, HAVING, 집계함수

#### 집계 함수

- 집계함수는 주로 group by 절과 함께 쓰이며 데이터를 그룹핑 해준다.

```sql
-- 사용자별로 총 구매한 금액
select userID as '유저 아이디', sum(amount*price) as '총 구매금액' from buytbl group by userID;

-- 전체 구매 중 평균 구매 개수
select avg(amount) as '평균 구매 개수' from buytbl;

-- 가장 큰키, 가장 작은키를 가진 사람의 이름과 키를 출력(서브쿼리에서 집계함수 사용 예)
select name, height from usertbl where height = (select max(height) from usertbl) or height = (select min(height) from usertbl);
```

집계 함수의 종류

- avg() : 평균
- min() : 최소값
- max() : 최대값
- count() : 행의 개수
- count(distinct) : 행의 개수(중복 제거)
- stdev() : 표준편차
- var_samp() : 분산

#### Having 절

- 집계 함수의 조건을 지정한다.
- **having 절은 group by 절 뒤에 와야한다.**

```sql
select userID as '사용자', sum(price*amount) as '총 구매액' 
	from buytbl 
	group by userID 
	having sum(price*amount) > 1000;
	order by sum(price*amount);
```

#### ROLLUP

- 분류 별로 합계 및 총 합계를 구하고 싶다면 WITH ROLLUP 문을 사용하면 된다.

```sql
-- num, groupName 쌍의 각 합계와 groupName 별 합계 및 총 합계를 같이 출력
select num, groupName, SUM(price*amount) AS '비용' 
	from buytbl 
	group by groupName, num 
	with rollup;

-- groupName 별 합계와 총 합계 출력
select groupName, sum(price*amount) as '비용' 
	from buytbl 
	group by groupName 
	with rollup;
```

### SQL의 분류

### DML(Data Manipulation Language)

- 데이터 조작(선택, 삽입, 수정, 삭제)에 사용되는 언어
- SELECT, INSERT, UPDATE, DELETE
- 트랜잭션이 발생하는 SQL이 DML이다.

### DDL(Data Definition Language)

- 데이터 정의어
- 데이터베이스, 테이블, 뷰, 인덱스와 같은 데이터베이스 개체를 생성, 삭제, 변경한다.
- CREATE, DROP, ALTER 등
- 트랜잭션을 발생시키지 않고 실행 즉시 적용되기 때문에 롤백하거나 커밋할 수 없다.

### DCL(Data Control Language)

- 데이터 제어 언어
- 사용자에게 권한을 부여하는 것과 관련된 언어
- GRANT, REVOKE, DENY 등

### 데이터 변경 SQL 문

### INSERT

```sql
INSERT INTO member (id, name, age) values (1, 'Kim', 25);

-- 테이블 이름 다음에 나오는 컬럼 이름들 생략 가능. 
-- 생략할 경우 values 다음에 오는 값들이 테이블의 열 순서 및 개수가 동일해야함
INSERT INTO member values (1, 'Kim', 25);
```

#### INSERT INTO ... SELECT

- 다른 테이블의 데이터를 가져와서 입력하기

```sql
-- employees 테이블의 데이터를 가져와서 member 테이블에 입력한다.
INSERT INTO member 
	SELECT emp_no, first_name, last_name from employees.employees;
```

#### INSERT IGNORE - PK 중복 무시

- 입력하는 데이터 중 PK 중복이 있으면 에러가 발생하면서 나머지 데이터들도 입력되지 않는다.

```sql
INSERT INTO member VALUES (1, 'kim', 25); -- PK 중복
INSERT INTO member VALUES (2, 'jeong', 21);
INSERT INTO member VALUES (3, 'lee', 26);
```

위 코드에서 PK 1이 중복된 키 값이라고 가정하면, SQL 실행시 에러가 발생하면서 나머지 중복되지 않은 키 값 2, 3을 가진 데이터들도 입력되지 못한다.

```sql
INSERT IGNORE INTO member VALUES (1, 'kim', 25); -- PK 중복
INSERT IGNORE INTO member VALUES (2, 'jeong', 21);
INSERT IGNORE INTO member VALUES (3, 'lee', 26); 
```

이러한 상황에 `**IGNORE` 문을 사용하면 중복되는 PK 값이 있는 경우 에러를 발생시키지 않고 해당 데이터는 무시하고 넘어가기 때문에 나머지 데이터들은 정상적으로 입력이 된다.**

#### ON DUPLICATE KEY UPDATE - PK 중복시 UPDATE

- ON DUPLICATE KEY UPDATE 를 사용하면 PK가 중복될 때 INSERT가 아닌 UPDATE 문이 실행되도록 할 수 있다. → PK 중복이 아니면 INSERT, 중복이면 UPDATE

```sql
-- PK 1이 중복이 아니면 정상적으로 INSERT, 중복이면 PK 1번의 name과 age를 lee와 23으로 UPDATE 한다.
INSERT INTO member VALUES(1, 'kim', 25) 
	ON DUPLICATE KEY UPDATE name = 'lee', age = 23;
```

### AUTO_INCREMENT

테이블 열의 속성이 AUTO_INCREMENT로 지정되어 있으면 행이 추가 될 때마다 자동으로 1부터 1씩 증가하는 값을 해당 컬럼에 넣어준다.

INSERT문에서 AUTO_INCREMENT 가 설정된 열이 없다고 생각하고 데이터를 추가하면 된다. NULL 값으로 입력하면 자동으로 값이 저장된다.

```sql
CREATE TABLE member (
	id INT AUTO_INCREMENT PRIMARY KEY,
    name CHAR(3),
    age INT
);

INSERT INTO member (name, age) VALUES ("kim", 12);
INSERT INTO member VALUES (null, "lee", 25);

-- auto_increment 를 사용한 컬럼에 마지막으로 입력된 값을 출력해준다.
SELECT LAST_INSERT_ID();

-- auto_increment 초기값을 1000으로 변경
ALTER TABLE member AUTO_INCREMENT = 1000;

-- auto_increment 증가값을 3으로 변경
SET @@auto_increment_increment=3;
```

### UPDATE

```sql
UPDATE 테이블명 SET 컬럼명1=값1, 컬럼명2=값2 WHERE 조건;
```

### DELETE

```sql
DELETE FROM 테이블명 WHERE 조건;
```

#### 대용량의 테이블 전체 삭제

```sql
-- 세 SQL 모두 테이블 전체 삭제(DROP은 테이블까지 삭제)
delete from bigTbl1;
drop table bigTbl2;
truncate table bigTbl3;
```

DROP과 TRUNCATE는 빠르지만 DELETE는 DML 이여서 트랜잭션 로그를 기록하는 작업 때문에 느리다.

- TRUNCATE(DDL) : 레코드 단위가 아닌 테이블을 DROP하고 CREATE하는 방식으로 동작(auto_increament 값도 초기값으로 초기화), DDL 이기 때문에 트랜잭션 발생X
- DELETE는 속도는 느리지만 조건을 지정할 수 있다.

**결론 : 대용량의 테이블을 삭제해야 해야 할 때 테이블 자체가 필요 없을 경우에는 DROP, 테이블은 남겨놔야 할 때는 TRUNCATE를 사용하는 것이 효율적이다.**

### WITH절, CTE(Common Table Expression)

CTE는 쿼리문의 결과를 테이블처럼 사용할 수 있도록 하는 것으로 MySQL 8.0 이후부터 사용할 수 있다.

WITH 절은 CTE(Common Table Expression)을 표현하기 위한 구문이다.

#### 비재귀적 CTE

- CTE는 재귀적, 비재귀적 두가지 방법이 있다. 주로 사용되는 것은 비재귀적 CTE이다.

```sql
WITH cte_order(userId, total) 
AS 
(SELECT userid, SUM(price*amount) FROM order GROUP BY userId)
    
SELECT * FROM cte_order ORDER BY total DESC;
```

WITH 절의 AS 다음에 오는 쿼리문의 결과를 WITH 절 다음에 오는 이름과 컬럼의 테이블로 사용할 수 있다.

# Ch 07. SQL 고급

## MySQL 데이터 타입

### 정수형

| 타입명       | 바이트 | 범위                                  |
| ------------ | ------ | ------------------------------------- |
| BIT(n)       | N/8    | 1~64bit를 표현, b'0000' 형식으로 표현 |
| TINYINT      | 1      | -128 ~ 127                            |
| SMALLINT     | 2      | -32,768 ~ 32,767                      |
| MEDIUMINT    | 3      | -8,388,608 ~ 8,388,607                |
| INT, INTEGER | 4      | 약 ~21억 ~ 21억                       |
| BIGINT       | 8      | 약 -900경 ~ 900경                     |

부호 없는 정수를 사용하려면 UNSIGNED 예약어를 INT UNSIGEND와 같이 타입 뒤에 붙혀주면 된다.

실수형에도 UNSIGEND를 사용할 수는 있지만, 잘 사용되지 않는다.

### 실수형

| 타입명                           | 바이트 | 범위                                                         |
| -------------------------------- | ------ | ------------------------------------------------------------ |
| FLOAT                            | 4      | -3.40E+38 ~ 1.17E-38(소수점 아래 7자리까지 표현)             |
| DOUBLE, REAL                     | 8      | -1.22E-308 ~ 1.79E+308(소수점 아래 15자리까지 표현)          |
| DECIMAL(m, [d]), NUMERIC(m, [d]) | 5 ~ 17 | -10^38 ~ 10^38-1, 전체 자릿수 m과 소수점 이하 자릿수 d를 가진 숫자형 |

- **DECIMAL** : 정확한 수치를 저장, 실수 저장시 DECIMAL을 사용하는 것이 좋다.
- FLOAT, DOUBLE : DECIMAL과 달리 근사치를 저장하지만, 큰 숫자를 저장할 수 있다는 장점이 있다.

### 문자형

| 타입명                  | 바이트        | 설명                                       |
| ----------------------- | ------------- | ------------------------------------------ |
| CHAR(n)                 | 1 ~ 255       | 고정 길이 문자열, n은 1 ~ 255              |
| VARCHAR(n)              | 1 ~ 65535     | 가변 길이 문자열, n을 사용시 n은 1 ~ 65535 |
| BIANRY(n), VARBINARY(n) | 1 ~ 255       | 고정, 가변 길이 이진 데이터                |
| ENUM(값들 ...)          | 1 or 2        | 최대 65535개의 열거형 데이터               |
| SET(값들 ...)           | 1, 2, 3, 4, 8 | 최대 64개의 서로 다른 데이터2              |

MySQL의 char, varchar는 utf-8을 사용하여 영문이냐 한글이냐에 따라 내부적으로 크기가 달라지지만, 사용자 입장에선 신경쓸 필요가 없다. 예를 들어 CHAR(100) 이라고하면 영어, 한글 구분 없이 100글자를 입력할 수 있다.

### TEXT 형식

대용량의 문자를 저장하기위한 형식

| 타입명     | 바이트               |
| ---------- | -------------------- |
| TINYTEXT   | 1 ~ 255              |
| TEXT       | 1 ~ 65535            |
| MEDIUMTEXT | 1 ~ 16777215         |
| LONGTEXT   | 1 ~ 4294967295(4 GB) |

### BLOB 형식

사진, 동영상, 문서 등 대용량의 이진 데이터를 저장하는데 사용

MySQL은 LOB(Large Object)을 저장하기 위해 LONGTEXT, LONGBLOB 형식을 지원

| 타입명    | 바이트               |
| --------- | -------------------- |
| TINYBLOB  | 1 ~ 255              |
| BLOB      | 1 ~ 65535            |
| MEDIUMBLB | 1 ~ 16777215         |
| LONGBLOB  | 1 ~ 4294967295(4 GB) |

### 날짜와 시간 형식

| 타입명    | 바이트 | 설명                                                         |
| --------- | ------ | ------------------------------------------------------------ |
| DATE      | 3      | 'YYYY-MM-DD' 형식으로 사용                                   |
| TIME      | 3      | 'HH:MM:SS' 형식으로 사용                                     |
| DATETIME  | 8      | 'YYYY-MM-DD HH:MM:SS' 형식으로 사용                          |
| TIMESTAMP | 4      | 'YYYY-MM-DD HH:MM:SS' 형식으로 사용, time_zone 시스템 변수와 관련이 있으며 UTC 시간대에 변환하여 저장 |
| YEAR      | 1      | 'YYYY' 형식으로 저장                                         |

### 기타 형식

| 타입명   | 바이트 | 설명                                                         |
| -------- | ------ | ------------------------------------------------------------ |
| GEOMETRY | N/A    | 공간 데이터 형식. 선, 점, 다각형과 같은 공간 데이터 개체를 저장하고 조작 |
| JSON     | 8      | JSON 파일을 저장                                             |

## 변수 사용

```sql
SET @변수명 = 변수값; -- 변수 선언 및 값 대입
SELECT @변수명; -- 변수 값 출력
```

참고

- @변수명 → 전역 변수처럼 사용
- DECLARE 문으로 스토어드 프로시저, 함수 내에 선언한 변수 → 지역변수 처럼 사용

예제

```sql
SET @str = 'Hello World!';
SET @num1 = 3;
SET @num2 = 2.35;
SET @str2 = '사람 이름 ==> ';

SELECT @str;
SELECT @num1 + @num2;

SELECT @str2, name FROM member WHERE height > 175;
```

## 형 변환

- CAST(), CONVERT() 함수를 사용하여 형 변환을 할 수 있다.
- 사용 가능 데이터형 : BINARY, CHAR, DATE, DATETIME, TIME, DECIMAL, JSON, SINGNED INTEGER, UNSIGNED INTEGER 등

```sql
CAST(expression AS 데이터형[ (길이) ]);
CONVERT(expression, 데이터형 [ (길이) ]);
```

예제

```sql
-- 실수인 평균을 정수로 형 변환하여 출력
SELECT cast(AVG(price) AS SIGNED INTEGER) AS '평균 주문 가격' FROM order;

-- 다양한 구분자를 날짜, 시간 형식으로 형 변환 
SELECT CAST('2019%08%11' AS DATE);
SELECT CAST('2019/08/11' AS DATE);
SELECT CAST('2019$08$11' AS DATE);
SELECT CAST('2019@08%11#23-12/34' AS DATETIME);
```

### 암시적 형 변환

```sql
-- 암시적 형 변환 예

SELECT '5' + '15'; -- 20
SELECT CONCAT('5', '15'); -- '515'
SELECT CONCAT(5, 15); -- '515'
SELECT 1 = '1asd'; -- 1(true), 맨 앞 1이 숫자로 변환
SELECT 2 = 'asd2'; -- 0(false), 맨 앞이 문자인 경우 0으로 변환
```

## MySQL 내장 함수

### 제어 함수

#### IF(수식, 참, 거짓)

- 수식이 참이면 2번째 인자, 거짓이면 3번째 인자를 반환

```sql
SELECT IF(5 > 15, '참', '거짓'); -- 거짓 출력
```

#### IFNULL(수식1, 수식2)

- 수식1이 NULL이면 수식2 반환, 수식 1이 NULL이 아니면 수식1 반환

```sql
SELECT IFNULL(null, '수식1은 널!'); -- 수식은1은 널! 출력
```

#### NULLIF(수식1, 수식2)

- 수식1과 수식2의 값이 같으면 NULL 반환, 다르면 수식1의 값을 반환

```sql
SELECT NULLIF(5+5, 3+7); -- NULL 출력
```

#### CASE ~ WHEN ~ ELSE ~ END

```sql
SELECT CASE 5
		WHEN 1 THEN 'ONE'
    WHEN 3 THEN 'THREE'
    WHEN 5 THEN 'FIVE' -- FIVE 반환
    WHEN 10 THEN 'TEN'
    ELSE '?'
END AS 'CASE 예제'; -- 별칭
```

참고 : CASE는 내장함수가 아닌 연산자

### 문자열 함수

#### ASCII(아스키코드)

- 해당 문자의 아스키 코드 값을 반환

```sql
SELECT ASCII('A'); -- 65 출력
```

#### CHAR(숫자)

- 해당 숫자의 아스키 코드 값에 해당하는 문자 반환

```sql
SELECT CHAR(65); -- A 출력
```

#### BIT_LENGTH(문자열), CHAR_LENGTH(문자열), LENGTH(문자열)

- 각각 문자열의 BIT 크기 반환, 문자의 개수 반환, 바이트 수 반환

- MySQL은 UTF-8을 기본으로 사용하기 때문에 한 글자당 영어는 1Byte, 한글은 3Byte

```sql
SELECT BIT_LENGTH('Hello'); -- 40 반환
SELECT CHAR_LENGTH('Hello'); -- 5 반환 
SELECT LENGTH('Hello'); -- 5 반환

SELECT BIT_LENGTH('안녕'); -- 48 반환
SELECT CHAR_LENGTH('안녕'); -- 2 반환
SELECT LENGTH('안녕'); -- 6 반환
```

#### CONCAT(문자열1, 문자열2, ...), CONCAT_WS(구분자, 문자열1, 문자열2, ...)

- 문자열을 이어서 반환해준다.

- CONCAT_WS()는 문자열 사이에 구분자를 붙혀서 이어준다.

```sql
SELECT CONCAT('Hello ', 'World!'); -- Hello World! 출력
SELECT CONCAT_WS('-', 'Hello ', 'World!'); -- Hello-World! 출력
```

#### ELT(위치, 문자열1, 문자열2, ...)

- 위치 번째에 해당하는 문자열을 반환

- 위치 값이 문자열 개수 범위를 넘어서면 NULL 반환

```sql
SELECT ELT(2, 'kim', 'jeong', 'choi'); -- jeong 출력
```

#### FIELD(타겟 문자열, 문자열1, 문자열2, ...)

- 타겟 문자열의 위치를 반환

- 타겟 문자열과 같은 문자열이 없을 경우 0 반환

```sql
SELECT FIELD('C#', 'C++', 'JAVA', 'C#'); -- 3 출력
SELECT FIELD('C', 'C++', 'JAVA', 'C#'); -- 0 출력
```

#### FIND_IN_SET(타겟 문자열, 문자열 리스트)

- 문자열 리스트에서 타겟 문자열을 찾아 위치를 반환

- 리스트에 타겟 문자열이 없는 경우 0반환

```sql
SELECT FIND_IN_SET('c', 'a,b,c,d,e,f,g'); -- 3 출력
```

#### INSTR(기준 문자열, 부분 문자열), LOCATE(부분 문자열, 기준 문자열), POSITION(부분 문자열 IN 기준 문자열)

- 기준 문자열에서 부분 문자열을 찾아 시작 위치를 반환.

- INSTR(), LOCATE(), POSITION()은 파라미터 순서 등만 다를 뿐 같은 기능을 한다.

- 기준 문자열에 부분 문자열이 없는 경우 0 반환.

```sql
SELECT INSTR('I love MySQL', 'sql'); -- 10 출력
SELECT LOCATE('sql', 'I love MySQL'); -- 10 출력
SELECT POSITION('sql' IN 'I love MySQL'); -- 10 출력
```

#### FORMAT(숫자, 소수점 자리수)

- 숫자의 소수점을 인자로 전달된 자리수까지 반올림하여 반환.

- 1000단위마다 , 를 찍어준다.

```sql
SELECT FORMAT(123456.123456, 4); -- 123,456.12345 출력
```

#### BIN(숫자), HEX(숫자), OCT(숫자)

- 숫자를 2진수, 16진수, 8진수로 변환하여 반환

```sql
SELECT BIN(10), HEX(10), OCT(10); -- 1010, A, 12 출력
```

#### INSERT(기준 문자열, 위치, 길이, 삽입할 문자열)

- 기준 문자열의 위치부터 길이만큼의 문자열을 잘라내고 삽입할 문자열을 삽입하여 반환

```sql
-- 안Hi요 출력
SELECT INSERT('안녕하세요', 2, 3, 'Hi');
```

#### LEFT(문자열, 길이), RIGHT(문자열, 길이)

- 문자열의 왼쪽 또는 오른쪽부터 길이 만큼을 잘라서 반환

```sql
--- abc, ef 출력
SELECT LEFT('abcdef', 3), RIGHT('abcdef', 2);
```

#### UPPER(문자열), LOWER(문자열)

- 소문자를 대문자로, 대문자를 소문자로 변환하여 반환

LOWER()와 LCASE(), UPPER()와 UCASE()는 같은 기능을 하는 함수

```sql
-- HELLO WORLD, hello world 출력
SELECT UPPER('Hello World'), LOWER('HELLO WORLD'); 
SELECT UCASE('Hello World'), LCASE('HELLO WORLD');
```

#### LPAD(문자열, 길이, 채울 문자열), RPAD(문자열, 길이, 채울 문자열)

- 문자열을 길이만큼 늘리고, LPAD는 문자열의 왼쪽, RPAD는 문자열의 오른쪽에  채울 문자열을 채워서 반환

```sql
-- ##MySQL, MySQL##### 출력
SELECT LPAD('MySQL', 7, '##'), RPAD('MySQL', 10, '##'); 
```

#### TRIM(문자열)

- LTRIM(문자열), RTRIM(문자열) : 문자열의 왼쪽, 오른쪽 공백을 제거
- TRIM(문자열) : 문자열 양 옆의 공백을 제거
- TRIM(방향 자를_문자열 FROM 문자열) : 방향은 LEADING(앞), BOTH(양쪽), TRAILING(뒤)를 지정할 수 있으며, 문자열의 주어진 방향에 있는 자를_문자열을 제거

```sql
-- abc 반환
SELECT TRIM('  abc  '), LTRIM('  abc'), RTRIM('abc   ');
SELECT TRIM(BOTH '-' FROM '----abc----');
```

#### REPEAT(문자열, 횟수)

- 문자열을 횟수만큼 반복하여 반환 

```sql
-- ababab 출력
SELECT REPEAT('ab', 3);
```

#### REPLACE(문자열, 원래 문자열, 바꿀 문자열)

- 문자열에서 원래 문자열을 바꿀 문자열로 바꿔준다.

```sql
-- Java 스프링 MySQL 출력
SELECT REPLACE('자바 스프링 MySQL', '자바', 'Java');
```

#### REVERSE(문자열)

- 문자열 순서를 거꾸로

```sql
-- 'LQSyM' 출력
SELECT REVERSE('MySQL');
```

#### SPACE(길이)

- 길이만큼의 공백을 반환

```sql
-- 'Hello     World!' 반환
SELECT CONCAT('Hello', SPACE(5), 'World!');
```

#### SUBSTRING(문자열, 시작위치[, 길이]), SUBSTRING(문자열 FROM 시작위치[ FOR 길이])

- 시작 위치부터 길이만큼 문자열 반환, 길이 생략시 시작 위치부터 문자열 끝까지 반환

- SUBSTRING(), SUBSTR(), MID()는 동일 함수

```sql
-- '안녕' 출력
SELECT SUBSTRING('안녕하세요', 1, 2);

-- '하세' 출력
SELECT SUBSTRING('안녕하세요' FROM 3 FOR 2);
```

#### SUBSTRING_INDEX(문자열, 구분자, 횟수)

- 문자열 왼쪽부터 구분자가 횟수 번째 나오면 나머지 오른쪽 문자열을 버린다.

- 횟수가 음수면 오른쪽부터 세고 왼쪽을 버린다.

```sql
-- 'www.google' 출력
SELECT SUBSTRING_INDEX('www.google.com', '.', 2);

-- 'google.com' 출력
SELECT SUBSTRING_INDEX('www.google.com', '.', -2);
```

### 수학 함수

#### 절대 값

```sql
-- 10 출력
SELECT ABS(-10);
```

#### 삼각함수

ACOS(숫자), ASIN(숫자), ATAN(숫자), ATAN2(숫자1, 숫자2), SIN(숫자), COS(숫자), TAN(숫자)

#### 올림, 내림, 반올림

```sql
SELECT CEILING(5.2); -- 6, 올림
SELECT FLOOR(5.6); -- 5, 내림
SELECT ROUND(4.7); -- 5, 반올림
```

#### 진수 변환

- 숫자를 원래 진수에서 변환할 진수로 변환

```sql
-- 16진수 'AA'를 2진수로 변환
SELECT CONV('AA', 16, 2);
```

#### DEGREES(숫자), RADIAN(숫자), PI()

- DEGREES(숫자) : 라디안 값을 각도 값으로 변환
- RADIAN(숫자) : 각도 값을 라디안 값으로 변환
- PI() : 파이값 3.141592 반환

#### 지수, 로그 관련 함수

- EXP(X), LN(숫자), LOG(숫자), LOG(밑수, 숫자), LOG2(숫자), LOG10(숫자)

####  나머지 구하는 함수

- MOD(숫자1, 숫자2), 숫자1 % 숫자2, 숫자1 MOD 숫자2

```sql
SELECT MOD(3, 2); -- 1
```

#### 거듭 제곱 구하는 함수

- POW(숫자1, 숫자) , POWER(숫자1, 숫자2)

```sql
SELECT POW(2, 3); -- 8
```

#### 제곱근 구하는 함수

- SQRT(숫자)

```sql
SELECT SQRT(9); -- 루트9
```

#### RAND()

- 0 이상 1 미만의 랜덤 실수 값 반환

```sql
-- m 이상 n 미만의 랜덤 정수 구하기
SELECT FLOOR(m + RAND()*(n-m));
```

#### SIGN(숫자)

- 숫자가 음수인지 양수인지 판단

- 양수면 1, 0이면 0, 음수면 -1 반환

#### TRUNCATE(숫자, 정수)

- 숫자를 소수점을 기준으로 정수 위치까지 구하고 나머지 버린다.

```sql
SELECT TRUNCATE(12345.12345, 2); -- 12345.12
SELECT TRUNCATE(12345.12345, -2); -- 12300
```

### 날짜 및 시간 함수

#### ADDDATE(날짜, 차이), SUBDATE(날짜, 차이)

- 날짜를 기준으로 더하거나 뺸다.

- ADDDATE() == DATE_ADD() , SUBDATE() == DATE_SUB()

```sql
SELECT ADDDATE('2020-01-01', INTERVAL 31 DAY); -- 2020-02-01
SELECT ADDDATE('2020-01-01', INTERVAL 2 MONTH); -- 2020-03-01
SELECT ADDDATE('2020-01-01', INTERVAL 1 YEAR); -- 2021-01-01

SELECT SUBDATE('2020-01-01', INTERVAL 31 DAY); -- 2019-12-01
SELECT SUBDATE('2020-01-01', INTERVAL 2 MONTH); -- 2019-11-01
SELECT SUBDATE('2020-01-01', INTERVAL 1 YEAR); -- 2019-01-01
```

#### ADDTIME(날짜/시간, 시간), SUBTIME(날짜/시간, 시간)

- 날짜/시간을 기준으로 시간을 더하거나 뺸다.

```sql
SELECT ADDTIME('2020-01-01 00:00:00', '10:10:10'); -- 2020-01-01 10:10:10
SELECT ADDTIME('00:00:00', '10:10:10'); -- 10:10:10

SELECT SUBTIME('2020-01-01 00:00:00', '10:10:10'); -- 2019-12-31 13:49:50
SELECT SUBTIME('15:30:30', '10:10:10'); -- 05:20:20
```

#### CURDATE(), CURTIME(), NOW(), SYSDATE()

- CURDATE() : 현재 '년-월-일' 반환
- CURTIME() : 현재 '시:분:초' 반환
- NOW(), SYSDATE() : 현재 '년-월-일 시:분:초' 반환

CURDATE() == CURRENT_DATE() == CURRENT_DATE

CURTIME() == CURRENT_TIME() == CURRENT_TIME

NOW() == LOCALTIME == LOCALTIME() == LOCALTIMESTAMP == LOCALTIMESTAMP()

#### YEAR(날짜), MONTH(날짜), DAY(날짜)

- 날짜에서 년, 월, 일을 반환한다.

- DAY() == DAYOFMONTH()

```sql
-- 2020 | 12 | 25
SELECT YEAR('2020-12-25'), MONTH('2020-12-25'), DAY('2020-12-25'); 
```

#### HOUR(시간), MINUTE(시간), SECOND(시간), MICROSECOND(시간)

- 시간에서, 시, 분, 초, 밀리초를 반환한다.

```sql
SELECT HOUR('23:24:25'), MINUTE('23:24:25'), SECOND('23:24:25'); -- 23 | 24 | 25
```

#### DATE(), TIME()

- DATETIME 형식에서 '연-월-일', '시:분:초'추출해서 반환한다.

```sql
SELECT DATE('2020-12-31 12:12:12'); -- 2020-12-31
SELECT TIME('2020-12-31 12:12:12'); -- 12:12:12
```

#### DATEIDFF(날짜1, 날짜2), TIMEDIFF(시간1, 시간2)

- 날짜1 - 날짜2의 일수를 반환

시간1- 시간2의 시간을 반환

```sql
SELECT DATEDIFF('2021-02-15', '2021-01-01'); -- 45
SELECT TIMEDIFF('23:59:59', '15:00:00'); -- 08:59:59
```

#### DAYOFWEEK(날짜), MONTHNAME(날짜), DAYOFYEAR(날짜)

```sql
-- 현재 요일을 반환(일:1, 월~토 : 2~7)
SELECT DAYOFWEEK(CURDATE()); 

-- 현재 월 이름(예: December) 출력
SELECT MONTHNAME(CURDATE());

-- 현재 날짜가 1년 중 몇번째 날인지 출력
SELECT DAYOFYEAR(CURDATE());
```

#### LAST_DAY(날짜)

- 날짜에 해당하는 년, 월의 마지막 날짜를 반환

```sql
-- 2021-2-28 출력
SELECT LAST_DAY('2021-2-10');
```

#### MAKEDATE(연도, 정수)

- 연도에서 정수만큼 지난 날짜를 반환

```sql
-- 2021-02-01 출력
SELECT MAKEDATE(2021, 32);
```

#### MAKETIME(시, 분, 초)

- 시, 분, 초로 '시:분:초' TIME 형식을 만들어서 반환

```sql
-- 23:10:15 출력
SELECT MAKETIME(23, 10, 15);
```

#### PERIOD_ADD(연월, 개월수), PERIOD_DIFF(연월1, 연월2)

- PERIOD_ADD(연월, 개월수) : 연월에서 개월수 만큼 지난 연월을 반환
- PERIOD_DIFF(연월1, 연월2) : 연월1 - 연월2의 개월수를 반환

연월은 YYYY 또는 YYYYMM 형식을 사용

```sql
-- 202108 출력
SELECT PERIOD_ADD(202105, 3);

-- 6 출력
SELECT PERIOD_DIFF(202105, 202011);
```

#### QUARTER(날짜)

- 4분기 중 날짜가 몇 분기에 해당하는지 반환

```sql
-- 2 출력
SELECT QUARTER('2021-04-01');
```

#### TIME_TO_SEC(시간)

- 시간을 초 단위로 변환하여 반환

```sql
-- 54605 반환
SELECT TIME_TO_SEC('15:10:5');
```

### 시스템 정보 함수

#### USER(), DATABASE()

- 현재 사용자 및 선택된 데이터베이스 반환

- USER() == SESSION_USER() == CURRENT_USER()

- DATABASE() == SCHEMA()

#### FOUND_ROWS()

- 바로 앞의 SELECT 문에서 조회된 행의 개수를 반환

```sql
SELECT * FROM MEMBER;

-- MEMBER 테이블의 행 개수 반환
SELECT FOUND_ROWS(); 
```

#### ROW_COUNT()

- 바로 앞의 INSERT, UPDATE, DELETE 문에서 삽입, 수정, 삭제가 일어난 행의 개수를 반환

- CREATE, DROP 문은 0을 반환, SELECT 문은 -1 반환

#### VERSION()

- MySMQL의 현재 버전을 반환

#### SLEEP(초)

- 쿼리의 실행일 초 동안 잠시 멈춘다.

### JSON 데이터

#### 테이블 데이터 → JSON 데이터

- 테이블 데이터를 JSON 데이터로 변환하려면 JSON_OBJECT(), JSON_ARRAY() 함수르 사용하면 된다.

```sql
SELECT JSON_OBJECT('name', name, 'age', age) AS '회원 JSON 값' FROM member;
```

#### 그 외 JSON 관렴 함수

```sql
-- @json 변수에 member라는 이름의 테이블로 JSON 데이터 우선 대입
SET @json = '{ "member" :
	[
		{"name":"Kim", "height":182},
		{"name":"Lee", "height":172},
		{"name":"Jeong", "height":186}
	]
}';

-- 문자열이 JSON 형식을 만족하면 1, 아니면 0 반환
SELECT JSON_VALID(@json) as JSON_VALID;

/*
첫번째 파라미터는 JSON 문자열
세번째 파라미터에 입력한 문자열의 위치를 반환
두번째 파라미터는 'one' 또는 'all'이 올 수 있으며, 'one'인 경우 처음 매치되는 하나를 반환, 'all' 인 경우 매치되는 모든 것을 반환
결과 : "$.member[1].name" -> 문자열 lee는 0번째의 name에서 처음 발견
*/
SELECT JSON_SEARCH(@json, 'one', 'lee');

/*
JSON_SEARCH와 반대로 지정된 위치의 값을 반환
결과 : "Kim"
*/
SELECT JSON_EXTRACT(@json, '$.member[0].name');

/*
새로운 값을 추가
member의 0번째 데이터에 "address":"서울 특별시" 를 추가
*/
SELECT JSON_INSERT(@json, '$.member[0].address', '서울 특별시');

/*
값을 변경
member의 1번째 데이터의 name 을 'Park'으로 변경
*/
SELECT JSON_REPLACE(@json, '$.member[1].name', 'Park');

/*
삭제
member의 2번째 데이터를 삭제
*/
SELECT JSON_REMOVE(@json, '$.member[2]');
```

## 조인(Join)

INNER JOIN, OUTER JOIN, FULL JOIN 정리는 생략

### CROSS JOIN

CROSS JOIN은 양쪽 테이블의 모든 행을 JOIN 시킨다. 그래서 CROSS JOIN 결과의 행 개수는 두 테이블 행 개수를 곱한 것과 같다.

CROSS JOIN에는 ON 구문을 사용할 수 없다.

```sql
SELECT * FROM buyTbl CROSS JOIN userTbl;
```

### SELF JOIN

SELF JOIN은 별도의 구문이 있는 것은 아니며, 단지 자기 자신과 조인을 한다는 것이다.

```sql
/*
emp 테이블 예
|emp |empTel|manager|
---------------------
|김대리|0000  | 이부장 |
--------------------- 
|이부장|0001  | 김재무 | 
---------------------
사원 테이블엔 모든 직급 관계없이 모든 사원들의 이름이 있다.
사원 테이블의 manager 열에는 직속 상관의 이름이 있다.
직속상관도 결국 사원이기 때문에 사원 테이블의 하나의 행이다.
아래의 쿼리문은 셀프 조인을 사용해서 김대리의 직속 상관의 정보를 조회하는 문이다.
결과 : 김대리 이부장 0001
*/
SELECT 
	A.emp AS '부하직원', B.emp AS '직속상관', B.empTel AS '직속상관연락처' 
	FROM empTbl A INNER JOIN empTbl B ON A.manager = B.emp WHERE A.emp = '김대리';
```

### UNION / UNION ALL

UNION은 두 쿼리의 결과를 행으로 합치는 것을 말한다. 

두 쿼리의 결과가 서로 열의 개수가 같아야하며, 각 열의 데이터 타입이 호환이 가능해야한다.

UNION 결과 열의 이름은 앞 쪽 쿼리문의 열 이름을 사용한다.

UNION은 중복된 열은 제거되어 조회되고, UNION ALL은 중복된 열까지 모두 조회된다.

### IN / NOT IN

NOT IN은 첫번째 쿼리의 결과 중에서, 두번째 쿼리에 해당하는 것을 제외하기 위한 구문이다.

```sql
-- 모든 멤버의 이름과 폰 번호를 조회하는데 폰 번호가 없는 사람은 제외하고 조회
SELECT name, phoneNum FROM member 
	where name NOT IN(SELECT name FROM member where phoneNum IS NULL);
```

IN은 NOT IN과 반대로 첫번쨰 쿼리 결과 중에서, 두번쨰 쿼리에 해당되는 것만 조회하기 위한 구문이다.

```sql
-- 모든 멤버의 이름과 폰 번호를 조회하는데 폰 번호가 없는 사람만 조회
SELECT name, phoneNum FROM member 
	where name IN(SELECT name FROM member where phoneNum IS NULL);
```

## MySQL 프로그래밍
MySQL도 Java, C++ 등과 같은 프로그래밍 언어와 비슷하게 분기, 흐름 제어, 반복의 기능이 있다.
### IF...ELSE
```sql
DROP PROCEDURE IF EXISTS ifProc2;
use employees;
DELIMITER $$
CREATE PROCEDURE ifProc()
	BEGIN
	/*
	사용자 정의 변수를 만들때 @를 앞에 붙혀야한다 했지만,
	스토어드 프로시저나 함수에서는 DELCLARE문을 사용해서 지역변수를 선언하고 ,
	@을 붙히지 않고 일반 프로그래밍 언어의 변수처럼 그냥 사용하면된다.
	*/
	DECLARE hireDATE DATE; -- 입사일
	DECLARE curDATE DATE; -- 오늘
	DECLARE days INT; -- 근무 일수

	-- SELECT 열이름 INTO 변수명 FROM 테이블 : 조회된 열의 결과를 변수에 대입한다.
	SELECT hire_date INTO hireDATE -- hire_date열의 결과를 hireDATE에 대입
	FROM employees.employees
	WHERE emp_no = 10001;

	SET curDATE = CURRENT_DATE();
	SET days = DATEDIFF(curDate, hireDate);

	IF(days/365) >= 5 THEN
		SELECT CONCAT('입사한지 ', 5, '년이 지났습니다. 축하합니다!');
	ELSEIF (days/365) >= 1 THEN
		SELECT CONCAT('입사한지 ', 1, '년이 지났습니다. 축하합니다!');
	ELSE
		SELECT '입사한지 ' + days + '일 밖에 안되었네요. 열심히 일하세요.';
	END IF;
END $$
DELIMITER ;

CALL ifProc();
```
### CASE
```sql
DROP PROCEDURE IF EXISTS caseProc;
DELIMITER $$
CREATE PROCEDURE caseProc()
BEGIN
	DECLARE point INT;
	DECLARE credit CHAR(1);
	SET point = 77;

	CASE
		WHEN point >= 90 THEN
			SET credit = 'A';
		WHEN point >= 80 THEN
			SET credit = 'B';
		WHEN point >= 70 THEN
			SET credit = 'C';
		WHEN point >= 60 THEN
			SET credit = 'D';
		ELSE
			SET credit = 'F';
	END CASE;

SELECT CONCAT('점수 : ', point, ', 학점 : ', credit);

END $$
DELIMITER ;

CALL caseProc();
```
CASE 문은 SELECT 절에서 더 많이 사용된다.
```sql
SELECT u.userID, u.name, SUM(price*amount) AS '총 구매액',
	CASE
		WHEN (SUM(price*amount) >= 1500) THEN '최우수고객'
		WHEN (SUM(price*amount) >= 1000) THEN '우수고객'
		WHEN (SUM(price*amount) >= 1) THEN '일반고객'
		ELSE '유령고객'
	END AS '고객등급'
FROM buytbl b RIGHT OUTER JOIN usertbl u ON b.userId = u.userId
GROUP BY u.userID, u.name ORDER BY sum(price*amount) DESC;
```
### WHILE, ITERATE/LEAVE
INTERATE는 CONTINUE문과 비슷하다.
LEAVE는 BREAK문과 비슷하다.

형식
```sql
WHILE <부울 식> DO
	SQL 명령문들
END WHILE;
```

예제
```sql
-- 1~100합을 구하는데 7의 배수는 제외하고, 합이 1000을 초과하면 종료
DROP PROCEDURE IF EXISTS whileProc;
DELIMITER $$
CREATE PROCEDURE whileProc()
BEGIN
	DECLARE i INT;
	DEClARE sum INT;
	SET i = 1;
	SET sum = 0;

	myWhile:WHILE (i <= 100) DO
		IF(i%7 = 0) THEN
			SET i = i + 1;
			ITERATE myWhile; -- myWhile 레이블로 가서 계속 진행
		END IF;

		SET sum = sum + i;

		IF(sum > 1000) THEN
			LEAVE myWhile; -- myWhile 레이블을 떠남, 즉 WHILE문 종료
		END IF;

		SET i = i+1;
	END WHILE;

	SELECT sum;
END $$
DELIMITER ;
CALL whileProc();
```
### 오류 처리
오류가 발생했을 때, 직업 오류를 처리할 수 있는 방법을 제공한다.
```sql
-- 형식
DECLARE 액션 HANDLER FOR 오류조건 처리할_문장;
```
- 액션 : 오류 발생 시의 행동, CONTINUE 또는 EXIT 둘 중에 하나 사용
	- CONTINUE : '처리할_문장' 부분이 실행된다.
- 오류조건 : 어떤 오류를 처리할 것인지 지정
	- 오류 코드 숫자
	- SQLSTATE '상태코드' -> 상태코드는 5자리 문자열
	- SQLEXCEPTION -> 대부분의 오류
	- SQLWARNING -> 경고 메시지
	- NOT FOUND -> 커서, SELECT...INTO에서 발생되는 오류
	- 등
- 처리할_문장 : 처리할 문장이 여러 개인 경우 BEGIN...END로 묶어줄 수 있다.
```sql
-- 예제1
DROP PROCEDURE IF EXISTS errorProc;
DELIMITER $$
CREATE PROCEDURE errorProc()
BEGIN
	-- 오류코드 1146은 테이블이 없을 경우 발생한다.
	DECLARE CONTINUE HANDLER FOR 1146 SELECT '테이블이 없습니다.' AS '오류 메시지';

	SELECT * FROM noTable; -- noTable은 없는 테이블
END $$
DELIMITER ;
CALL errorProc();

-- 예제2
DROP PROCEDURE IF EXISTS errorProc2;
DELIMITER $$
CREATE PROCEDURE errorProc2()
BEGIN
	DECLARE CONTINUE HANDLER FOR SQLEXCEPTION
	BEGIN
		SHOW ERRORS; -- 오류에 대한 코드와 메시지 출력
		SHOW COUNT(*) ERRORS; -- 발생된 오류의 개수 출력
		SHOW WARNINGS; -- 경고에 대한 코드와 메시지 출력
		ROLLBACK;
	END;
	... 
END $$
DELIMITER ;
CALL errorProc2();
```
### 동적 SQL
쿼리문을 바로 실행하지 않고, 준비만 해놓고 나중에 실행하는 것을 '동적 SQL'이라고한다.
```sql
-- SQL문을 바로 실행하지 않고 준비만 해놓는다.
PREPARE myQuery FROM 'SELECT * FROM member WHERE memberId = "abc"';

-- 준비한 쿼리문을 실행
EXECUTE myQuery;

-- SQL문 해제
DEALLOCATE PREPARE myQuery;
```
PREPARE문에서 향후 입력될 값을 ?로 만들어놓고, EXECUTE문에서 USING을 사용해서 변수의 값을 전달받아 사용할 수도 있다.
```sql
SET @memberId = 'abc';
PREPARE myQuery FROM 'SELECT * FROM memberWHERE userId = ?';
EXECUTE myQuery USING @memberId;
DEALLOCATE PREPARE myQuery;
```
# 테이블 
참고 : auto_increment 지정한 열은 primary_key 또는 unique_key로 반드시 지정해야된다.

## 제약 조건
제약조건이란 데이터의 무결성을 지키기 위한 제한된 조건을 의미한다.
- 무결성: 데이터의 정보가 변경되거나 오염되지 않도록하는 원칙  

어떠한 데이터를 입력할 때, 특정 조건을 만족했을 때에만 입력이 되도록 제약할 수 있다.
MySQL은 아래와 같은 제약조건을 제공한다.
- PRIMARY KEY 제약조건
- FOREIGN KEY 제약조건
- UNIQUE 제약조건
- CHECK 제약조건(8.0.16버전부터 정식 지원)
- DEFAULT 정의
- NULL 값 허용

### 기본 키 제약 조건
- **기본키는 값이 중복될 수 없으며, NULL이어서는 안된다.**
- 기본키로 생성한 것은 자동으로 클러스터형 인덱스가 생성된다.
- 기본키는 하나 이상의 열에 설정할 수 있다.
- 모든 제약조건은 이름을 가지는데, CREATE TABLE 구문 내에서 기본키를 설정하면 제약 조건의 이름을 MySQL이 자동으로 지어준다.

참고 : SHOW KEYS FROM 테이블 이름 -> 테이블에 지정된 키 보기

기본키 제약 조건의 이름을 따로 지정해주는 방법은 다음과 같다.
```sql
-- 테이블 생성시에 지정
CREATE TABLE member ( 
	userID CHAR(8) NOT NULL,
    name VARCHAR(10) NOT NULL,
    birthYear INT NOT NULL,
	-- CONSTRAINT는 생략 가능, PK_memberL_userID 는 제약조건 이름
    CONSTRAINT PRIMARY KEY PK_member_userID (userID)
);

-- 이미 만들어진 테이블을 수정하여 지정
ALTER TABLE member ADD CONSTRAINT PK_member_userID PRIMARY KEY (userID);
```

### 외래 키 제약 조건
- 외래 키 제약조건은 두 테이블 사이의 관계를 선언함으로써 데이터의 무결성을 보장해주는 역할을 하며, 외래 키 관계를 설정하면 하나의 테이블이 다른 테이블에 의존하게 된다.
- **외래 키 테이블에 데이터를 입력할 때는 기준 테이블을 참조해서 입력하므로 기준 테이블에 이미 데이터가 존재해야한다.**
- 외래 키 테이블이 참조하는 기준 테이블의 열은 반드시 Primary Key 또는 Unique 제약 조건이 설정되어 있어야한다.
```sql
CREATE TABLE member ( 
	userID CHAR(8) NOT NULL PRIMARY KEY,
    name VARCHAR(10) NOT NULL,
    birthDate DATE NOT NULL
);

CREATE TABLE orders (
	id int auto_increment not null primary key,
    memberID char(8) not null,
    itemName char(6) not null,
	-- 이름 지정할 필요 없으면 CONSTRAINT FK_member_orders 생략하면 된다.
	-- 기준 테이블 열 이름과 외래 키 테이블의 열이름이 달라도 된다.
    CONSTRAINT FK_member_orders FOREIGN KEY(memberID) REFERENCES member(userID)
);

-- ALTER TABLE 구문을 사용하여 외래키 지정
ALTER TABLE orders 
	ADD CONSTRAINT FK_member_orders 
	FOREIGN KEY (memberID) 
	REFERENCES member(memberID);
```
외래 키의 옵션중 ON DELETE CASCADE 또는 ON UPDATE CASCADE를 사용하면 기준 테이블의 데이터가 변경 되었을 떄, 외래 키 테이블에도 자동으로 적용되도록 할 수 있다.

- ON DELETE CASCADE : 기준 테이블 데이터 삭제시 외래 키 테이블의 관련된 데이터도 함께 삭제
- ON UPDATE CASCADE : 기준 테이블의 키 값 변경시 외래 키 테이블의 외래 키 값도 함께 변경  

별도로 옵션을 지정하지 않으면 ON DELETE NO ACTION, ON UPDATE NO ACTION을 지정한 것과 같다.
```sql
-- 외래 키 제거
ALTER TABLE orders DROP CONSTRAINT FK_member_orders;

-- ON UPDATE CASCADE 옵션 사용 예
ALTER TABLE orders 
	ADD CONSTRAINT FK_member_orders 
    FOREIGN KEY (memberID) 
    REFERENCES member(memberID) 
    ON UPDATE CASCADE;
```
외래 키 컬럼에 기준 테이블에 없는 값을 입력하려하면 외래 키 제약 조건을 만족하지 않는다는 에러가 발생한다. 하지만 어떤 경우 외래 키 테이블의 데이터를 먼저 입력해야 할수도 있다. 그런 경우 외래키 제약 조건을 잠시 비활성화 시키는 방법은 다음과 같다.
```sql
SET foreign_key_checks = 0 -- 외래 키 제약 조건 검사 비활성화
SET foreign_key_checks = 1 -- 외래 키 제약 조건 검사 홯성화
```

생성된 외래키 조회
- information_schema 데이터베이스의 referntial_constraints 테이블을 조회하면 생성된 외래키들을 조회할 수 있다.
```sql
-- sqldb 데이터베이스에서 외래 키가 있는 테이블의 이름과 외래 키 이름 조회
SELECT table_name, constraint_name
	FROM information_schema.referential_constraints
	WHERE constraint_schema = 'sqldb';
```

### UNIQUE 제약 조건
- **UNIQUE 제약 조건은 NULL 값은 허용하지만 중복되지 않는 유일한 값을 입력해야 한다는 조건이다.**

UNIQUE 제약 조건을 추가하는 방법은 다음과 같다.
```sql
-- 1. 컬럼을 정의하면서 지정
CREATE TABLE member (
	userID CHAR(8) NOT NULL PRIMARY KEY,
    eamil CHAR(30) NULL UNIQUE
);

-- 2. 컬럼 정의 후 지정
CREATE TABLE member (
	userID CHAR(8) NOT NULL PRIMARY KEY,
    eamil CHAR(30) NULL,
    -- AK는 Alternate Key의 약자로 Unique는 Alternate Key로도 불린다.
    CONSTRAINT AK_email UNIQUE (email)
);
```
참고 : 제약 조건의 이름은 일반적으로 PRIMARY KEY는 PK, FOREIGN KEY는 FK, Uique는 AK를 주로 사용한다. AK는 Alternate Key의 약자로 Unique는 Alternate Key로도 불린다.

### CHECK 제약 조건
CHECK 제약 조건은 입력되는 데이터가 지정한 조건에 맞게 입력되었는지 점검하는 기능을 한다.
```sql
CREATE TABLE member (
	id INT PRIMARY KEY,
    name VARCHAR(10),
    -- birthYear는 1900 이상, 2023 이하의 값이어야 한다.
    birthYear INT CHECK (birthYear >= 1900 AND birthYear <= 2023),
    mobile1 CHAR(3) NULL,
    -- mobile1은 011, 019, 010 중 하나의 값이어야 한다.
    CONSTRAINT CK_mobile1 CHECK (mobile1 IN ('011', '019', '010'))
);

-- ALTER TABLE 문으로 CHECK 제약 조건을 추가할 수도 있다.
ALTER TABLE member ADD CONSTRAINT CK_name CHECK (name IS NOT NULL);
```
CHECK 제약 조건을 추가하지만, 작동하지 않도록 하려면 제약 조건 뒤에 `NOT ENFORCED` 구문을 추가하면 되지만 거의 사용되지 않는다.  
참고 : MySQL에서는 CHECK 제약 조건이 설정된 컬럼을 삭제 시 그냥 삭제 되지만, 다른 DBMS에서는 CHECK 제약 조건이 설정된 열은 삭제되지 않는다.

### DEFAULT 정의
**DEFAULT는 값을 입력하지 않았을 때, 자동으로 입력되는 디폴트 값을 정의하는 방법이다.**

DEFAULT를 설정하는 방법은 다음과 같다.
```sql
-- 1. 테이블 생성 시에 설정
CREATE TABLE member (
	id INT NOT NULL PRIMARY KEY,
    name varchar(10) NOT NULL DEFAULT '익명 사용자',
    height SMALLINT
);

-- 2. ALTER TABLE 문을 사용하여 설정
ALTER TABLE member ALTER COLUMN height SET DEFAULT 170;
```

DEFAULT가 설정된 열에 DEFAULT 값을 입력하는 방법은 다음과 같다.
```sql
-- default(소문자)는 DEFAULT로 설정된 값을 입력해준다.
INSERT INTO member VALUES (1, default, default);

-- 열 이름을 명시하지 않으면 DEFAULT로 설정된 값을 입력해준다.
INSERT INTO member(id) VALUES(2);
```
### NULL 값 허용
- NULL은 NULL 허용, NOT NULL은 NULL을 허용하지 않는다.
- PRIMARY KEY가 설정된 열은 어차피 NULL이 올 수 없기 떄문에 NOT NULL을 생략해도 된다.
- NULL 저장 시 고정 길이 문자열 타입 CHAR는 공간을 모두 차지하지만, 가변 길이 문자열 타입 VARCHAR는 공간을 차지하지 않는다. 그렇기 때문에 NULL이 많이 입력되는 컬럼이라면 가변 길이 문자열 타입을 사용하는 것이 좋다.

## 테이블 압축
테이블 압축 기능은 대용량 테이블의 공간을 절약한다.
MySQL 5.0 버전부터 제공하기 시작했으며, MySQL 8.0 버전부터 내부적인 기능이 더욱 강화되어 MySQL이 허용하는 최대 용량의 데이터도 오류없이 압축이 잘 동작한다.
```sql
CREATE TABLE member(
	id BIGINT, 
    name VARCHAR(10)
) ROW_FORMAT=COMPRESSED; -- 압축 설정
```
동일한 컬럼과 로우를 가진 압축 테이블, 일반 테이블 두 개의 테이블을 만들고 `SHOW TABLE STATUS DB명` 쿼리를 사용하여 테이블의 상태를 보면 압축 테이블의 평균 행 길이(Avg_row_length)나 데이터 길이(Data_length)가 훨씬 작은 것을 볼 수 있다(데이터 값의 분포에 따라 압축률은 달라질 수 있다).

## 임시 테이블
```sql
-- 형식 : TABLE 위치에 TEPORARY TABLE로 써주기만 하면 된다.
CREATE TEMPORARY TABLE [IF NOT EXISTS] 테이블명 (
	-- 컬럼 정의
);
```
임시 테이블은 이름 그대로 임시로 사용되는 테이블이다.
- 임시 테이블은 세션 내에서만 존재하며, 세션 종료시 자동 삭제된다.
- 임시 테이블은 생성한 클라이언트에서만 접근할 수 있다.
- 임시 테이블은 데이터베이스 내의 다른 테이블과 같은 이름으로 만들 수 있지만, 이 경우 임시 테이블이 삭제될 때까지 기존 테이블에는 접근할 수 없고 임시 테이블에만 접근할 수 있다.

임시 테이블이 삭제되는 시점은 다음과 같다.
- DROP TABLE로 직접 삭제 시
- MySQL 클라이언트 종료 시
- MySQL 서비스가 재시작될 떄

## 테이블 삭제
```sql
DROP TABLE 테이블명;

-- 여러 테이블 동시에 삭제 가능
DROP TABLE 테이블1, 테이블2, ...;
```
- 외래 키 제약 조건의 기준 테이블은 바로 삭제할수 없고, 먼저 외래 키가 생성된 외래 키 테이블을 삭제해야 삭제 할 수 있다.

## 테이블 수정 
이미 생성된 테이블의 추가/변경/삭제는 `ALTER TABLE` 구문을 사용한다.

### 컬럼 추가
```sql
ALTER TABLE member ADD nickname VARCHAR(20) DEFAULT 'member' NULL;
```
SQL문의 맨 뒤에 `FIRST` 또는 `AFTER 컬럼명`을 사용하여 컬럼이 추가될 위치를 지정할 수 있다.
- `FIRST` : 테이블의 맨 앞에 컬럼이 추가된다.
- `AFTER 컬럼명` : 해당 컬럼의 다음에 컬럼이 추가된다.

### 컬럼 삭제
```sql
ALTER TABLE member DROP COLUMN nickname;
```

### 컬럼 이름 및 데이터 타입 변경
```sql
/*
member 테이블의 name 컬럼의 이름을 mName으로,
데이터 타입을 VARCHAR(20), NULL 값 허용으로 변경한다.
*/
ALTER TABLE member CHANGE COLUMN name mName VARCHAR(20) NULL;
```

### 컬럼의 제약 조건 삭제
```sql
ALTER TABLE member DROP PRIMARY KEY;
ALTER TABLE order DROP FOREIGN KEY FK_member_order;
```
## 뷰(View)
뷰는 SELECT 문의 결과로 만들어지는 가상 테이블이다.

### 뷰의 장점
- 보안 : 원 테이블에는 접근할 수 없도록 제한하고, 필요한 내용만 뷰로 만들어서 뷰에만 접근 권한을 준다.
- 복잡한 쿼리를 단순화 : 복잡한 쿼리문을 뷰로 만들어서 간편하게 조회할 수 있다.

### 뷰 생성 : CREATE VIEW
```sql
-- 뷰 생성
CREATE view v_userbuytbl
AS
	SELECT u.userid AS 'USER ID', u.name AS 'USER NAME', b.prodName AS 'PRODUCT NAME', u.addr, concat(u.mobile1, u.mobile2) AS 'MOBILE PHONE'
    FROM usertbl u INNER JOIN buytbl b ON u.userid = b.userid;

-- 뷰 조회 : 일반 테이블과 똑같이 조회하면 된다.
SELECT `user id`, `user name` FROM v_usertbl; -- 띄어쓰기 때문에 `` 사용
```    

### 뷰 수정 : ALTER VIEW
```sql
ALTER VIEW v_userbuytbl
AS 
	SELECT u.userid AS '사용자 아이디', u.name AS '사용자 이름', b.prodName AS '제품 이름', u.addr, concat(u.mobile1, u.mobile2) AS '전화 번호'
    FROM usertbl u INNER JOIN buytbl b ON u.userid = b.userid;
```

### 뷰 덮어쓰기 : CREATE OR REPLACE
CREATE VIEW는 기존에 뷰가 있으면 오류가 발생하지만, CREATE OR REPLACE VIEW를 사용하면 기존에 뷰가 있어도 덮어쓰기 때문에 오류가 발생하지 않는다.

DROP VIEW와 CREATE VIEW를 연속으로 사용한 효과

```sql
CREATE OR REPLACE view v_usertbl
AS
	SELECT userid, name, addr FROM usertbl;
```

### 뷰를 통한 데이터 삽입/수정/삭제 가능
기본적으로 뷰는 읽기 전용으로 사용하는 것이 좋지만, 뷰의 데이터를 변경할 수 있으며 원 테이블의 데이터가 함께 변경된다. 

예외적으로 뷰를 통해 데이터의 수정이나 삭제를 할 수 없는 경우는 다음과 같다.
- 집계 함수를 사용한 뷰
- UNION ALL, JOIN 등을 사용한 뷰
- DISTINCT, GROUP BY 등을 사용한 뷰

뷰를 통해 데이터를 삽입할 때, 원테이블에 NOT NULL로 설정된 열이 뷰에는 없는 경우 해당 컬럼이 DEFAULT가 지정되어 있어야 정상적으로 데이터를 삽입할 수 있다.
```sql
-- 뷰를 통한 데이터 삽입
INSERT INTO v_usertbl VALUES('PHS', '박효신', '서울');

-- 뷰를 통한 데이터 수정
UPDATE v_usertbl SET addr = '부산' WHERE userid='JKW';

-- 뷰를 통한 데이터 삭제
DELETE FROM v_usertbl WHERE userid='JKW';
```

### 뷰 조건에 맞는 데이터만 삽입하기 : WITH CHECK OPTION
예를 들어 아래와 같이 사용자 중 키가 170 이상인 사람들을 대상으로 뷰를 만든다고 가정해보자.
```sql
CREATE VIEW v_height170
AS 
	SELECT * FROM usertbl WHERE height >= 170;
```
여기서 뷰의 조건에 맞지 않는 키가 158인 데이터를 뷰를 통해 삽입하면 어떻게 될까?
```sql
INSERT INTO v_height170 VALUES('KBM', '김병만', 1977, '경기', '010', '5555555', 158, '2021-10-10');
```
정상적으로 데이터가 삽입은 되지만 뷰에선 조회되지 않고, 원테이블을 조회하면 해당 데이터를 볼 수 있다.  
이와 같은 경우 뷰에 조건에 맞는 데이터만 뷰를 통해 삽입하고 싶다면, WITH CHECK OPTION문을 사용하면 된다.
```sql
CREATE VIEW v_height170
AS 
	SELECT * FROM usertbl WHERE height >= 170 WITH CHECK OPTION;
```

### 뷰 삭제 : DROP VIEW
```sql
DROP VIEW v_usertbl;
```

## 테이블 스페이스
데이터베이스가 테이블이 저장되는 논리적인 공간이라면, 테이블 스페이스는 테이블이 실제로 저장되는 물리적인 공간이다.  
대용량의 데이터를 다룰 떄는 성능 향상을 위해 테이블스페이스에 대한 설정을 하는 것이 좋다.  
따로 테이블 스페이스를 지정하지 않으면 시스템 테이블스페이스에 테이블이 저장되며, 시스템 테이블 스페이스에 대한 정보는 시스템 변수 innodb_data_file_path에 저장되어 있다.

```sql
-- 결과 : 파일명:파일크기:최대파일크기
SHOW VARIABLES LIKE 'innodb_data_file_path';
```

각 테이블을 별도의 테이블 스페이스에 저장하려면 시스템 변수 inno_db_file_per_table가 ON 으로 설정되어 있어야한다(MySQL 8.0에서는 디폴트 값이 ON)

```sql
-- 해당 변수가 ON 이어야 테이블을 별도의 테이블 스페이스에 저장 가능
SHOW VARIABLES LIKE 'innodb_file_per_table';

-- 테이블 스페이스 생성, 테이블 스페이스의 확장자는 idb이어야 한다.
CREATE TABLESPACE ts_a ADD DATAFILE 'ts_a.ibd';
CREATE TABLESPACE ts_b ADD DATAFILE 'ts_b.ibd';

-- 테이블 생성시 테이블 스페이스 지정
CREATE TABLE member(id INT, name VARCHAR(10)) TABLESPACE ts_a;

-- ALTER 문으로 테이믈 스페이스 변경 가능
ALTER TABLE member TABLESPACE ts_b;
```
# 인덱스
책에서 특정 단어를 찾을때 책의 처음부터 찾는 것보다 책 뒤에 있는 찾아보기를 보면 해당 단어가 있는 페이지를 빠르게 찾을 수 있다. 인덱스는 이와 같이 데이터를 좀 더 빠르게 찾을 수 있도록 도와주는 도구라고 할 수 있다.

## 인덱스 장단점
인덱스는 튜닝에 즉각적인 효과를 내는 가장 빠른 방법 중 하나이다. 인덱스를 사용하면 기존보다 빠른 응답속도를 얻을 수 있고, 서버 입장에서도 더 적은 처리량으로 요청한 결과를 얻게 된다.

장점
- 검색(Select)은 속도가 무척 빨리질 수 있다.(단, 항상 그런 것은 아니다)
- 그 겨로가 해당 쿼리의 부하가 줄어들어, 결국 시스템 성능이 향상된다.

단점  
- 인덱스도 데이터베이스의 공간을 차지하기 떄문에 추가 공간이 필요(대략 데이터베이스 크기의 10%)
- 처음 인덱스를 생성할 때 시간이 많이 소요될 수 있다.
- 데이터 변경 작업(Insert, Update, Delete) 작업이 자주 일어나느 경우 오히려 성능이 나빠질 수 있다.

### 데이터베이스 튜닝  
튜닝이란 SQL 서버가 기존보다 더 좋은 성능을 내도록 하는 전반적인 방법론을 말한다. 튜닝은 크게 응답시간 단축과 서버 부하량 최소화가 있다.

응답 시간 단축
- 사용자가 쿼리문을 실행하면 얼마나 빨리 결과를 얻는가의 문제
- 응답 시간이 단축되면 사용자 입장에선 좋은 것 이지만, 서버 입장에서는 기존보다 작업량이 늘어나는 경우가 발생할수도 있다. 
- 이 경우 한 명의 사용자에게는 결과가 빨리 나오겠지만, 시스템 전체로 봤을때 성능은 오히려 나빠질 수도 있다.

서버 부하량 최소화
- 사용자 한 명 한 명의 응답시간보다는 서버가 처리하는 총 작업량을 줄임으로써 시스템의 전반적인 성능을 향상시키는 것

사용자의 응답답속도가 빨리지는 것은 서버에서 처리할 양이 줄어들어서 빨라지는 경우도 있지만, 아닌 경우도 있기 때문에 주의해야 한다.

## 인덱스 주의사항
예를 들어 데이터베이스 책에서 데이터베이스라는 단어를 찾는 다고 생각해보자. 데이터베이스 책에서 데이터베이스라는 단어는 정말 많이 나올 것이기 때문에 찾아보기와 책 사이를 엄청 왔다갔다 하게 되고, 찾아보기 페이지의 수도 엄청 늘어날 것이다. 이 경우 차라리 그냥 책을 처음부터 찾는 것이 효율적이고 빠를 수도 있다.
이렇듯 실제 데이터베이스에서도 필요 없는 인덱스를 만들어 사용하게되면 데이베이스 공간만 차지하고, 전체 테이블 스캔보다 속도가 느려진다.

참고 : 실제로는 MySQL이 인덱스를 사용하는 것이 효율적일지 테이블 전체 스캔이 효율적일지 판단해주기는 하지만, 쓸데 없는 인덱스를 만들어서 
발생하는 문제점은 많다.

**제약 조건 설정 시 주의사항**  
많은 데이터가 입력된 후에 ALTER 문으로 PRIMARY KEY 또는 UNIQUE를 지정하면 인덱스를 구성하는데 많은 시간이 소요될 수 있기 때문에 주의해야한다.

## 인덱스의 종류
MySQL에서 인덱스는 크게 클러스터형 인덱스, 보조 인덱스 2가지로 나뉜다.  
클러스터형 인덱스  
- 사전과 같이 책의 내용 자체가 순서대로 정렬되어 있는 것
- 테이블당 1개만 생성 가능

보조 인덱스  
- 책 뒷부분과 같은 찾아보기가 별도로 있고, 찾아 보기를 찾아본 후에 그 옆에 표시된 페이지로 가야 실제 페이지가 있는 것
- 테이블당 여러개 생성 가능

Unique Index, Nonunique 인덱스로 나눌수도 있다.
- Unique Index 
	- 값들이 서로 중복되지 않는 인덱스
	- Primary key 또는 Unique key로 지정하면 Unique Index가 생성된다.
- Nonunique Index 
	- 값들이 중복되는 인덱스
	- Primary key 또는 Unique key로 지정되지 않은 열에 인덱스를 설정하면 생성된다.

## 자동 생성되는 인덱스
인덱스는 컬럼 단위에 생성이 되며, 테이블 생성 시에 제약 조건 PRIMARY KEY 또는 UNIQUE를 사용하면 자동으로 인덱스가 생성된다.
- 테이블 생성시 PRIMARY KEY로 지정하면 자동으로 클러스터형 인덱스가 생성된다.
- 테이블 생성시 UNIQUE로 지정하면 보조 인덱스가 자동으로 생성된다.
- PRIMARY KEY가 없는 테이블에서 UNIQUE NOT NULL로 지정하면 클러스터형 인덱스로 자동 생성이된다.
- UNIQUE NOT NULL이어도 PRIMARY KEY가 있으면 PRIMARY KEY가 우선적으로 클러스터형 인덱스로 설정되고 UNIQUE는 보조 인덱스로 자동 생성된다. -> 클러스터형 인덱스는 테이블당 1개이기 때문에
- PRIMARY KEY로 지정한 열은 데이터가 오름차순 정렬된다. -> 클러스터형 인덱스는 행 데이터를 자신의 열을 기준으로 정렬한다.

인덱스 상태 확인 : SHOW INDEX FORM 테이블명;
```sql
SHOW INDEX FROM member;
```
결과 컬럼 설명
- Non_uniqu : 0이면 Unique 인덱스, 1이면 Nonunique 인덱스
- Key_name : index_name과 같은 의미로 인덱스 이름
- Seq_in_index : 해당 열에 여러 개의 인덱스가 설정되었을 때 순서
- Null : NULL 값 허용 여부
- Index_type : 어떤 형태로 인덱스가 구성되었는지(MySQL은 기본적으로 B-Tree)

## 인덱스 내부 작동
- MySQL은 인덱스를 구현할 때 B-Tree 자료구조를 사용한다.
- MySQL에선 페이지는 16KB 크기의 최소한의 저장 단위이다. 아무리 작은 크기의 데이터를 1개만 저장해도 적어도 16KB는 차지한다는 것이다.(DBMS마다 페이지 크기 다름) 
- 인덱스에서 트리의 노드에 해당하는 것을 페이지라고 부른다.

### 페이지 분할
인덱스를 사용하면 데이터 변경(INSERT, UPDATE, DELETE) 시에 '페이지 분할' 작업이 발생하기 떄문에 성능이 나빠지는 단점이 있다.(특히 INSERT 작업시) 

페이지 분할이란 데이터 추가시에 데이터가 정렬상 알맞은 자리를 찾는 과정에서 B-Tree의 페이지가 분할되는 것을 말한다.(자세한 과정은 책 참고)

### 클러스터형 인덱스와 보조 인덱스 구조
(구조에 대한 자세한 그림은 책 388p~ 참고)

#### 클러스터형 인덱스
- 클러스터형 인덱스 생성 시에 데이터 페이지 전체가 정렬된다. 그렇기 때문에 대용량 데이터가 있는 경우 시간이 오래 걸릴 수 있다.
- 클러스터형 인덱스는 인덱스 자체의 리프 페이지가 곧 데이터이다. -> 인덱스 자체에 데이터가 포함되어 있다고 볼 수 있다.
- 클러스터형 인덱스의 검색 속도는 보조 인덱스보다 대부분 더 빠르지만, 삽입/삭제/수정은 더 느리다.
- 클러스터형 인덱스는 테이블당 1개의 열에만 생성할 수 있다.

#### 보조 인덱스
- 보조 인덱스는 데이터 페이지를 건드리지 않고, 별도의 장소에 페이지를 생성한다.
- 보조 인덱스의 리프 페이지는 데이터가 아니라 데이터가 위치하는 주소값(RID, 페이지 번호 + #오프셋)이다. 
- 보조 인덱스는 데이터 페이지를 정렬하는 것이 아니라 데이터 페이지의 뒤쪽 빈 부분에 삽입된다.
- 클러스터형 인덱스보다 검색 속도는 느리지만 삽입/수정/삭제 속도는 더 빠르다.
- 보조 인덱스는 한 테이블에 여러 개를 생성할 수 있지만, 남용할 경우 오히려 시스템 성능을 떨어트릴 수 있다.

### 클러스터형 인덱스와 보조 인덱스를 같이 사용할 경우
클러스터형 인덱스와 보조 인덱스를 같이 사용할 경우 클러스터형 인덱스는 달라지는 것이 없지만, 보조 인덱스는 달라지는 점이 있다.

클러스터형 인덱스가 없다면 보조 인덱스의 리프 페이지는 데이터 페이지의 주소값이다. 하지만 클러스터형 인덱스와 같이 사용하게되면 클러스터형 인덱스의 키 값을 가지게된다. 그리고 이 키 값을 가지고 클러스터형 인덱스의 루트 페이지부터 해당 키 값을 검색해서 결과 값을 얻는다.

예를 들어 아래 SELECT 쿼리의 실행 과정은 다음과 같다.
```sql	
CREATE TABLE member (
	id INT PRIMARY KEY,
	name VARCHAR(10) UNIQUE
);

INSERT INTO member VALUES (1, '김이름');
...

SELECT * FROM member WHERE name = '김이름';
``` 
1. 보조 인덱스의 루트 페이지부터 시작해 리프 페이지까지 내려가서 '김이름'을 찾는다.
2. 리프 페이지의 '김이름'은 값으로 클러스터형 인덱스의 키값 1을 가지고 있다.
3. 클러스터형 인덱스의 루트 페이지부터 리프 페이지(데이터 페이지)까지 키 값 1을 찾아 내려가서 해당 데이터를 반환 한다.

그렇다면 클러스터형 인덱스와 보조 인덱스를 같이 사용할 때 굳이 보조 인덱스의 동작 방식을 변경하는 이유는 무엇일까?

기존처럼 보조 인덱스의 리프 페이지에 '페이지 번호 + #오프셋'으로 사용하여 클러스터형 인덱스와 분리하여 구성한다면 검색에서는 더 좋은 성능을 보여줄 것이다.

만약 보조 인덱스의 리프 페이지가 기존처럼 데이터 페이지의 주소값으로 되어있는데 데이터 삽입 시에 클러스터형 인덱스에 페이지 분할이 발생하면 클러스터형 인덱스의 리프 페이지(데이터 페이지)의 변경 즉, 데이터들의 위치(주소 값)가 바뀌기 때문에 인해 보조 인덱스의 많은 부분을 변경해야한다. 이러한 치명적인 단점 때문에 클러스터형 인덱스와 보조 인덱스를 같이 사용할 경우 보조 인덱스의 동작 방식을 다르게 사용하는 것이다.

혼합 인덱스에서 인덱스 지정 시 고려사항
- 클러스터형 인덱스로 지정할 열의 자릿수가 크다면 보조 인덱스에 저장되어야할 양도 증가한다. 주의하자.

**인덱스를 생성하기 위한 일차 조건은 WHERE 절에 해당 인덱스를 생성한 열의 이름을 사용하는 것이다. 하지만 WHERE 절에 해당 인덱스를 생성한 열의 이름을 사용해도 인덱스를 사용하지 않는 경우도 많다.**

## 인덱스 생성/변경/삭제

### 인덱스 생성 - CREATE INDEX
```sql
-- 문법
CREATE [UNIQUE | FULLTEXT | SPATIAL] INDEX index_name
	[index_type]
	ON tbl_name (key_part, ...)
	[index_option]
	[algorithm_option | lock_option] ...

	key_part:
		{col_name [(length)] | (expr)} [ASC | DESC]
	
	index_option:
		KEY_BLCK_SIZE [=] value
		| index_type
		| WITH PARSER parser_name
		| COMMENT 'string'
		| {VISIBLE | INVISIBLE}

	index_type:
		USING {BTREE | HASH}

	algorithm_option:
		ALGORITHM [=] {DEFAULT | INPLACE | COPY}

	lock_option:
		LOCK [=] {DEFAULT | NONE | SHARED | EXCLUSIVE}

```
- CREATE FULLTEXT INDEX 문은 전체 텍스트 인덱스를 만든다.
- CREATE SPATIAL INDEX 문은 점, 선, 면 등의 공간 데이터와 관련된 인덱스를 만든다.
- CREATE INDEX문으로 Primary Key로 생성되는 클러스터형 인덱스를 만들 수 없다. 클러스터형 인덱스를 생성하려면 ALTER TABLE 문을 사용해야 한다.
- UNIQUE로 지정된 인덱스는 동일한 데이터 값이 입력될 수 없다.(디폴트 값은 UNIQUE X)
- CREATE INDEX 문으로 생성되는 인덱스는 보조 인덱스이다.
- ASC(디폴트 값), DESC는 인덱스가 정렬되는 방식이다.
- index_type을 생략하면 B-TREE 형식을 사용한다.

### 인덱스 제거 - DROP INDEX
```sql
-- 문법
DROP INDEX index_name ON tbl_name
	[algorithm_option | lock_option] ...

algorithm_option:
	ALGORITHM [=] {DEFAULT | INPLACE | COPY}

lock_option:
	LOCK [=] {DEFAULT | NONE | SHARED | EXCLUSIVE}
```
- 기본 키로 설정된 클러스터형 인덱스의 이름은 항상 'PRIMARY'로 되어있기 때문에 인덱스의 이름 부분에 PRIMARY를 써주거나, ALTER TABLE 문으로 기본키를 제거하여 클러스터형 인덱스를 삭제할 수 있다.
- 인덱스를 모두 제거할 때는 보조 인덱스부터 삭제하는 것이 좋다. 보조 인덱스의 리프 페이지들의 값은 클러스터형 인덱스의 키 값을 저장하고 있는데 클러스터형 인덱스가 삭제되면 다시 '페이지 번호 + #오프셋'으로 값이 모두 재구성 된다. 만약 클러스터형 인덱스부터 삭제한다면 불필요한 보조 인덱스 재구성 작업까지 하게 되는 것이다.
- 활용도가 떨어지는 인덱스는 삭제하자. 전반적인 MySQL 성능 저하를 발생시킬 수 있다.

인덱스 관련 예시 코드
```sql
-- 인덱스 조회
SHOW INDEX FROM usertbl;

/*
인덱스 크기 확인
(최소 단위가 1페이지이고, 1페이지가 16KB이기 때문에 최소 16KB이다)
- Data_length : 클러스터형 인덱스(또는 데이터)의 크기
- Index_length :  보조 인덱스의 크기
*/
SHOW TABLE STATUS LIKE 'usertbl';

-- 보조 인덱스 생성
CREATE INDEX idx_usertbl_name ON usertbl (name);

-- 인덱스를 생성 후 실제 적용시키려면 ANALYZE TABLE문으로 먼저 테이블을 분석/처리해줘야 한다.
ANALYZE TABLE usertbl;

-- 두개의 열을 조합하여 인덱스 생성 예 
CREATE INDEX idx_usertbl_name_birthYear 
	ON usertbl (name, birthYear);

-- idx_usertbl_name_birthYear 인덱스로 설정한 두 열이 조합된 조건문의 쿼리를 실행하여 MySQL Workbench에서 실행 계획을 확인해보면 해당 인덱스가 사용된 것을 볼 수 있다.
SELECT * FROM usertbl WHERE name = '윤종신' AND birthYear = '1969';

-- name 인덱스와 name+birthYear 인덱스 두개가 있으면 아래와 같이 name만 사용하여 조회하는 경우엔 name 인덱스를 사용한다.
-- -> MySQL이 알아서 효율적인 인덱스를 선택해준다.
SELECT * FROM usertbl WHERE name = '윤종신';

-- 인덱스 삭제
DROP INDEX idx_usertbl_name ON usertbl;

-- ALTER TABLE 문을 사용하여 인덱스 삭제
ALTER TABLE usertbl DROP INDEX idx_usertbl_name;

-- 클러스터형 인덱스 삭제
ALTER TABLE usretbl DROP PRIMARY KEY;
```

