- [Ch 04. 테이터베이스 모델링](#ch-04-테이터베이스-모델링)
	- [MySQL Workbench 모델링 툴 실습](#mysql-workbench-모델링-툴-실습)
- [Ch 06. SQL 기본](#ch-06-sql-기본)
	- [select](#select)
	- [between A and B](#between-a-and-b)
	- [in()](#in)
	- [문자열 검색 : like](#문자열-검색--like)
	- [ANY/ALL/SOME 그리고 서브쿼리](#anyallsome-그리고-서브쿼리)
	- [ORDER BY](#order-by)
	- [DISTINCT - 중복 제거](#distinct---중복-제거)
	- [LIMIT - 출력 개수 제한](#limit---출력-개수-제한)
	- [CREATE TABLE ... SELECT - 테이블 복사](#create-table--select---테이블-복사)
	- [GROUP BY, HAVING, 집계함수](#group-by-having-집계함수)
		- [집계 함수](#집계-함수)
		- [Having 절](#having-절)
		- [ROLLUP](#rollup)
	- [SQL의 분류](#sql의-분류)
		- [DML(Data Manipulation Language)](#dmldata-manipulation-language)
		- [DDL(Data Definition Language)](#ddldata-definition-language)
		- [DCL(Data Control Language)](#dcldata-control-language)
		- [데이터 변경 SQL 문](#데이터-변경-sql-문)
		- [INSERT](#insert)
			- [INSERT INTO ... SELECT](#insert-into--select)
			- [INSERT IGNORE - PK 중복 무시](#insert-ignore---pk-중복-무시)
			- [ON DUPLICATE KEY UPDATE - PK 중복시 UPDATE](#on-duplicate-key-update---pk-중복시-update)
		- [AUTO_INCREMENT](#auto_increment)
		- [UPDATE](#update)
		- [DELETE](#delete)
			- [대용량의 테이블 전체 삭제](#대용량의-테이블-전체-삭제)
		- [WITH절, CTE(Common Table Expression)](#with절-ctecommon-table-expression)
			- [비재귀적 CTE](#비재귀적-cte)
- [Ch 07. SQL 고급](#ch-07-sql-고급)
	- [MySQL 데이터 타입](#mysql-데이터-타입)
		- [정수형](#정수형)
		- [실수형](#실수형)
		- [문자형](#문자형)
		- [TEXT 형식](#text-형식)
		- [BLOB 형식](#blob-형식)
		- [날짜와 시간 형식](#날짜와-시간-형식)
		- [기타 형식](#기타-형식)
	- [변수 사용](#변수-사용)
	- [형 변환](#형-변환)
		- [암시적 형 변환](#암시적-형-변환)
	- [MySQL 내장 함수](#mysql-내장-함수)
		- [제어 함수](#제어-함수)
			- [IF(수식, 참, 거짓)](#if수식-참-거짓)
			- [IFNULL(수식1, 수식2)](#ifnull수식1-수식2)
			- [NULLIF(수식1, 수식2)](#nullif수식1-수식2)
			- [CASE ~ WHEN ~ ELSE ~ END](#case--when--else--end)
		- [문자열 함수](#문자열-함수)
			- [ASCII(아스키코드)](#ascii아스키코드)
			- [CHAR(숫자)](#char숫자)
			- [BIT_LENGTH(문자열), CHAR_LENGTH(문자열), LENGTH(문자열)](#bit_length문자열-char_length문자열-length문자열)
			- [CONCAT(문자열1, 문자열2, ...), CONCAT_WS(구분자, 문자열1, 문자열2, ...)](#concat문자열1-문자열2--concat_ws구분자-문자열1-문자열2-)
			- [ELT(위치, 문자열1, 문자열2, ...)](#elt위치-문자열1-문자열2-)
			- [FIELD(타겟 문자열, 문자열1, 문자열2, ...)](#field타겟-문자열-문자열1-문자열2-)
			- [FIND_IN_SET(타겟 문자열, 문자열 리스트)](#find_in_set타겟-문자열-문자열-리스트)
			- [INSTR(기준 문자열, 부분 문자열), LOCATE(부분 문자열, 기준 문자열), POSITION(부분 문자열 IN 기준 문자열)](#instr기준-문자열-부분-문자열-locate부분-문자열-기준-문자열-position부분-문자열-in-기준-문자열)
			- [FORMAT(숫자, 소수점 자리수)](#format숫자-소수점-자리수)
			- [BIN(숫자), HEX(숫자), OCT(숫자)](#bin숫자-hex숫자-oct숫자)
			- [INSERT(기준 문자열, 위치, 길이, 삽입할 문자열)](#insert기준-문자열-위치-길이-삽입할-문자열)
			- [LEFT(문자열, 길이), RIGHT(문자열, 길이)](#left문자열-길이-right문자열-길이)
			- [UPPER(문자열), LOWER(문자열)](#upper문자열-lower문자열)
			- [LPAD(문자열, 길이, 채울 문자열), RPAD(문자열, 길이, 채울 문자열)](#lpad문자열-길이-채울-문자열-rpad문자열-길이-채울-문자열)
			- [TRIM(문자열)](#trim문자열)
			- [REPEAT(문자열, 횟수)](#repeat문자열-횟수)
			- [REPLACE(문자열, 원래 문자열, 바꿀 문자열)](#replace문자열-원래-문자열-바꿀-문자열)
			- [REVERSE(문자열)](#reverse문자열)
			- [SPACE(길이)](#space길이)
			- [SUBSTRING(문자열, 시작위치[, 길이]), SUBSTRING(문자열 FROM 시작위치[ FOR 길이])](#substring문자열-시작위치-길이-substring문자열-from-시작위치-for-길이)
			- [SUBSTRING_INDEX(문자열, 구분자, 횟수)](#substring_index문자열-구분자-횟수)
		- [수학 함수](#수학-함수)
			- [절대 값](#절대-값)
			- [삼각함수](#삼각함수)
			- [올림, 내림, 반올림](#올림-내림-반올림)
			- [진수 변환](#진수-변환)
			- [DEGREES(숫자), RADIAN(숫자), PI()](#degrees숫자-radian숫자-pi)
			- [지수, 로그 관련 함수](#지수-로그-관련-함수)
			- [나머지 구하는 함수](#나머지-구하는-함수)
			- [거듭 제곱 구하는 함수](#거듭-제곱-구하는-함수)
			- [제곱근 구하는 함수](#제곱근-구하는-함수)
			- [RAND()](#rand)
			- [SIGN(숫자)](#sign숫자)
			- [TRUNCATE(숫자, 정수)](#truncate숫자-정수)
		- [날짜 및 시간 함수](#날짜-및-시간-함수)
			- [ADDDATE(날짜, 차이), SUBDATE(날짜, 차이)](#adddate날짜-차이-subdate날짜-차이)
			- [ADDTIME(날짜/시간, 시간), SUBTIME(날짜/시간, 시간)](#addtime날짜시간-시간-subtime날짜시간-시간)
			- [CURDATE(), CURTIME(), NOW(), SYSDATE()](#curdate-curtime-now-sysdate)
			- [YEAR(날짜), MONTH(날짜), DAY(날짜)](#year날짜-month날짜-day날짜)
			- [HOUR(시간), MINUTE(시간), SECOND(시간), MICROSECOND(시간)](#hour시간-minute시간-second시간-microsecond시간)
			- [DATE(), TIME()](#date-time)
			- [DATEIDFF(날짜1, 날짜2), TIMEDIFF(시간1, 시간2)](#dateidff날짜1-날짜2-timediff시간1-시간2)
			- [DAYOFWEEK(날짜), MONTHNAME(날짜), DAYOFYEAR(날짜)](#dayofweek날짜-monthname날짜-dayofyear날짜)
			- [LAST_DAY(날짜)](#last_day날짜)
			- [MAKEDATE(연도, 정수)](#makedate연도-정수)
			- [MAKETIME(시, 분, 초)](#maketime시-분-초)
			- [PERIOD_ADD(연월, 개월수), PERIOD_DIFF(연월1, 연월2)](#period_add연월-개월수-period_diff연월1-연월2)
			- [QUARTER(날짜)](#quarter날짜)
			- [TIME_TO_SEC(시간)](#time_to_sec시간)
		- [시스템 정보 함수](#시스템-정보-함수)
			- [USER(), DATABASE()](#user-database)
			- [FOUND_ROWS()](#found_rows)
			- [ROW_COUNT()](#row_count)
			- [VERSION()](#version)
			- [SLEEP(초)](#sleep초)
		- [JSON 데이터](#json-데이터)
			- [테이블 데이터 → JSON 데이터](#테이블-데이터--json-데이터)
			- [그 외 JSON 관렴 함수](#그-외-json-관렴-함수)
	- [조인(Join)](#조인join)
		- [CROSS JOIN](#cross-join)
		- [SELF JOIN](#self-join)
		- [UNION / UNION ALL](#union--union-all)
		- [IN / NOT IN](#in--not-in)
	- [MySQL 프로그래밍](#mysql-프로그래밍)
		- [IF...ELSE](#ifelse)
		- [CASE](#case)
		- [WHILE, ITERATE/LEAVE](#while-iterateleave)
		- [오류 처리](#오류-처리)
		- [동적 SQL](#동적-sql)
- [테이블](#테이블)
	- [제약 조건](#제약-조건)
		- [기본 키 제약 조건](#기본-키-제약-조건)
		- [외래 키 제약 조건](#외래-키-제약-조건)
		- [UNIQUE 제약 조건](#unique-제약-조건)
		- [CHECK 제약 조건](#check-제약-조건)
		- [DEFAULT 정의](#default-정의)
		- [NULL 값 허용](#null-값-허용)
	- [테이블 압축](#테이블-압축)
	- [임시 테이블](#임시-테이블)
	- [테이블 삭제](#테이블-삭제)
	- [테이블 수정](#테이블-수정)
		- [컬럼 추가](#컬럼-추가)
		- [컬럼 삭제](#컬럼-삭제)
		- [컬럼 이름 및 데이터 타입 변경](#컬럼-이름-및-데이터-타입-변경)
		- [컬럼의 제약 조건 삭제](#컬럼의-제약-조건-삭제)
	- [뷰(View)](#뷰view)
		- [뷰의 장점](#뷰의-장점)
		- [뷰 생성 : CREATE VIEW](#뷰-생성--create-view)
		- [뷰 수정 : ALTER VIEW](#뷰-수정--alter-view)
		- [뷰 덮어쓰기 : CREATE OR REPLACE](#뷰-덮어쓰기--create-or-replace)
		- [뷰를 통한 데이터 삽입/수정/삭제 가능](#뷰를-통한-데이터-삽입수정삭제-가능)
		- [뷰 조건에 맞는 데이터만 삽입하기 : WITH CHECK OPTION](#뷰-조건에-맞는-데이터만-삽입하기--with-check-option)
		- [뷰 삭제 : DROP VIEW](#뷰-삭제--drop-view)
	- [테이블 스페이스](#테이블-스페이스)
- [인덱스](#인덱스)
	- [인덱스 장단점](#인덱스-장단점)
		- [데이터베이스 튜닝](#데이터베이스-튜닝)
	- [인덱스 주의사항](#인덱스-주의사항)
	- [인덱스의 종류](#인덱스의-종류)
	- [자동 생성되는 인덱스](#자동-생성되는-인덱스)
	- [인덱스 내부 작동](#인덱스-내부-작동)
		- [페이지 분할](#페이지-분할)
		- [클러스터형 인덱스와 보조 인덱스 구조](#클러스터형-인덱스와-보조-인덱스-구조)
			- [클러스터형 인덱스](#클러스터형-인덱스)
			- [보조 인덱스](#보조-인덱스)
		- [클러스터형 인덱스와 보조 인덱스를 같이 사용할 경우](#클러스터형-인덱스와-보조-인덱스를-같이-사용할-경우)
	- [인덱스 생성/변경/삭제](#인덱스-생성변경삭제)
		- [인덱스 생성 - CREATE INDEX](#인덱스-생성---create-index)
		- [인덱스 제거 - DROP INDEX](#인덱스-제거---drop-index)
	- [인덱스 성능 비교](#인덱스-성능-비교)
	- [결론 : 인덱스를 생성해야 하는 경우, 그렇지 않은 경우](#결론--인덱스를-생성해야-하는-경우-그렇지-않은-경우)
- [스토어드 프로그램](#스토어드-프로그램)
	- [스토어드 프로시저](#스토어드-프로시저)
		- [스토어드 프로시저 변경과 삭제](#스토어드-프로시저-변경과-삭제)
		- [매개변수의 사용](#매개변수의-사용)
			- [입력 매개변수](#입력-매개변수)
			- [출력 매개변수](#출력-매개변수)
		- [스토어드 프로시저 프로그래밍](#스토어드-프로시저-프로그래밍)
		- [기타 프로시저 관련](#기타-프로시저-관련)
			- [저장된 프로시저의 이름 및 내용 조회](#저장된-프로시저의-이름-및-내용-조회)
			- [테이블 이름을 파라미터로 전달하는 방법](#테이블-이름을-파라미터로-전달하는-방법)
		- [스토어드 프로시저의 특징](#스토어드-프로시저의-특징)
			- [MySQL 성능의 향상](#mysql-성능의-향상)
			- [유지관리의 간편함](#유지관리의-간편함)
			- [모듈식 프로그래밍](#모듈식-프로그래밍)
			- [보안 강화](#보안-강화)
	- [스토어드 함수](#스토어드-함수)
	- [커서](#커서)
	- [트리거](#트리거)
		- [트리거의 종류](#트리거의-종류)
		- [트리거 생성](#트리거-생성)
		- [트리거 삭제](#트리거-삭제)
		- [트리거가 생성하는 임시 테이블 - NEW, OLD](#트리거가-생성하는-임시-테이블---new-old)
		- [생성된 트리거 확인](#생성된-트리거-확인)
		- [다중 트리거](#다중-트리거)
		- [중첩 트리거](#중첩-트리거)
# Ch 04. 테이터베이스 모델링

데이터베이스 모델링이란 현실 세계의 작업, 사물들을 DBMS의 데이터베이스 테이블로 옮기는 과정이다.

## MySQL Workbench 모델링 툴 실습

- 다이어그램 작성 : Menu → File → New Model
- 다이어그램 저장 : Menu → Save Model
- 다이어그램 → 데이터베이스 테이블로 변환 : Menu → Database → Foward Engineer
- 데이터베이스 → 다이어그램으로 변환 : Menu → Database → Reverse Engineer

# Ch 06. SQL 기본

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

## between A and B

키가 180~183인 사람을 조회

`select * from usertbl where height between 180 and 183;`

## in()

지역이 '경남', '전남', '경북'인 사람의 정보를 조회

`select * from usertbl where addr IN ('경남', '전남', '경북');`

## 문자열 검색 : like

이름이 김으로 시작하는 사람 조회

`select name, height from usertbl where name like '김%';`

- % : 무엇이든 허용
- _ : 글자 1개 매치

**참고** 

%, _ 가 검색할 문자열의 제일 앞에 들어가면 MySQL 성능에 악영향을 끼치게된다. 예를 들어 name 열을 '%용' 등으로 검색하면, name 열에 인덱스가 있더라도 인덱스를 사용하지 않고 전체 데이터를 검색한다.

## ANY/ALL/SOME 그리고 서브쿼리

```sql
select name, height from usertbl where height >= any(select height from usertbl where addr = '경남');

select name, height from usertbl where height >= all(select height from usertbl where addr = '경남');

select name, height from usertbl where height = any(select height from usertbl where addr = '경남');
```

- any 또는 some : 서브쿼리의 여러 개의 결과 중 한가지만 만족하면됨.
- all : 서브쿼리의 여러 개의 결가를 모두 만족해야함.
- `= any(서브쿼리)` == `in(서브쿼리)`

## ORDER BY

```sql
select name, height from usertbl order by name asc, height desc;
select name, height from usertbl order by name, height desc;
```

- asc(오름차순)은 디폴트 값이므로 생략 가능.
- order by 절은 select, from, where, group by, having 중에서 제일 뒤에 와야한다.
- **order by 절은 MySQL 의 성능에 상당한 영향을 줄 수 있기 때문에, 꼭 필요한 경우가 아니라면 사용하지 말자.**

## DISTINCT - 중복 제거

```sql
select distinct from usertbl;
```

## LIMIT - 출력 개수 제한

```sql
select * from userTbl limit 5;

-- 0부터 시작해서 5개 출력
select * from userTbl limit 0, 5;
select * from userTbl limit 5 offset 0;
```

## CREATE TABLE ... SELECT - 테이블 복사

- PK, FK 등의 제약조건은 복사되지 않는다.

```sql
-- member 테이블의 내용을 member_copy 테이블에 복사
create table member_copy (select * from member);

create table member_name (select name from member);
```

## GROUP BY, HAVING, 집계함수

### 집계 함수

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

### Having 절

- 집계 함수의 조건을 지정한다.
- **having 절은 group by 절 뒤에 와야한다.**

```sql
select userID as '사용자', sum(price*amount) as '총 구매액' 
	from buytbl 
	group by userID 
	having sum(price*amount) > 1000;
	order by sum(price*amount);
```

### ROLLUP

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

## SQL의 분류

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
-- usertbl 테이블 인덱스 크기 확인
SHOW TABLE STATUS LIKE 'usertbl';

-- 현재 데이터베이스에 있는 테이블들의 인덱스 크기 확인
SHOW TABLE STATUS;

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
## 인덱스 성능 비교

만약 member 테이블의 primary key인 id의 최대 값이 49999인데 아래와 같이 where 조건에 id가 500000 이하인 경우를 조회하는 쿼리를 실행한다고 가정해보자
```sql
-- 사실상 전체 범위 조회지만 인덱스를 사용하여 조회
SELECT * FROM member WHERE id <= 500000;
```
이 경우 사실상 테이블 전체를 가져오는 것과 같기 때문에 풀 테이블 스캔을 하는 것이 Data Read 양이 더 줄어들고 효율적일 것 같지만 WHERE 조건에 id가 있기 때문에 Index Range Scan을 수행한다.

주의 : 전체적인 효율은 시스템의 다양한 상황이나 데이터 분포 등에 영향을 받을 수 있기 때문에 읽은 양이 적다고 반드시 효율적인 것은 아니다.

인덱스 힌트를 사용해서 인덱스를 강제로 사용하거나 사용하지 않도록 할 수 있다.
- IGNORE INDEX (인덱스명) : 강제로 해당 인덱스를 사용하지 않도록 한다.
- USE INDEX(인덱스명) : 강제로 해당 인덱스를 사용하도록 한다.  

인덱스 힌트는 이 두가지 정도만 알고 있으면 된다.
```sql
-- 인덱스를 사용하지 않도록하여 풀 테이블 스캔한다.
SELECT * FROM member IGNORE INDEX(PRIMARY) WHERE id < 5000000;
```

응용프로그램이 주로 전체 데이터의 15% 이상의 범위의 데이터를 검색하는 경우에는 인덱스를 만들지 않는 것이 시스템 성능에 도움이 된다. -> 만약 많은 양의 데이터를 보조 인덱스를 사용하여 검색하면 그만큼 인덱스와 데이터 페이지 사이를 왔다갔다 해야하기 때문에

불필요한 보조 인덱스는 데이터의 변경 작업(특히 INSERT) 시에 시스템 성능을 나쁘게 만들 수 있다.

**WHERE 문에서 인덱스가 있는 열에 함수나 연산을 사용하는 것을 주의하자**  
```sql
-- 인덱스 사용
SELECT * FROM member WHERE id = 100000;

-- 인덱스 미사용
SELECT * FROM member WHERE id*1 = 100000;
```
최근 버전에서는 어떤 경우에는 열 이름에 함수가 적용되어도 인덱스를 사용하기도 하지만, 그렇지 않은 경우도 많기 때문에 최대한 WHERE 절에 나오는 열 이름에는 아무런 가공을 하지 않도록 하자.

참고 : MySQL Workbench의 실행 계획에서 Key Lookup은 인덱스에서 검색된 RID(Row ID, 각 행의 고유번호)를 가지고 실제 데이터 페이지의 데이터로 찾아가는 과정을 뜻한다.(클러스터형 인덱스는 리프 페이지가 곧 데이터 페이지이기 때문에 Key Lookup 용어를 사용하지 않는다.)

데이터 중복도
- 데이터 중복도란 데이터의 종류가 얼마나 분포되어 있는가를 말한다. 
- 다른 용어로 Cardinality(원소 개수) 로도 사용된다. 
- 예로 Gender 값의 경우 남자, 여자 두가지 뿐이기 때문에 Cardinality가 상당히 낮은 것이다. 
- Cardinality가 높다는 것은 데이터의 종류가 넓게 분포되어 있다는 것이다. 
- Primary Key, Unique는 데이터가 중복되지 않으므로 Cardinality가 높다.

데이터 중복도가 낮은 경우 인덱스를 사용하여 조회하는 것이 효율성이 없는 것은 아니지만, 인덱스 관리 비용과 INSERT 등의 데이터 변경 구문에서는 성능이 저하될 수 있기 때문에 데이터 중복도가 낮은 경우 인덱스를 사용하는 것은 좋지 않다.

## 결론 : 인덱스를 생성해야 하는 경우, 그렇지 않은 경우
- 인덱스는 열 단위에 생성되고 여러 개의 열을 조합하여 생성할 수 있다.
- WHERE 절에서 사용되는 열에 인덱스를 만들어야 한다 -> 조회 시에 인덱스를 사용하는 경우는 WHERE 절 조건에 해당 열이 나오는 경우에만 사용된다.
- WHERE 절에 사용되더라도 자주 사용되어야 가치가 있다. -> 페이지 분할 작업으로 인해 INSERT 성능이 나빠진다.
- 인덱스는 테이블을 정의하는 데이터베이스 모델링 시점에 어디에 생성할 것인지 정하는 것이 좋다. 이미 운영되고 있는 대용량 테이블의 인덱스를 변경하는 겻은 간단한 일이 아니다. 
- 데이터 중복도가 높은 열은 인덱스를 만들때 신중하게 판단하자 -> 보조 인덱스를 만들어도 MySQL이 사용하지 않거나, 성능 향상이 크게 없다.
- 외래 키를 생성한 열에는 자동으로 외래 키 인덱스가 생성된다. -> 쿼리문에서 외래 키 인덱스가 필요할 경우 MySQL 알아서 사용해준다.
- JOIN에 자주 사용되는 열에는 인덱스를 생성해 주는 것이 좋다.
- 인덱스를 만들어서 SELECT 성능을 높힐지, 만들지 않아서 INSERT/UPDATE/DELETE 시에 영향을 최소화 할 것인지 잘 결정해야한다.
	- 인덱스는 읽기 성능은 향상 시키지만, 데이터 변경 작업의 성능은 저하된다.
	- 인덱스를 많이 만들어도 문제가 되지 않는 테이블은 INSERT 작업이 거의 없는 테이블
- 클러스터형 인덱스는 테이블당 하나 생성 가능
- 클러스터형 인덱스를 사용하면 데이터 페이지가 정렬되기 때문에, 클러스터형 인덱스를 사용하는 열은 범위(BETWEEN, <, > 등), 집계 함수, ORDER BY에 사용하기에 좋다.
- 클러스터형 인덱스가 테이블에 아예 없는 것이 좋은 경우도 있다.
	- 대용량 데이터가 계속 입력되는 시스템에서 클러스터형 인덱스가 있으면 데이터가 입력되는 즉시 정렬 작업이 일어나면서 페이지 분할 작업이 끊임 없이 일어날 수 있다. -> 이런 경우 Primary Key 대신 Unique로 지정하는 것이 낫다.  
	(주의 : UNIQUE를 NOT NULL로 지정하면 클러스터형 인덱스가 생성되기 때문에 NULL로 지정하고 프로그래밍으로 필수로 입력되도록 해야한다.)
- 사용하지 않는 인덱스는 제거하자. -> WHERE 조건에서 사용되지 않는 열의 인덱스는 공간만 차지하고, 입력 성능을 저하시킨다.
- 주기적인 OPTIMIZE TABLE, ANALYZE TABLE 구문으로 인덱스 재구성을 통해서 조각화를 최소화해야 시스템 성능을 최상으로 유지할 수 있다.
	- OPTIMIZE TABLE : 테이블 데이터 및 관련 인덱스 데이터의 물리적 스토리지를 재구성하여 테이블에 액세스할 때 스토리지 공간을 줄이고 I/O 효율성을 향상시킨다.
	- ANALYZE TABLE : 인덱스를 재생성하여 성능을 최적화, 키를 재분배

# 스토어드 프로그램
스토어드 프로그램이란 MySQL에서 프로그래밍 언어와 같은 기능을 제공하는 것을 통틀어 말하는 것이다. 스토어드 프로그램은 크게 스토어드 프로시저, 스토어드 함수, 트리거, 커서 등이 있다.

스토어드 프로그램은 쿼리를 묶어주는 것뿐 나이라, 프로그래밍 기능을 제공하고 시스템 성능 향상에 도움을 줄 수 있기 때문에 실제 현업에서도 쿼리의 대부분을 스토어드 프로그램을 만들어서 사용한다.

## 스토어드 프로시저
스토어드 프로시저는 쿼리 문의 집합으로 어떠한 동작을 일괄 처리하기 위한 용도로 사용된다. 자주 사용하는 쿼리를 모듈화 시켜서 필요할 때마다 호출하여 사용하면 훨씬 편리하게 MySQL을 운영할 수 있다.

스토어드 프로시저도 데이터베이스의 개체 중 하나로, 테이블처럼 각 데이터베이스 내부에 저장된다.

```sql
-- 스토어드 프로시저 형식
DELIMITER $$
CREATE PROCEDURE 스토어드_프로시저이름(IN 또는 OUT 파라미터)
BEGIN
	...
END $$
DELIMITER ;

CALL 스토어드_프로시저이름();
```
참고 : DELIMITER $$ ~ END $$는 스토어드 프로시저를 묶어주는 부분으로, MySQL에서 기본 종료문자는 ;인데 프로시저 내에서도 ;을 종료 문자로 사용하면 어디까지가 프로시저인지 구별하기 어렵다. 그래서 종료 문자를 일시적으로 변경하여 사용한다.

### 스토어드 프로시저 변경과 삭제
- ALTER PROCEDURE, ALTER FUNCTION 문으로 스토어드 프로시저나 스토어드 함수의 내용은 변경할 수 없다. 내용을 바꾸려면 DROP후 CREATE해야 한다.
- DROP PROCEDURE 문을 사용하여 스토어드 프로시저를 삭제할 수 있다.

### 매개변수의 사용

#### 입력 매개변수 
입력 매개변수를 지정하는 방법은 다음과 같다.
```sql
IN 입력_매개변수_이름 데이터_형식
```
입력 매개변수가 있는 스토어드 프로시저의 실행 방법은 다음과 같다.
```sql
CALL 스토어드_프로시저이름(전달 값);
```
```sql
-- 입력 매개변수가 있는 프로시저의 예
DROP PROCEDURE IF EXISTS memberProc;
DELIMITER $$
CREATE PROCEDURE memberProc(
	IN memberBirthYear INT,
    IN membrerHeight INT
)
BEGIN
	SELECT * FROM member WHERE birthYear > memberBirthYear AND height > memberHeight;
END $$
DELIMITER ;

CALL memberProc(1995, 182);
```

#### 출력 매개변수
출력 매개변수를 지정하는 방법은 다음과 같다.
```sql
OUT 출력_매개변수_이름 데이터_형식
```
`SELECT ... INTO`문을 사용하여 출력 매개변수에 값을 대입한다.  
출력 매개변수가 있는 스토어드 프로시저의 실행 방법은 다음고 같다.
```sql
CALL 프로시저_이름(@변수명);
SELECT @변수명;
```
```sql
-- 출력 매개변수 예
DROP PROCEDURE IF EXISTS myProc;
DELIMITER $$
CREATE PROCEDURE myProc(
	IN memberName CHAR(10),
    OUT createdMemberId INT
)
BEGIN
	INSERT INTO member VALUES(NULL, memberName);
    SELECT MAX(id) INTO createMemberId FROM member;
END $$
DELIMITER ;

CALL myProc('김김김', @memberId);
SELECT CONCAT('현재 입력된 ID 값 ==>', @memberId);
```

### 스토어드 프로시저 프로그래밍
IF..ELSE, CASE문, 반복문과 같은 SQL 프로그래밍 기능의 대부분을 프로시저에서 사용할 수 있다.

### 기타 프로시저 관련 
#### 저장된 프로시저의 이름 및 내용 조회
```sql
/*
프로시저의 이름 및 내용 조회
- routine_name : 이름
- routine_definition : 내용
*/
SELECT routine_name, routine_definition FROM INFORMATION_SCHEMA.ROUTINES WHERE routine_schema = 'sqldb' AND routine_type = 'PROCEDURE';

-- SHOW CREATE PROCEDURE 문으로도 스토어드 프로시저의 내용을 조회할 수 있다.
SHOW CREATE PROCEDURE sqldb.userProc;

/*
프로시저 파라미터 조회
- parameter_mode : IN or OUT
- parameter_name : 파라미터명 
- dtd_identifier : 데이터 타입
*/
SELECT parameter_mode, parameter_name, dtd_identifier FROM INFORMATION_SCHEMA.PARAMETERS WHERE specific_name = 'userProc';
```

#### 테이블 이름을 파라미터로 전달하는 방법
 - FROM 절 테이블 이름에 일반적인 입력 매개변수를 사용하는 방법으로 사용하면 오류가 발생한다. 즉, MySQL에서 직접 테이블 이름을 파라미터로 사용할 수 없다.
 동적 SQL을 사용하여 입력 매개변수를 테이블 이름으로 사용할 수 있다.
 ```sql
 DROP PROCEDURE IF EXISTS nameProc;
DELIMITER $$
CREATE PROCEDURE nameProc(
	IN tblName VARCHAR(20)
)
BEGIN
	SET @sqlQuery = CONCAT('SELECT * FROM ', tblname);
    PREPARE myQuery FROM @sqlQuery;
    EXECUTE myQuery;
    DEALLOCATE PREPARE myQuery;
END $$
DELIMITER ;

CALL nameProc('userTBL');
 ```
 
 ### 스토어드 프로시저의 특징
 #### MySQL 성능의 향상
 많은 양의 코드로 구현된 쿼리를 실행하면 클라이언트에서 서버로 쿼리의 모든 텍스트가 전송되는데, 이 많은 양의 코드를 서버에서 스토어드 프로시저로 생성해 놓으면 단지 스토어드 프로시저 이름과 매개변수 등 몇 글자의 텍스트만 전송하면 되기 때문에 네트워크 부하를 어느 정도 줄일 수 있고, 결과적으로 MySQL의 성능을 향상시킨다.

 참고: 다른 DBMS에서는 스토어 프로시저가 최초 호출 되었을때 때 한번 컴파일되어 메모리에 로딩되고 이후 호출부터는 메모리에 있는 것이 호출되어 상당한 성능 향상이 있다. 이와 달리 MySQL의 스토어드 프로시저는 호출될 떄마다 컴파일이 되기 때문에 다른 DBMS에 비해 성능 향상은 미미하지만 네트워크의 부하를 줄이는 등 효과가 있기 때문에 어느 정도 성능 향상이 있다고 볼 수 있다.

 #### 유지관리의 간편함
 응용 프로그램에서 직접 SQL을 작성하지 않고 스토어드 프로시저를 호출함으로써, 데이터베이스에서 관련된 스토어드 프로시저의 내용을 일관되게 작업을 할 수 있다.

 #### 모듈식 프로그래밍
 스토어드 프로시저르 한번 생성해 놓으면 언제든지 재사용 할 수 있고, 스토어드 프로시저에 사용된 쿼리의 변경 삭제가 수월해진다. 다른 모듈식 프로그래밍 언어와 동일한 장점을 가진다.

 #### 보안 강화
 뷰와 같이 사용자 별로 테이블에 접근 권한을 주지 않고, 스토어드 프로시저에만 접근 권한을 줌으로써 보안을 강화할 수 있다.

 ## 스토어드 함수
사용자가 직접 만들어서 사용하는 함수를 스토어드 함수라고 하며, 스토어드 프로시저와 유사하지만 형태와 사용 용도가 약간 다르다.

스토어드 프로시저는 여러 SQL 문을 묶거나 계산 등 다양한 용도로 사용되지만, 스토어드 함수는 어떤 계산을 통해 하나의 값을 반환하는데 사용된다.

```sql
-- 스토어드 함수 형식
DELIMITER $$
CREATE FUNCTION 스토어드_함수이름( 파라미터 )
    RETURNS 반환_데이터_타입
BEGIN
    ...
END $$
DELIMITER ;

SELECT 스토어드_함수이름();
```

- 스토어드 함수의 파라미터는 모두 입력 파라미터로 사용된다.
- 스토어드 함수는 RETURNS 문으로 반환 값의 타입을 지정하고, 본문 안에서 RETURN 문으로 하나의 값을 반환한다.(스토어드 프로시저는 OUT을 사용하여 여러 개의 값 반환 가능)
- 스토어드 함수는 SELECT 문 안에서 호출된다.(스토어드 프로시저는 CALL로 호출)
- 스토어드 프로시저 내에서는 SELECT 문을 사용할 수 있지만, 스토어드 함수 내에서는 집합 결과를 반환하는 SELECT를 사용할 수 없다.(SELECT ... INTO 는 결과를 반환하는 것이 아니기 때문에 예외적으로 사용 가능)

```sql
-- 스토어드 함수를 사용하려면 스토어드 함수 생성 권한을 줘야한다.
SET GLOBAL log_bin_trust_function_creators = 1;

-- 나이 계산 스토어드 함수
DROP FUNCTION IF EXISTS getAgeFunc;
DELIMITER $$
CREATE FUNCTION getAgeFunc(bYear INT)
	RETURNS INT
BEGIN
	DECLARE age INT;
    SET age = YEAR(CURDATE()) - bYear;
    RETURN AGE;
END $$
DELIMITER ;

SELECT getAgeFunc(1995);

-- 함수의 반환 값을 SELECT ... INTO문으로 저장했다가 사용할 수도 있다.
SELECT getAgeFunc(1995) INTO @age1990;

SELECT CONCAT('1995년 생의 나이: ', @age1990);

-- 함수는 주로 테이블을 조회할 때 활용 된다.
SELECT id, name, getAgeFunc(birthYear) AS '만 나이' FROM member;

-- 현재 저장된 스토어드 함수의 이름 및 내용 조회
SHOW CREATE FUNCTION 함수명;

-- 스토어드 함수 삭제
DROP FUNCTION 함수명;
```
## 커서
커서는 테이블에서 여러 개의 행을 쿼리한 후에, 쿼리의 결과인 행 집합을 한 행씩 처리하기 위한 방식이다.

텍스트 파일을 처리하는 프로그래밍 과정을 생각해보자. 
1. 파일을 연다(OPEN), 파일 포인터튼 BOF(Begin Of File)을 가리킨다.
2. 현재 파일 포인터가 가리키는 데이터를 읽거나 쓰고, 파일 포인터는 자동으로 다음 행으로 이동한다.
3. EOF(End Of File)까지 2번 과정을 반복한다.
4. 파일을 닫는다.(CLOSE)

커서는 파일 처리의 동작 방식과 비슷하게 동작한다.
1. 커서의 선언(DECLARE CURSOR)
2. 반복 조건 선언(DECLARE CONTINUE HANDLER) -> 더이상 읽은 행이 없을 경우 실행할 내용 설정
3. 커서 열기(OPEN)
4. LOOP ~ END LOOP문을 사용해 커서에서 데이터를 가져오고(FETCH) 데이터를 처리하는 과정을 반복 조건을 만족하는 동안 반복한다.
5. 커서 닫기(CLOSE)

커서의 대부분은 스토어드 프로시저 내에서 활용된다. 

커서 예제 코드
```sql
-- 고객이 구매한 총액에 따라 고객 등급 열에 값을 입력하는 예제
DROP PROCEDURE IF EXISTS gradeProc;
DELIMITER $$
CREATE PROCEDURE gradeProc()
BEGIN
    DECLARE id VARCHAR(10);
    DECLARE hap BIGINT; 
    DECLARE userGrade CHAR(5);
    
    DECLARE endOfRow BOOLEAN DEFAULT FALSE;
    
    DECLARE userCursor CURSOR FOR -- 1. 커서 선언
        SELECT U.userid, sum(price*amount) 
        FROM buyTbl B RIGHT OUTER JOIN userTbl U 
        ON B.userid = U.userid 
        GROUP BY U.userid, U.name;
    
    DECLARE CONTINUE HANDLER --  2. 반복 조건 선언
        FOR NOT FOUND SET endOfRow = TRUE; -- 행의 끝이면 endOfRow 변수에 TRUE를 대입
        
    OPEN userCursor; -- 3. 커서 열기
    
    grade_loop: LOOP -- 4
        FETCH userCursor INTO id, hap;  -- 커서에서 데이터 가져오기
        IF endOfRow THEN -- 더이상 읽을 행이 없으면 Loop를 종료
            LEAVE grade_loop;
        END IF;
        
        CASE
            WHEN (hap >= 1500) THEN SET userGrade = '최우수고객';
            WHEN (hap >= 1000) THEN SET userGrade = '우수고객';
            WHEN (hap >= 1) THEN SET userGrade = '일반고객';
            ELSE SET userGrade = '유령고객';
        END CASE;
        
        UPDATE userTbl SET grade = userGrade WHERE userID = id;
    END LOOP grade_loop;
    
    CLOSE userCursor; -- 5. 커서 닫기
END $$
DELIMITER ;

CALL gradeProc();

SELECT * FROM userTBL;
```

## 트리거
트리거란 테이블에 부착되어 해당 테이블에서 DML(INSERT, UPDATE, DELETE 등)의 이벤트가 발생했을 때 작동하는 데이터베이스 개체이다.

참고 : 일부 DBMS에서는 View에 트리거를 부착할 수 있지만, MySQL에서는 View에 트리거를 부착할 수 없다.

만약 어떤 테이블의 행을 실수로 삭제해버린 상황을 가정해보자. 이런 경우 테이블에서 삭제될 때 삭제한 내용을 다른 테이블에 저장하도록 트리거를 만들어 놓음으로써 문제를 해결할 수 있다.

### 트리거의 종류
- AFTER 트리거 : 테이블에 삽입, 수정, 삭제 작업이 일어난 후에 작동하는 트리거
- BEFORE 트리거 : 테이블에 삽입, 수정, 삭제 작업이 일어나기 전에 작동하는 트리거

### 트리거 생성
```sql
-- 트리거 생성 문법
CREATE
	[DEFINER = user]
	TRIGGER trigger_name
	trigger_time trigger_event
	ON tbl_name FOR EACH ROW
	[trigger_order]
	trigger_body

trigger_time: { BEFORE | AFTER}
trigger_event: { INSERT | UPDATE | DELETE }

/*
테이블에 여러 트리거가 부착되어 있을때 어떤 트리고보다 먼저 또는 나중에 수행되도록 지정하는 것
- FOLLOWS 트리거명 : 지정한 트리거 다음에 현재 트리거 실행
- PRECEDES 트리거명 : 지정한 트리거 작동 이전에 현재 트리거 실행
*/
trigger_order: { FOLLOWS | PRECEDES } other_trigger_name
```
예를 들어 member 테이블의 행이 삭제되었을 때, 삭제된 내용을 백업 테이블에 저장하는 트리거는 다음과 같이 생성할 수 있다.
```sql
DELIMITER //
CREATE TRIGGER backupMemberTrg
	AFTER DELETE
	ON member
	FOR EACH ROW
BEGIN
	INSERT INTO backup_member (name, birthDate, address, deleteDate) VALUES (OLD.name, OLD.birthDate, OLD.address, CURDATE());
END //
DELIMITER ;
```

### 트리거 삭제
```sql
DROP TRIGGER trigger_name
```

참고로 트리거는 ALTER 문을 사용할 수 없다.

### 트리거가 생성하는 임시 테이블 - NEW, OLD
트리거에서 작업이 수행되면 임시로 사용되는 시스템 테이블 NEW, OLD 두개가 있다.

NEW 테이블
- 테이블에 INSERT/UPDATE 쿼리가 실행됐을 때, 입력/변경되는 새 값이 NEW 테이블에 가장 먼저 저장된 후에 NEW 테이블에 있는 값이 테이블에 입력/변경이 된다.
- new 테이블을 조작하면 입력된 새로운 값을 다른 값으로 조작할 수 있다.

OLD 테이블
- 테이블에 DELETE/UPDATE 쿼리가 실행됐을 때, 삭제/변경 되기 전의 예전 값이 저장된다.

### 생성된 트리거 확인
SHOW TRIGGERS 문으로 데이터베이스 생성된 트리거를 확인할 수 있다.
```sql
SHOW TRIGGERS 데이터베이스명;
```

### 다중 트리거
다중 트리거는 하나의 테이블에 동일한 트리거가 여러개 부착되어 있는 것을 말한다.

예를 들어 어떤 테이블에 AFTER DELETE 트리거가 여러개 부착되어 있는 것을 말한다.

트리거들의 순서를 정해야 한다면 앞서 살펴본 트리거의 생성 문법에서 trigger_order 옵션을 사용하여 순서를 지정해주면 된다.

### 중첩 트리거
중첩 트리거란 트리거 내에서 또 다른 트리거가 작동하도록 하는 것을 말한다.

참고 : MySQL은 재귀 트리거는 지원하지 않는다. 다른 DBMS에서는 재귀 트리거를 지원하기도 한다.

중첩 트리거의 예
```sql
-- 구매를 하면(INSERT order) 물품의 재고가 감소하고(UPDATE product), 물품의 재고가 감소하면 배송 테이블에 데이터를 추가(INSERT delivery)하는 중첩 트리거 사용 예제

CREATE TABLE order -- 구매 테이블
(
	orderNo INT AUTO_INCREMENT PRIMARY KEY,
    userID VARCHAR(5),
    prodName VARCHAR(5),
    orderamount INT
);

CREATE TABLE product -- 물품 테이블
(
	prodName VARCHAR(5),
    account INT
);

CREATE TABLE delivery -- 배송 테이블
(
	deliverNo INT AUTO_INCREMENT PRIMARY KEY,
    prodName VARCHAR(5),
    account INT
);

-- 물품 테이블에서 개수를 감소시키는 트리거
DROP TRIGGER IF EXISTS orderTrg;
DELIMITER //
CREATE TRIGGER orderTrg
	AFTER INSERT
    ON order
    FOR EACH ROW
BEGIN
	UPDATE product SET account = account - NEW.orderAmount WHERE prodName = NEW.prodName;
END //
DELIMITER ;

-- 배송 테이블에 새 배송건을 입력하는 트리거
DROP TRIGGER IF EXISTS porductTrg;
DELIMITER //
CREATE TRIGGER productTrg
	AFTER UPDATE
    ON product
    FOR EACH ROW
BEGIN
	DECLARE orderAmount INT;
    -- 주문 개수 = (변경 전의 개수 - 변경 후의 개수)
    SET orderAmount = OLD.account - NEW.account;
    INSERT INTO delivery(prodName, account) VALUES (NEW.prodName, orderAmount);
END //
DELIMITER ;
```
