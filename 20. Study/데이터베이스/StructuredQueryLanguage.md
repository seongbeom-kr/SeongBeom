#복습 #DB #SQL #lecture 

## DDL 
> 릴레이션의 집합 뿐만  아니라 각각의 릴레이션에 대한 데이터도 명세함

- 스키마, 도메인, 참조 무결성, 인덱스의 집합, 보안, 물리 저장 장소에 대한 데이터

# DDL
> DDL에 대한 내용을 배움
> **Create Table, Drop Table, Truncate Table, Alter Table, Data Type, Constraint**

## DDL 요약
- 테이블 생성 : Create Table []
- 테이블 스키마 관련 변경 : Alter Table []
- 테이블 삭제 : Drop Table
- 이름 변경 : Rename 
- 테이블의 모든 데이터 삭제 : Truncate 
- 테이블에 설명 추가 : Comment

## 테이블 생성
Create Table 문 이용 -> 테이블 이름, 컬럼 이름, 데이터 타입 정의
```
Create Table Book(
	bookno Number(5),
	title varchar2(50),
	author varchar2(10),
	pubdate DATE
);
```

### Subquery를 이용한 테이블 생성
이미 존재하는 테이블과 같은 스키마를 가지고 테이블 생성

```
create table temp_account like accout*
```

- 서브쿼리의 결과와 동일한 테이블 생성
- 질의 결과 레코드들이 포함됨
- Not Null 제약조건만 상속됨

```
Create table empSALES
AS
	Select * from emp
	where job = 'SALES' # 빈테이블 만들기,
```

### Table의 종류
#### User Table
- 유저에 의해 유지되고 만들어진 테이블의 집합들
#### Data Dictionary
- 오라클 서버에 의해 만들어지고 유지되는 테이블들


## Alter Table
- 추가 `Alter Table book Add (pubs VARCHAR2(50))`
- 수정 `Alter Table book Modify (title VARCHAR2(100))`
- 삭제 `Alter Table book Drop author`
- unused `set unused(칼럼이름)`
### 기타 테이블 관련 명령
- 테이블 삭제 : `Drop Table`
- 모든 데이터 삭제(스키마 제외) : `Truncate Table 000`
- Comment 
- `Rename`
- 주의해야할 점 : 롤백의 대상이 아님
	- 테이블 관련 명령어들은 로그를 남기지 않기 때문에 되돌릴 수 없다

## 제약조건
- (Intergrity) Constraint 
	- 데이터베이스 테이블 레벨에서 특정한 규칙을 설정해둠
	- 예상치 못한 데이터의 손실이나 일관성을 위반하는 데이터의 추가, 변경을 예방함
- 종류
	- Not null
	- UNIQUE
	- PRIMARY KEY
	- FOREIGN KEY
	- CHECK

### 제약조건 정의
#### Syntax
```
Create Table 테이블 이름 (
	컬럼이름 데이터타입 [Default 기본값] [컬럼제약조건],
	컬럼이름 데이터타입 [Default 기본값] [컬럼제약조건],
	... //디폴트가 없다면 널이 기본값
	[테이블 제약조건]
);
```

- 컬럼 제약조건 : [Constraint 이름] constraint_type
- 테이블 제약조건 : [Constraint 이름] constraint_type(columns)

#### Not NULL
- null 값이 존재할 수 없음
- 컬럼형태로만 제약조건 정의할 수 있음 (테이블 제약조건 불가)

#### UNIQUE
- 중복된 값을 허용하지 않음 (Null은 허용)
- 복합 컬럼에 대해서 정의 가능
- 자동으로 인덱스 생성

#### PRIMARY KEY
- Not Null + UNIQUE 
- 테이블 당 하나만 존재할 수 있음
- 복합 컬럼에 대해서 정의 가능
```
Create Table book(
	ssn1 number(9),
	ssn2 number(9),
	PRIMARY KEY (SSN1, SSN2)
)
```

#### Check
- 임의의 조건 검사, 조건식 참이어야 변경 가능
- SQL CHECK  ON CREATE TABLE

#### Foreign Key
- 참조무결성 제약
- 일반적으로 Reference되는 테이블의 PK를 참조
- 참조되는 테이블에 없는 값은 삽입 불가
- 참조되는 테이블의 레코드 삭제 시 동작방식
	- **on delete no action** : 해당 FK를 가진 참조행 삭제 시도시에 오류 발생 및 delete 문 롤백
	- **on delete cascade** : 해당 FK를 가진 참조행도 삭제
	- **on delete** : 지정하지 않을 시 No Action이 기본값으로 사용됨
	- **on delete set null** : 해당하는 FK를 NULL로 바꿈 <- null이 허용되어야 가능
	- **on delete set default** : 기본값이 null일 수 있음, 디폴트가 정의되거나 null(암묵적)
```
create Table book (

...
author_id NUMBER(10),
constraint c_book_fk Foreign Key(author_id)
References author(id)
on delete set null
);
```

#### 제약조건 추가 삭제
- 추가
	- *Alter table name add constraint*
	- not null은 추가 못함
```
Alter Table emp add constraint emp_mgr_fk foreign key(mgr) references emp (empno);
```

- 삭제
	-  *Alter Table name Drop constraint 제약조건 이름*
	- primary key의 경우 fk 조건이 걸린 경우에는 cascade로 삭제

# DML
## Data Manipulation Language
### 종류
- add new row
	- insert into table_name [col_list] values ()
