# Dictionary = catalog
## Data Dictionary
- DBMS가 관리하는 모든 정보를 저장하는 카탈로그
- 내용
	- 모든 스키마 객체정보, 공간정보, 컬럼의 기본값, 제약조건정보, 사용자정보, 권한및 역할정보
- Base-Table과 View로 구성됨
	- View의 Prefix
		- User : 로그인한 사용자레벨
		- ALL
		- DBA
- Sys Schema에 속함
- 딕셔너리의 테이블이나 컬럼이름은 모두 대문자 사용!

# RANK 관련 함수
## Rank 관련 함수

```sql
Select sal, ename,
	rank() over (order by sal desc) as rank,
	dens_rank() over (order by sal desc) as dense,
	row_number() over (order by sal desc) as row_number,
	rownum as "rownum"
from emp;
```

- rank() : 중복순위 개수만큼 다음 순위 값을 증가
- Dense_rank() : 중복 순위가 존재해도 순차적으로 다음 순위값을 표시함

# Duplicates
## Duplicates
- 중복 튜플을 가진 테이블에서, SQL은 결과에서 중복 튜플들을 어떻게 표현할 지 정의할 수 있음
- Multiset versions

# NULL
## Null Values
- it is possible for tuples to have a null value, denoted by null, for some of their attributes
- null signifies an unknown value or that a value does not exist.
- the result of any arithmetic expression involving null is null
	- 5 + null = null
- the predicate(조건식) is null can be used to check for null values
	```SQL
	  select name from instructor where salary is null
	  
	  ```
## Null values and Three Valued Logics
- 널과 비교는 알수없음
	- or : true, unknown, unknown
	- and : unknown, false, unknown
	- not : not known = unknown
- result of where clause predicate is treated as *false* if it evaluates to unknown
- 집계함수는 null을 무시함

# Nested Query
## Nested Subqueries
- SQL은 중첩쿼리를 제공함
- 중첩 쿼리는 select, from, where 표현에 사용가능함

## Set Comparison
```sql
select distinct T.name
from instructor as T, instructor as S
where T.Salary > S.salary and S.dept name = 'Biology';
```

위 코드와 아래 코드는 같은 결과를 나타냄

```sql
select name
from instructor
where Salary > some (select salary
					 from instructor
					 where dept name = 'Biology')
```

![[스크린샷 2024-05-25 17.09.33.png]]

## Correlation Variables
```sql
select course_id
from section as t
where semester = 'fall' and year = 2020 and
		exists (select *
				from section as T
				where semester = 'spring' and year= 2021
					and s.sourse_id = T.course_id);
```

## Not Exists
- $X - Y = \varnothing \Leftrightarrow  X \subseteq Y$
- = 는 사용하지 않음

## Test for Absence of Duplicate Tuples
- 유니크한 구조는 서브쿼리가 결과로 어떤 복제본 튜플을 가지고 있는 지 테스트 
- 2024년에 많아도 한번  제공받은 과목을 찾아라
 ```sql
select T.corse_id
from cousre T
where unique (select R.couse_id
			  from section as R
			  where T.course_id = R.course_id and R.year = 2024);
```
- 만약에 2024년도에 받지 못했다면, 서브쿼리는 empty를 리턴함, 그래서 unique조건문은 True가 됨
-
## Derived Relations // Derive : 유도하다
- SQL은 from 절에서 사용되는 서브쿼리를 허용함
```sql
select dept_name, avg_salary
from (select dept_name, avg(salary) as avg_salary
	  from instructor
	  group by dept_name)
where avg_salary > 42000;
```
- from절 안에 group by를 쓰면 주 쿼리문에서는 having을 쓰면 안됨

```sql
select dept_name, avg_salary
from (select dept_name, avg(salary) as avg_salary
	  from instructor
	  group by dept_name) as dept_avg(dept_name, avg_salary)
where avg_salary > 42000;
```
- from 절에 튜플형식으로 테이블 명(컬럼 이름1, 컬럼이름2..)으로도 사용 가능
#### Lateral
- 아니면 lateral 절을 사용해도 됨
```sql
select dept_name, avg_salary
from instructor l1, lateral (select avg(salary) as avg_salary
							 from instructor l2
							 where l2.dept_name = l1.dept_name);
```
- from 절에서 서프쿼리나 진행중인 테이블의 속성에 접근할 때 lateral 을 사용
	- from절에서 다른 릴레이션에서 correlation 변수를 사용하지 말아야함
	- 짧은 문장

## With Clause
- *with* 절은 임시 뷰를 정의하는 방법을 제공하는데, 이 정의는 with caluse가 포함된 쿼리에서만 사용할 수 있음
```sql
with max_budget(value) as 
	(select max budget
	 from department)
select dept_name, budget
from department, max_budget
where department.budget = max_budget.value;
	
```

- 복잡하게도 사용 가능
```sql
with dept_total(dept_name, value) as
	(select dept_name, sum(salary)
	 from instructor
	 group by dept_name),
dept_total_avg(value)as
	(select avg(value)
	 from dept_total)
select dept_name
from dept_total, dept_total_avg
where dept_total.value >= dept_total_avg.value
```
- with문으로 두개의 뷰를 생성(부서별 임금 합, 부서 전체 평균 임금), 이를 비교하는 쿼리문 작성

## Scalar Subquery
- 스칼라 서브쿼리 : 하나의 속성을 포함하는 오직 하나의 튜플
- 스칼라 서브쿼리는 having, where, select 절에서 발생함

# Modification
## Modification of the Database - Deletion
```sql
delete from instructor
```

- 조건을 걸어 삭제할 수 있음

## Modification of the Database - Insertion
- 값을 속성의 순서대로 입력
```sql
insert into course
	value ('cs-439', 'Database System', 'comp.sci', 4);
```

- 아니면 속성의 이름을 붙여서 삽입가능
```sql
insert into course(course_id, title, dept_name, credits)
	value ('cs-439', 'Database System', 'comp.sci', 4);
```

- 아니면 null을 포함한 상태로 넣을 수도 있음

-  select from where 문은 결과가 관계에 삽입되기 전에 완전히 평가됩니다(그렇지 않으면 테이블 1에서 전체 컬럼을 선택하여 테이블 1에 삽입하는 것과 같은 쿼리가 문제를 일으킬 수 있습니다).

## Modification of t