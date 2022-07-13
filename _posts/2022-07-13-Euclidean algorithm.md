---
layout : single
title:  "[알고리즘]최대 공약수(GCD) 알고리즘 - 유클리드 호제법"
last_modified_at : 2022-07-13
categories: algorithm-concept
toc: true
toc_sticky: true
---

## 기존 GCD구하는 방법
```python
for i in range(min(a,b),0,-1):
    if a%i==0 and b%i==0:
        print(i)
        break
```
a와 b중 작은 숫자부터 시작해서 1까지 감소하면서, 제일 처음 a와 b모두 나눠떨어트리는 수를 찾는 방법이다.  
이러한 경우는 무차별 대입(Brute Force)로 O(N)의 시간복잡도를 갖는다.

## 유클리드 호제법 (Euclidean Algorithm)

```python
def gcd(a,b):  # a>b
    if b==0:
        return a
    c=a%b
    return gcd(b,c)
```
a를 b로 나눈 나머지를 r이라고 했을때, a와 b의 최대공약수는 b와 r의 최대공약수와 같다는 성질을 이용한 것이다.(`재귀함수 이용`)    
이러한 성질을 활용하여 b와 r의 최대공약수 r0를 구하고 r을 r0로 나눈 나머지를 구하는 과정을 반복하여 나머지가 0 이 되었을때 그 몫이 a와 b의 최대공약수가 된다.
이렇게 할 경우 O(logN)의 시간복잡도를 가진다  

> 기존의 방법보다 시간복잡도를 개선할 수 있다.

## 최소공배수
최소공배수=두 자연수의 곱 / 최대공약수 이다.  
즉 `(a*b)//gcd(a,b)`가 된다. 