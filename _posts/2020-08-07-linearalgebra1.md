---
layout: post
title: 선형대수의 기초 데이터와 행렬
use_math: true
categories: data
tags: math
comments: false
keywords: linear algebra 

---

대학교때 교양과목으로 linear algebra가 뭐지? 그냥 algebra같은건가?하고 뭣도 모르고 듣고  
이제와서야 정말 필요한 학문이구나 하고 기초를 공부한다.  

[밝히리님](http://blog.daum.net/eigenvalue/10856412)이 정리해주신 선형대수의 기초, [다크프로그래머님](https://darkpgmr.tistory.com/103?category=460967)의 포스팅, [김도형님의 데이터사이언스 스쿨 수학편](https://datascienceschool.net/view-notebook/04358acdcf3347fc989c4cfc0ef6121c/)을 통해 공부를 하고 있고 나중에 카이스트 AI대학원에 계신 주재걸 교수님의 [edwith강의](https://www.edwith.org/linearalgebra4ai)도 들어볼 예정이다.

목차  
[데이터의 유형](#데이터의-유형)  
[전치(transpose)연산](#전치연산)  
[특수한 벡터와 행렬-영벡터,일벡터,정방행렬,대각행렬,단위행렬,대칭행렬](#특수한-벡터와-행렬)  

# 선형대수란?
어떤 함수가 선형함수일때 그 성질을 배우는것  

ex)
$$y = ax$$
   - 미분,적분
   - 회전변환,확대/축소변환

비선형함수는? 이차함수, 삼각함수, 로그함수, 지수함수등이 있다.

# 데이터의 유형

- 1) 스칼라(scalar)  
실수인 숫자 하나로 이루어진 데이터, 1차원벡터, 크기만 갖고 방향은 없음 

$$x\in \mathbf{R}$$
  
  
- 2) 벡터(vector)  
여러 개의 숫자가 특정한 순서대로 모여있는 것, 수의 순서쌍! 순서가 중요함 반대로 집합은 순서가 중요하지 않음   
다음은 4차원 열(column)벡터에 대한 예시다. (행(row)벡터도 존재)  

$$x=\vec{x} = \begin{bmatrix}x_{1} \\x_{2} \\x_{3} \\x_{4} \\\end{bmatrix}$ , $x \in \mathbf{R}^4$$



```python
# 벡터를 numpy로 표현
x = np.array([[1], [2], [3], [4]])
x
```




    array([[1],
           [2],
           [3],
           [4]])



벡터가 예측문제에서 입력 데이터로 사용되면 feature vector라고 한다.


```python
# 붓꽃데이터를 통한 feature vector의 예
from sklearn.datasets import load_iris

iris = load_iris()
iris.data[0, :] # 첫 번째 꽃의 데이터 
```




    array([5.1, 3.5, 1.4, 0.2])



첫번째 붓꽃의 꽃받침:5.1, 꽃받침폭:3.5, 꽃잎길이1.4, 꽃잎 폭:0.2 라고 하면 이 데이터 레코드를 $x_1$이라 하고 다음과 같이 표시한다.  

$$x_1 = 
\begin{bmatrix}
5.1 \\
3.5 \\
1.4 \\
0.2 \\  
\end{bmatrix}$$  
이러한 붓꽃 크기 벡터를 이용해 어떤 붓꽃종인지 결정하는 예측문제를 푼다면 붓꽃 크기 벡터는 특징벡터다.

- 3) 행렬(matrix)
백터들의 모임, 2D array of numbers  
예를 들면 붓꽃 6송이에 대해 꽃잎과 꽃받침의 크기를 층정한 4차원 붓꽃데이터가 6개를 표현 한 6 X 4 행(row)렬(column)이다.$X \in \mathbf{R}^{6\times 4}$  
아래첨자중 첫번째는 행, 두번째는 열을 뜻한다
$$X = 
\begin{bmatrix}
\boxed{\begin{matrix} x_{1, 1} & x_{1, 2} & x_{1, 3} & x_{1, 4}\end{matrix}}  \\
\begin{matrix} x_{2, 1} & x_{2, 2} & x_{2, 3} & x_{2, 4}\end{matrix} \\
\begin{matrix} x_{3, 1} & x_{3, 2} & x_{3, 3} & x_{3, 4}\end{matrix} \\
\begin{matrix} x_{4, 1} & x_{4, 2} & x_{4, 3} & x_{4, 4}\end{matrix} \\
\begin{matrix} x_{5, 1} & x_{5, 2} & x_{5, 3} & x_{5, 4}\end{matrix} \\
\begin{matrix} x_{6, 1} & x_{6, 2} & x_{6, 3} & x_{6, 4}\end{matrix} \\
\end{bmatrix}$$


