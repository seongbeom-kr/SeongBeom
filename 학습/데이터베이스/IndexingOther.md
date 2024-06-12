# OtherType of useful indices
## B+xmfl
- 대표적인 트리 기반 인덱스구조
- n-ary 탐색 트리
- 블록 기반의 접근 구조
- 데이터 삽입 삭제에도 자동적으로 트리 구조유지
- 최소 저장공간활용 비율 보장 가능
- 리프노드 데이터는 키에 의해 정룔됨
## 문법
```sql
-- create [unique] index_name On table name column list
-- drop index name
-- dict : user index, user ind index
select t.index_name, t.uniqueness, c.column_name, c.column_position
from user_indexes t, user_ind_columns c
where c.index_name = t.index_name and t.table_name = 'EMP';
```

## 오라클 데이터 접근 방법
- 전체 테이블 순차적 접근
- 인덱스 스켄 : 인덱스를 통한 접근
- 

## 쿼리 최적화
- 비용추정
- 휴리스틱 베이스(스스로 발견하는)
- 일반적인 고려 요소(인덱스 유무, 테이블 크기, 데이터 분포)

## Composite index & covering index
- e둘 이상의 컬럼에 대한 인덱스(옛날엔 순서가 중요했음)
- Covering index
	- 테이블 참조 없이 인덱스만 처리 가능한 경우
	- 일반적으로 성능이 우수(인덱스만 사용)
## Hint index
강제로 질의에서 특정 인덱스를 사용하도록 강요함

## Ohter types of indices
- 오라클의 기본 인덱스는 B+ tree임
- 비트맵
  ```sql
  create bitmap index bitmap_index[name] on branch(state)[table_name(col)]
```
- reverse index
- 컬럼 인덱스의 대표적인 문제 : 테이블의 기본키가 시퀀셜하게 증가하는 숫자일 경우
	- 일반적인 비트리의 이점
		- 새로운 데이터 삽입시 이전 노드들에 접근할 필요 없음
	- 단점
		- 새로운 인덱스 개체가 항상 리프노드 마지막에 위치하게 됨
	- 리버스 키 인덱스 : 키 앖을 뒤집은 것을 인덱스 키 값으로 사용하는 것
		- 인덱스에 무작위로 값을 뿌리는 방식
		- 장점 : contention 문제가 사라짐
		- 다점 : 새로 삽입 값이 인덱스의 앞 뒤로 분산시킴 -> 기존 항목 중에 새로운 리프노드 항목을 삽입함
- Function based index
	- 문제 : dbms에 저장된 데이터는 대소문자에 민감하지만 표준 sql은 아님
	- 해결법 : 
	- 더 나은 해결법 : 함수를 미리 걸어 인덱스 생성
