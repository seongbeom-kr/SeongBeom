> [!note]+
> 요구사항 명세 설계단계
> 
> - 아키텍쳐 디자인 결정
> - 관점들
> - 패턴들
> - 아키텍쳐 적용

# Software architecture
- 아키텍쳐 설계
	- 시스템을 구성하는 서브시스템 및 서브시스템간의 제어 및 통신을 식별하는 과정
	- 명세 및 설계간 링크 역할 
	- (object, class 아님, 시스템 단위라 추상화단위가 큼)

- 소프트웨어 아키텍쳐 : 아키텍쳐 설계의 산출물
- 장점
	- stakeholder간의 의사소통 증진
	- 시스템 분석 수행시 시스템의 비기능적 요구사항 달성 여부 분석
	- 큰 규모의 재사용 : 아키텍쳐 단위로 여러 시스템에 재사용될 수 있음


## 아키텍쳐 추상화
- 작은 단위에서 아키텍쳐
	- 프로그램 단위의 아키텍쳐
	- 단위 프로그램이 어떠한 컴포넌트로 구성되었는지 연구
- 큰 단위의 아키텍쳐
	- 복잡한 엔터프라이즈 시스템의 아키텍쳐를 다룸
		- 엔터프라이즈 시스템은 대개의 경우 다수의 컴퓨터에 분산되며 여러 회사에 의해 소유되고 운영됨
	- 엔터프라이즈 시스템을 이루는 타 시스템, 프로그램 및 프로그램 컴포넌트에 대해 연구

# 아키텍쳐 설계 결정
## Architectural design decisions
- 아키텍쳐 설계는 개발하고자 하는 시스템에 따라 다르게 진행되나, 다음과 같은 의사결정은 공통적으로 이루어짐
	- 사용가능한 일반적인 어플리케이션 아키텍쳐가 있는가?
	- 시스템이 어떻게 분산될 수 있는가?
	- 어떤 스타일이 좋은가?
	- 시스템을 구조화하기 위해 어떤 접근법을 사용할 수 있는가?
	- 시스템이 어떻게 모듈로나누어 질 수 있는가?
	- 어떤 통제 전략을 사용할 수 있는가?
	- 어떻게 평가?
	- 문서화는 또 어떻게?

## Architecture and system characteristics
- performance
	- 중요 오퍼레이션을 국소화시키고, 커뮤니케이션을 최소화
	- 작은 단위 컴포넌트보다는 큰단위 컴포넌트를 사용
- Security 
	- 계층적 아키텍쳐를 사용하고, 중요한 자산은 안쪽 게층에 위치시킴
- Safety
	- 안전에 영향을 주는 중요한 features는 작은 수의 sub-system에 위치시켜 국소화사킬거 -> 집중화(펼치기 x)
- Availability(가용성)
	- 중복된 컴포넌트를 포함시키고, 결함 허용을 위한 케머니즘을 도입
- Mainainability
	- 작은 크기의 컴포넌트를 사용, 대체할 수 있는 컴포넌트 사용

## Architectural views
- 각 아키텍쳐 모델은 시스템의 특정 관점만을 보기 때문에 여러 view를 통해 시스템의 아키텍쳐 묘사하는 것이 좋음

## 4 + 1 View model of software architecture
- logical view : object, object classes와 같은 주요 추상화내용을 보임
- process view : 시스템이 수행중에 어떤 프로세스들로 구성되는지를 보임
- Development(implementation) view : 소프트웨어가 개발과정에서 어떻게 나누어지는가를 보임
- Physical view : 시스템을 구성하는 하드웨어와 소프트웨어가 어떻게 분산되어 있는지를 보임
- 유즈케이스나 시나리오를 사용해서 (+1)

![](https://i.imgur.com/i0jRrya.png)
![](https://i.imgur.com/hT57zg9.png)


## Document
각 관점에 대해 작성해야함
# Architectural Patterns
## Archtectural Patterns
- Patterns : 지식을 표현하고, 공유하고, 재사용하기 위한 수단
- An architectural patterns : 여러 환경에서 시도, 테스트되어 유용함이 입증된 사례
- 표현 방법 : 표나 그림, 설명도 포함

### MVC 패턴
표현, 연결, 시스템 데이터를 구분 - 3개의 논리적 컴포넌트로 구조화됨
	- model component : 시스템 데이터 및 데이터와 연관된 operations을 관리하는 컴포넌트
	- View component : 데이터가 사용자에게 표현되는