```python
# 행렬을 numpy로 표현
np.array([[1,2,3],[4,5,6]])
```




    array([[1, 2, 3],
           [4, 5, 6]])



# 전치연산
(Transpose)

행렬의 행과 열을 바꾸는 연산  
벡터나 행렬에 $T$라는 위첨자(super-script)를 붙여서 표기  
$$x \;\; \rightarrow \;\; x^T$$

$$X = 
\begin{bmatrix}
\boxed{\begin{matrix} x_{1, 1} & x_{1, 2} & x_{1, 3} & x_{1, 4}\end{matrix}}  \\
\begin{matrix} x_{2, 1} & x_{2, 2} & x_{2, 3} & x_{2, 4}\end{matrix} \\
\begin{matrix} x_{3, 1} & x_{3, 2} & x_{3, 3} & x_{3, 4}\end{matrix} \\
\begin{matrix} x_{4, 1} & x_{4, 2} & x_{4, 3} & x_{4, 4}\end{matrix} \\
\begin{matrix} x_{5, 1} & x_{5, 2} & x_{5, 3} & x_{5, 4}\end{matrix} \\
\begin{matrix} x_{6, 1} & x_{6, 2} & x_{6, 3} & x_{6, 4}\end{matrix} \\
\end{bmatrix}
\;\; \rightarrow \;\;
X^T = 
\begin{bmatrix}
\boxed{\begin{matrix} x_{1, 1} \\ x_{1, 2} \\ x_{1, 3} \\ x_{1, 4}\end{matrix}} &
\begin{matrix} x_{2, 1} \\ x_{2, 2} \\ x_{2, 3} \\ x_{2, 4}\end{matrix} &
\begin{matrix} x_{3, 1} \\ x_{3, 2} \\ x_{3, 3} \\ x_{3, 4}\end{matrix} &
\begin{matrix} x_{4, 1} \\ x_{4, 2} \\ x_{4, 3} \\ x_{4, 4}\end{matrix} &
\begin{matrix} x_{5, 1} \\ x_{5, 2} \\ x_{5, 3} \\ x_{5, 4}\end{matrix} &
\begin{matrix} x_{6, 1} \\ x_{6, 2} \\ x_{6, 3} \\ x_{6, 4}\end{matrix} &
\end{bmatrix}$$


```python
#NumPy에서는 ndarray 객체의 T라는 속성을 이용하여 전치 행렬을 구함. T는 method가 아닌  attribute이므로 ()를 안붙임
A = np.array([[1,2,3],[4,5,6]])
A.T
```




    array([[1, 4],
           [2, 5],
           [3, 6]])



# 특수한 벡터와 행렬

- 1) 영벡터 $$\vec{0}$$  
모든 원소가 0인 N차원 벡터  
$$\mathbf{0}_N = \mathbf{0} = 0 =
\begin{bmatrix}
0 \\
0 \\
\vdots \\
0 \\
\end{bmatrix}$, $0 \in \mathbf{R}^{N \times 1} $$


```python
#numpy 에선 zeros()로 구현
np.zeros((3,1))
```




    array([[0.],
           [0.],
           [0.]])



- 2) 일벡터  
모든 원소가 1인 N차원 벡터  
$$\mathbf{1}_N = \mathbf{1}  = 1 = 
\begin{bmatrix}
1 \\
1 \\
\vdots \\
1 \\
\end{bmatrix}$, $1 \in \mathbf{R}^{N \times 1} $$


```python
# ones()로 구현
np.ones((3, 1))
```




    array([[1.],
           [1.],
           [1.]])



