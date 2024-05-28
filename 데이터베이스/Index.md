# Indexing
> Indexing Basic Concept
> Ordered Indices
> $B^{+}$ - Tree Index Files
> $B^{-}$ - Tree Index Files 


## Basic Concept
인덱스는 데이터에 효율적인 접근을 돕는 데이터 구조다.
![[스크린샷 2024-05-28 11.23.40.png]]


- 인덱싱 메커니즘은 원하는 데이터에 접근 속도를 높이기 위해 사용된다.

- __Search Key__ : 파일에서 튜플(레코드)를 찾기 위해 사용되는 속성들의 집합(또는 속성 하나)

- __Index file__ 은 레코드 형태로 구성된다. // index entries or record

- 인덱스 파일들은 원본 파일보다 매우 작음

- Indices의 두가지 기본 종류
	- 정렬된 Indices : Search Keys가 정렬된 상태로 저장된다.
	- Hash Indices : 해쉬함수를 사용

## Ordered Indices $\star^5$ 
- 정렬된 인덱스에서, 인덱스 엔트리들은 search key 값에 정렬된 상태로 저장된다.

- Primary index : 순차적으로 저장된 파일에서는, search key의 인덱스는 파일의 순차적인 정렬을 명시한다.
	- 클러스터링 인덱스라고도 함
	- 기본 인덱스의 search key는 보통 기본키이지만 필수는 아니다.
- Secondary index : 검색키의 인덱스는 순차정렬된 파일과 다른 형태의 정렬을 명시한다.
	- Non - c




