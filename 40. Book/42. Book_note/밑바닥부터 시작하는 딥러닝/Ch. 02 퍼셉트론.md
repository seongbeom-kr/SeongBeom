---
cssclasses:
  - img-grid
---

## 1. 퍼셉트론이란?
>[!note]+
>퍼셉트론은 다수의 신호를 입력받아 하나의 신호를 출력한다.
>
>
>


![](https://i.imgur.com/AH1iLbB.png)

$x_1, x_2$ 는 입력신호, $y$는 출력신호 $w_1, w_2$는 가중치를 뜻한다

원은 뉴런 혹은 노드라 부름

뉴런에서 보내온 신호의 총합이 정해진 한계를 넘어설 때만 1을 출력한다. 이를 뉴런이 활성화한다고 표현한다.

그 한계를 임계값이라 하며, $\theta$기호로 표기한다.

이를 수식으로 나타내면 다음과 같다.

$$
y=
\begin{cases}
0, \; w_1x_1 + w_2x_2 \leq \theta\\
1, \; w_1x_1 + w_2x_2 \gt \theta
\end{cases}
$$

이 수식에서 중요한 점은 가중치의 값($w_1, w_2$)의 값이 클수록 그 신호가 중요하다는 뜻이 된다.

## 2. 단순 논리 회로
> [!note]+ 
> 여기서는 AND, NAND, OR 게이트에 정리하는 페이지
> 
> 각 게이트에 대한 설명은 그림으로 대체


![](https://i.imgur.com/PdetrBv.png)

> [!notice] 
> 여기서 알아야하는 점은 퍼셉트론의 구조는 AND, NAND, OR게이트 모두 똑같다는 점이다.


세 가지 게이트에서 다른 것은 매개변수의 값(가중치, 임계값)이다. 

## 3. 퍼셉트론 구현하기

### 간단한 구현
```python
def AND(x1, x2):
	w1, w2, theta = 0.5, 0.5, 0.7
	temp = x1*w1 + x2*w2
	if temp <= theta:
		return 0
	else:
		return 1
```

매개변수 $w_1, w_2, \theta$ 는 함수 안에서 초기화 되고 가중치를 곱한 입력의 총합이 임계값을 넘으면 1을 반환, 아니면 0을 반환

### 가중치와 편향 도입
세타 대신 다른 변수를 도입해서 사용한 식은 다음과 같음

$$y = 
\begin{cases}
0, \; (b+w_1x_1+w_2x_2 \leq 0)\\
1, \; (b+w_1x_1+w_2x_2 \gt 0)
\end{cases}$$
여기서 $b$를 편향이라 하며 나머지는 가중치, 입력값임

이를 풀어 설명한다면, 퍼셉트론은 입력값에 가중치를 곱한 값과 편향을 합하여, 0을 넘으면 1, 아니면 0을 출력한다.

이를 이용한 코드는 다음과 같음
```python
import numpy as np
x = np.array([0,1]) # 입력
w = np.array([0.5, 0.5]) # 가중치
b = -0.7

if np.sum(w*x)+b <=0:
	print(0)
else:
	print(1)
```

### 가중치와 편향 구현하기
이를 함수화 하면 다음과 같음
```python
def AND(x1, ×2) :
	x = np. array ([x1, x2])
	w = np. array ([0.5, 0.5])
	b= -0.7
	tmp = np. sum (w*x) + b 
	if tmp <= 0:
		return 0
	else:
		return 1
```

$w_1, w_2$는 각 입력 신호가 결과에 주는 영향력을 조절하는 매개변수

편향은 뉴런이 얼마나 쉽게 활성화하느냐를 조정하는 매개변수

NAND, OR도 다음과 같음
```python
def NAND(x1, x2) :
	x= np. array ([x1, x2])
	W= np.array([-0.5, -0.5])#AND와는가중치(w와b)만다르다! 
	b = 0.7
	tmp = np. sum (w*x) + b 
	if tmp=< 0:
		return 0 
	else:
		return 1

def OR(x1, x2):
	x= np. array ([x1, x2]) 
	w= np. array([0,5, 0,5]) 
	b =-0.2
	tmp = np.sum(w*x)+ b
	if tmp=< 0: 
		return 0
	else: 
		return 1
```


코드를 보면 알겠지만 각 게이트들은 그냥 매개변수(가중치와 편향)의 차이일 뿐

## 4. 퍼셉트론의 한계
### XOR 게이트
하지만 단일 퍼셉트론으로는 XOR 게이트를 구현할 수 없음
![300](https://i.imgur.com/B0th1K6.png)

OR게이트를 시각적으로 표현하면 다음과 같음
$$y=
\begin{cases}
0 \; (-0.5+x_1+x+2 \leq0)\\
1 \; (-0.5+x_1+x+2\gt0)
\end{cases}$$


![](https://i.imgur.com/SO0s5ce.png)
![](https://i.imgur.com/vXcmnI0.png)

이를 두고 생각했을 때에 XOR게이트를 나타내기 위해서는 하나의 직선으로는 생각할 수 없음


### 선형과 비선형
직선의 제약을 벗어던지면 괜찮음, 곡선으로 그리면 가능하니깐!

## 5. 다층 퍼셉트론
하나가 안된다면, 층을 쌓아서 표현하면 된다!

### 기존 게이트 조합하기

![](https://i.imgur.com/MqAc7yF.png)

![](https://i.imgur.com/kegWho4.png)

진리표는 다음과 같다.


![400](https://i.imgur.com/TXzmIN9.png)

### XOR 게이트 구현하기
이걸 토대로 구현한다면?

```python
def XOR(x1, x2):
	s1 = NAND(x1, x2)
	s2 = OR(x1, x2)
	y = AND(s1, s2)
	return y
```

이를 도식화하면 다음과 같음

![](https://i.imgur.com/Rf82Xke.png)

순서대로 입력층, 은닉층, 출력층이며, 층이 여러 개인 퍼셉트론을 **다층퍼셉트론**이라 한다.

동작을 자세히 설명하자면
1. 입력층의 뉴런으로부터 입력을 받아 은닉층으로 출력을 보낸다.
2. 은닉층의 뉴런이 출력층으로 결과를 출력하고, 출력층은 최종 결과를 출력한다.

>[!summary]
>퍼셉트론은 가중치와 편향을 매개변수로 가지는 모델이다.
>논리회로를 표현할 수 있다
>다층 퍼셉트론이라면 비선형 영역도 표현할 수 있다.

