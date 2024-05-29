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
	- Non - clustering

- Index - sequential file : 기본인덱스를 가진 순차정렬 파일 

## Dense Index Files 
- Dense index : 인덱스 레코드는 파일에 있는 모든 검색키 값에 대해 나타낸다
- `Dense`라 불리는 이유는 데이터 파일의 모든 키가 인덱스에서 표현되기때문이다.
- 덴스 인덱스는 Key들을 그 파일 자체 순서와 동일하게 유지된다.
- 키와 포인터만 보유한 인덱스는, 완전한 레코드보다 훨씬 적은 공간을 차지하기 때문에 파일 자체보다 훨씬 적은 공간을 소모할 것이다.
- 즉, 파일보단 메모리를 위해 설계된 것이다.
- 또한 인덱스를 사용하면 searchKey를 통해 어떤 레코드던 찾을 수 있다.
![](https://i.imgur.com/TjH8ZDF.png)

- Dens index는 인덱스 블록을 주어진 Search Key 값으로 검색하며, 찾을 경우 그 포인터로 연관된 레코드를 따라간다.
- 이때 찾을때까지 인덱스의 모든 블록을 탐색(평균적으로는 절반만큼)
- 성능이 괜찮음

왜?
1. 인덱스 블록의 수는 대개 데이터 블록의 수보다 비교적 적다
2. 키값들이 정렬되어있기 때문에 이진탐색을 할 수 있다. $log_2n$
3. 인덱스는 메인 메모리 버퍼에 영구 유지될 수 있을 만큼 작다

## Sparse Index Files 
> - 하지만 Dense index는 모든키를 가지고 있기에는 비교적 크다
> - 이런 단점을 보완하여 설계한 것이 바로 Sparse Index라고 한다.


- 데이터 블록마다 오직 한 개의 Key 

- 