- modify existing rows
	- update table_name set changed_contents [where 조건]
- Remove exisiting rows
	- delete from table_name [where 조건]
### 트랜잭션의 대상임

## Insert $star^5$
- 묵시적 방법 : 컬럼이름, 순서를 지정하지 않음, 테이블 생성시 정의한 순서에 따라 값 지정
```
insert into dept values (777, 'marketing', null);
```

- 명시적 방법 : 컬럼이름 사용, 지정되지 않은 컬럼  = Null or Default
```
insert into dept(dname, deptno) values ('marketing',777);

```

- SubQuery 이용 : 타 테이블로부터 데이터 복사(테이블은 이미 존재하여야 함)
```
insert into deptusa
select deptno, dname from dept where country = 'usa';
```

## Update
- 조건을 만족하는 레코드를 변경 -> where 절 사용해서 진행
- where절이 없으면 모든 레코드에 적용됨
- subquery사용가능
## Delete
- where절 사용
- 사용안하면 tuple만 삭제, 스키마는 삭제 안함
- 서브쿼리 가능 

# 기본 Select
> 관계대수는 중복을 허용하지 않지만, sql은 중복을 허용

## Select
- DB에서 원하는 데이터를 검색, 추출
- select, from, where groupby(having) orderby(asc, desc), distinct

### 형식
as사용가능
### 내용
* *모든 컬럼 반환
* distinct : 중복된 결과 제거
* select col_name : 프로젝션
* from : 대상 테이블
* alias : 컬럼 이름
* expression : 기본적인 연산 및 함수 사용 가능

## 산술연산
- 기본적인 산술연산 사용가능 

## Null
- column constraint 중 not null + primary key 면 사용 불가능
- 널을 포함한 산술식은 일반적으로 null
```
select sal, comm, (sal + comm) * 12 from emp;
```
- NVL(expr1, expr2)
	- expr1이 null이면 expr2로 반환한다.
	- 데이터 타입이 호환되어야 함
```
select sal, comm, (sal + NVL(comm, 0)) * 12 from emp;
```

## Literal
- select절에 사용되는 문자, 숫자 date타입 등의 상수
- date 타입이나 문자열은 작은 따옴표로 둘러싸야함
- 문자열 결합 연산자 ||

## where
- 조건을 부여하여 만족하는 Row selection
- 연산자
	- =, != , <,>, <=, >=
	- in
	- between a and b
	- LIKE
	- is null , is not null
	- and, or
	- not
	- any, all
	- exisit
### LIKE 연산
- wildcard를 이용한 문자열 부분 매칭
- wildcard : % 임의의 길이의 문자열(공백가능), _ 한글자
- escape
## order by
- 주어진 컬럼 리스트의 순서로 결과를 정렬
- 정렬 방법
	- asc - 오름차순 (작은 -> 큰)
	- desc - 내림차순 (큰 -> 작은)
- 여러 컬럼 정의 가능
	- 첫번째 컬럼이 같으면 두번째 컬럼으로, 두번째 컬럼도 같으면
- 컬럼 이름대신 alias, expr, select 절 상에서의 순서도 사용 가능
```
select * from emp order by deptno, sal desc
```

# JOIN
## JOIN
- 둘 이상의 테이블을 합쳐서 하나의 큰 테이블로 만드는 방법
- 필요성
	- 데이터 일관성, 효율을 위해서
	- 외래키를 사용한 참조
	- 서로 다른 테이블의 데이터를 결합해서 볼 경우

## cartesian product
두 테이블에서 그냥 결과를 선택하면? -> 가능한 모든 쌍이 추출됨 -> 원하는 바가 아님

이를 막기 위해서는 join조건을 where절에 부여


## simple join
```
select t1.col1, t1.col2, t2.col1 ...
form table1 t1, table2 t2
where t1.col3 = t2.col3
```

- from절에 필요한 테이블을 모두 적고
- 컬럼이름의 모호성을 피하기 위해 alias사용
- 적절한 조인 조건을 where 절에 부여
- 일반적으로 pk와 fk간의 = 조건이 붙는 경우가 많음

## join 종류
cross join - cartesian join
inner join - 조인 조건을 만족하는 튜플만 나타냄
outer join - join조건을 만족하지 않는 튜플도 null과 함께 나타냄
theta join - 조건에 의한 조인, 동일 컬럼 2개
equi join - 세타조인이면서 조건이 = , 동일한 컬럼명이 2개
natural join - equi join & 동일한 컬럼명 합쳐짐
self join : 자기 자신과 조인함, alias를 사용해야함 

# Group & Aggregation
## Aggregate Function
- 여러행으로부터 하나의 결과값을 반환
- 종류
	- AVG
	- count
		- count( * )
		- count(expr) 
		- count(distinct expr)
	- max, min, sum, stddev, variance

## 일반적인 오류
```
select deptno, avg(sal) from emp;
```
집계함수의 결과는 한 row만 남게 됨
deptno 는 하나의 로우에 표현 불가능
부서별과 같은 내용이 필요할 때에는 **group by**절 사용

## Group by
### 일반적인 오류
주의 : group by 에 참여한 필드나 집계함수만 올 수 있음
group by 한 이후에는 참여 필드만 남아있는 셈

## Having 절
aggregation결과에 대해 다시 condition을 적용할 때 사용

where절은 집계함수 이전, having은 집계함수 이후


