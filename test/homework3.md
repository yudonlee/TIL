# Homework3

# 목차

- 0. 실행방법 및 컴파일 옵션
- 1. result for 5 method(Bisection, Linear interpolation, Secant, Newton-Rhapson, Newton with Bracketing)
- 2. muller method
- 3. Problems

# 실행방법 및 컴파일 옵션

## 컴파일 옵션

```
g++ -o main main.cpp problem1.cpp problem2.cpp problem3.cpp problem4.cpp bessj0.cpp bessj1.cpp zbrak.cpp rtbis.cpp rtflsp.cpp rtnewt.cpp rtsafe.cpp rtsec.cpp muller.cpp
```

## 실행방법

```
sh compile.sh
```

해당 명령어를 통해서 컴파일되어 만들어진 main.exe 실행파일을 통해서 실행합니다.
실제 구현파일은 main.cpp, muller.cpp, func_problem.h입니다.

# 1. Result for 5 method

<img src="https://github.com/yudonlee/TIL/blob/main/test/Image/zbrak.png" width="1000" height="200">
        
zbark 함수 결과

## Bisection

<img src="https://github.com/yudonlee/TIL/blob/main/test/Image/Bisection.png" width="1000" height="400">

## Linear interpolation

<img src="https://github.com/yudonlee/TIL/blob/main/test/Image/LinearInterpolation.png" width="1000" height="400">

## Secant

<img src="https://github.com/yudonlee/TIL/blob/main/test/Image/Secant.png" width="1000" height="400">

## Newton-Raphson

<img src="https://github.com/yudonlee/TIL/blob/main/test/Image/NewtonRaphson.png" width="1000" height="400">

## Newton with bracketing

<img src="https://github.com/yudonlee/TIL/blob/main/test/Image/NewtonWithBracketing.png" width="1000" height="400">

# 2. Muller method

## 상세구현

MullerMethod는 muller.cpp 파일에 구현하였습니다. 이때 Muller method의 최대 Iteration 반복횟수는 40회까지 하였으며, 그전에 x1과 x2의 범위를 벗어날 경우 그 직전의 root값을 리턴하도록 설계하였습니다.

## 결과값

<img src="https://github.com/yudonlee/TIL/blob/main/test/Image/MullerMethod.png" width="1000" height="400">

bejessj0에 대한 muller method의 결과값입니다.

# Problems

python matplotlib로 그린 그래프의 값들과 Newton with bracketing에서 나온 값들을 비교했습니다.

## Problem1

f(x) = 10 _ exp(-x) _ sin(2 _ M_PI _ x)-2
scope : [0.1, 1.0]
<img src="https://github.com/yudonlee/TIL/blob/main/test/Image/Problem1Graph.png" width="1000" height="400">
<그래프>
<img src="https://github.com/yudonlee/TIL/blob/main/test/Image/Problem1.png" width="1000" height="200">
<rtsafe()의 결과>  
그래프에서의 값과 함수값의 결과가 근사합니다.

## Problem2

f(x) = pow(x, 2) - 2 _ x _ exp(-1 * x) + exp(-2*x)
scope : [0.0, 1.0]
<img src="https://github.com/yudonlee/TIL/blob/main/test/Image/Problem2Graph.png" width="1000" height="400">
<그래프>
<img src="https://github.com/yudonlee/TIL/blob/main/test/Image/Problem2.png" width="1000" height="200">
<rtsafe()의 결과>  
해당 함수의 경우 중근이 존재하지만 rtsafe() method로 계산하는 것이 불가능했습니다.

## Problem3

f(x) = cos(x + sqrt(2)) + x\*(x/2 + sqrt(2))
scope : [0.0, 1.0]
<img src="https://github.com/yudonlee/TIL/blob/main/test/Image/Problem3Graph.png" width="1000" height="400">
<그래프>
<img src="https://github.com/yudonlee/TIL/blob/main/test/Image/Problem3.png" width="1000" height="200">
<rtsafe()의 결과>  
해당 함수의 경우 실제로 하나의 근이 존재하지만 rtsafe() method로 계산하는 것이 불가능했습니다.

## Problem4

f(x) = sin(x)
scope : [0.1, 6]
<img src="https://github.com/yudonlee/TIL/blob/main/test/Image/Problem4Graph.png" width="1000" height="400">
<그래프>
<img src="https://github.com/yudonlee/TIL/blob/main/test/Image/Problem4.png" width="1000" height="200">
<rtsafe()의 결과>  
sin함수는 [0.1, 6]의 범위에서는 pi값이 나와야하고 해당 값과 근사한 값이 출력되는 것을 확인했습니다.
