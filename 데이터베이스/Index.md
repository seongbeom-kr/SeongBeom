# Indexing
> Indexing Basic Concept
> Ordered Indices
> $B^{+}$ - Tree Index Files
> $B^{-}$ - Tree Index Files 


## Basic Concept
인덱스는 데이터에 효율적인 접근을 돕는 데이터 구조다.
![[스크린샷 2024-05-28 11.23.40.png]]


인덱싱 메커니즘은 원하는 데이터에 접근 속도를 높이기 위해 사용된다.

__Search Key__ : 파일에서 튜플(레코드)를 찾기 위해 사용되는 속성들의 집합(또는 속성 하나)

__Index file__ 은 레코드 형태로 구성된다. // index entries or record

인덱스 파일들은 원본 파일보다 매우 작

