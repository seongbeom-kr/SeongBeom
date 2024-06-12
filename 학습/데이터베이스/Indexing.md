#DB 
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
![](https://lucid.app/publicSegments/view/7203baa6-7dce-4f04-98f1-f66ad6cb09d3/image.png)

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


- 데이터 블록마다 오직 한 개의 Key - Pointer 쌍을 보유한다.
- 훨씬 적은 공간을 사용하지만 주어진 레코드의 Key를 찾는데 더 많은 시간이 걸린다.
- ![](https://lucid.app/publicSegments/view/b86349a6-1468-49be-bc4f-893c5ec5fe19/image.png)
- Sparse index는 주어진 검색키 값보다 작거나 같은 값중 가장 큰 값을 찾는다.
- 이진탐색가능, 찾은 다음에는 블럭 탐색

## Multiple Level Index
-  인덱스 자체로는 많은 블록을 커버할 수 있는 장점이 있지만, 이진 탐색으로 인덱스를 찾더라도, 원하는 레코드를 찾고자하면 오래걸릴 수 있음
- 이럴때 인덱스를 위한 인덱스를 사용
![](https://lucid.app/publicSegments/view/395be250-b56a-4507-896c-9b217e909e79/image.png)

## Secondary Indices
- 종종, 사람들은 어떤 조건을 만족하는 특정 필드에 있는 값들을 가진 모든 레코드들을 찾길 원한다.
- secondary index를 사용해서 찾는다.
- search-key 값에 대한 인덱스 레코드가 있는 secondary index를 가질 수 있음
- index 레코드는 특정 검색키값을 가진 모든 실제 레코드에 대한 포인터를 포함하는 buket을 포인트한다
- secondary index는 정렬되어 있지 않은 값을 표시하기 때문에 Dense하지 않으면 찾아갈 수 없음(index가 모든 포인터를 포함한다.)

![](https://i.imgur.com/j31YaU4.png)

## Primary and Secondary Indices
- 인덱스는 레코드를 검색할 때마다 상당한 이익을 준다(효율적이기 때문에!)
- 그러나 데이터가 바뀔 때마다 인덱스를 모두 업데이트 해주어야 하기때문에 비용이 발생함
- 순차적인 스캔을 사용할 시 Primary index를 사용하는 것이 효율적, Secondary index는 비효율적
	- 레코드에 접근하는 방식은 새로운 블록을 디스크에서 갖고오는데 블록에 접근하는데 걸리는 시간은 메모리에 접근하는 시간보다 비효율적이다
	- 스캔은 레코드를 한번 훑는데 그때 primary index는 디스크에 시퀀셜하게 저장되어있기 때문에 쉽게 가져올 수 있음, 하지만 secondary index는 정렬되어있지 않기 때문에(시퀀셜하지 않음) 디스크를 올렸다 내렸다 반복하게 되어서 (한번에 가지고 오는것이 아니기 때문) 과부화가 발생한다.

## B+ tree index files
> [!note] 정렬된 인덱스 파일의 문제점 
> 파일 사이즈가 커질 수록 성능이 하락함
> 주기적인 전체 파일의 재구조화가 요구됨

- Leaf : 실제 포인터
- 비교값이 크거나 같으면 value(search key)다음 포인터를 찾아감
- Null pointer -> 가르키는 포인터가 없음(마지막에 삽입)

- B+ tree는 indexed-sequential files의 대안이다
- index 시퀀스 파일의 약점