- 3) 정방 행렬(square matrix)  
행의 개수와 열의 개수가 같은 행렬  

(단위행렬,행렬의 거듭제곱, 역행렬일때 행렬의 교환법칙이 성립한다!)

$$X = 
\begin{bmatrix}
\begin{matrix} x_{1, 1} & x_{1, 2} & x_{1, 3} & x_{1, 4}\end{matrix}  \\
\begin{matrix} x_{2, 1} & x_{2, 2} & x_{2, 3} & x_{2, 4}\end{matrix} \\
\begin{matrix} x_{3, 1} & x_{3, 2} & x_{3, 3} & x_{3, 4}\end{matrix} \\
\begin{matrix} x_{4, 1} & x_{4, 2} & x_{4, 3} & x_{4, 4}\end{matrix} \\
\end{bmatrix}$$

---
정방행렬만의 성질(뒤에서 더 다룰 내용이지만 그냥 정리차원에서)  

정방행렬 A가 있을때   
$$A \cdot I = I \cdot A = A$$  단위행렬의 교환법칙이 성립  
$$A^2 = AA$$  
$$A^n = A^{n-1}{A} = {A}A^{n-1}$$  
$${A^m}A^n = A^{m+n}$$  
$${A^m}^n = A^{mn} = A^{nm} = {A^n}^m $$  
$$I^n = I$$

역행렬은 $$AX = XA = I$$를 만족하는 $$X$를 $A^-1$$,역행렬이라 한다.  
$$A^{-1} A = A A^{-1} = I$$ 나중에 더 자세히 다루겠다. 

정방행렬의 대각합(Trace)는 대각원소의 합으로 계산되며 뒤에 더 자세히 다루겠다.  
$$\text{tr}(A) = a_{11} + a_{22} + \dots + a_{NN}=\sum_{i=1}^{N} a_{ii}$$

정방행렬 행렬식(determinant) $\text{det}(A)$도 뒤에 더 자세히 다루겠다.

---

- 4) 대각 행렬(diagonal matrix)  
행렬에서 행과 열이 같은 위치를 대각(diagonal)이라 하고 그렇지 않은 것들을 비대각(off-diagnoal)이라한다.
대각행렬은 모든 비대각(off-diagonal) 요소가 0인 행렬이다.(반드시 정방행렬일 필요는 없다)

numpy에서는 diag 명령을 사용  
$$
D = 
\begin{bmatrix}
d_{1} & 0 & \cdots & 0 \\
0 & d_{2} & \cdots & 0 \\
\vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & d_{N} \\
\end{bmatrix}$$


```python
np.diag([1, 2, 3])
```




    array([[1, 0, 0],
           [0, 2, 0],
           [0, 0, 3]])




```python

```

- 5) 단위행렬(identity matrix)
대각 행렬중 모든 대각 성분의 값이 1인 대각행렬  
$$I$$ 로 표기한다.
$$I = 
\begin{bmatrix}
1 & 0 & \cdots & 0 \\
0 & 1 & \cdots & 0 \\
\vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & 1 \\
\end{bmatrix}$ , $I \in \mathbf{R}^{N \times N}$$


```python
#identity(), eye()명령을 사용한다
np.identity(3)
```




    array([[1., 0., 0.],
           [0., 1., 0.],
           [0., 0., 1.]])




```python
np.eye(3)
```




    array([[1., 0., 0.],
           [0., 1., 0.],
           [0., 0., 1.]])



- 6) 대칭행렬(symmetric matrix)  
정방행렬중 전치 행렬과 원래의 행렬이 같은 행렬  
$$S^T = S $, $S \in \mathbf{R}^{N \times N}$$ 


```python
S = np.array([[8, 1, 4], [1, 3, 7], [4, 7, 6]])
S
```




    array([[8, 1, 4],
           [1, 3, 7],
           [4, 7, 6]])




```python
St = S.T
St
```




    array([[8, 1, 4],
           [1, 3, 7],
           [4, 7, 6]])




```python
S == St
```




    array([[ True,  True,  True],
           [ True,  True,  True],
           [ True,  True,  True]])


