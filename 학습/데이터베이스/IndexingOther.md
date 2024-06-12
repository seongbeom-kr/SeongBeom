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

