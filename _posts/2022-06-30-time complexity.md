---
layout : single
title:  "[알고리즘]시간복잡도"
last_modified_at : 2022-06-30
categories: algorithm-concept
toc: true
toc_sticky: true
---

## 시간복잡도 = 알고리즘의 <span style="color:red">자원(resource) 사용량</span>을 분석

자원이란 실행시간, *메모리*, 저장장치, 통신 등을 말하는 것 인데 그 중에서 **실행시간**에 대해서 표현
> 메모리 : 가격대비 메모리 용량이 크게 증가하여 상대적인 중요성이 감소되었다.

**실행시간**은 하드웨어, 운영체제, 언어, 컴파일러 등에 따라서 달라지게 되므로 실행시간을 측정하는 대신 <span style="color:red">**연산의 실행 횟수**</span>를 측정해야 한다.  

## 연산의 실행 횟수
- 입력 데이터의 크기(n)에 관한 함수로 표현하게 된다.
- 데이터의 크기가 같더라도 실제 데이터에 따라서 달라진다.     
 (n개의 데이터에서 검색을 할 때, 운이 좋으면 바로 찾을 수도 있고, 운이 나쁘면 n개의 데이터를 모두 검색해서 찾아야 할 수도 있다.)
     - 최악의 경우 시간복잡도
     - 평균 시간복잡도   
    (평균 시간복잡도 분석이 어려워서 대부분 최악의 시간복잡도를 사용한다.)

## 점근적 분석
데이터의 갯수 n이 무한대로 증가할 때 수행시간이 증가하는 growth rate로 시간복잡도를 표현하는 기법이다.
예를들어 다음과 같이 표현하는 것이다.
![image](https://user-images.githubusercontent.com/80660585/176641910-2c33bf19-96b1-402e-a2b2-1f602e4538d5.png)
=>
![image](https://user-images.githubusercontent.com/80660585/176642206-b1fe075a-de45-48e8-a9ab-8451ca5dec99.png)

### 점근적 분석의 예
```c
int sum(int data[], int n){
    int sum=0;
    for (int i=0; i<n; i++)
        sum += data[i]; // 가장 자주 실행되는 문장
    return sum
}
```
위 코드에서 `sum+=data[i]` 문장이 가장 자주 실행되며, 항상 n번 실행된다.  
가장 자주 실행되는 문장의 실행횟수가 n번이라면 모든 문장의 실행 횟수의 합은 n에 선형적으로 비례하게 된다.   
모든 연산들의 실행횟수의 합도 역시 n에 선형적으로 비례하게 된다.  
O(n)의 시간복잡도를 가진다고 할 수 있다.

---
```c
bool is_distinct(int n, int x[]){
    for (int i=0; i<n-1; i++)
        for (int j=i+1; j<n; j++)
            if (x[i]==x[j]) // 가장 자주 실행되는 문장
                return false;
    return true;
}
```
배열 x에 중복된 원소가 있는지 검사하는 코드이다.  
위 문장에서 가장 자주 실행되는 문장은 `if(x[i]==x[j])` 문장이다.  

i=0 일 경우 1 ~ n-1 : n-1번 실행  
i=1 일 경우 2 ~ n-1 : n-2번 실행  
i=2 일 경우 3 ~ n-1 : n-3번 실행  
            .  
            .  
            .  
i=n-2 일 경우 n-1 ~ n-1 : 1번 실행

즉 최악의 경우 1+2+ ... + n-1 = n(n-1)/2번 실행된다. 
최악의 경우 O(n**2)의 시간복잡도를 가진다고 말할 수 있다.

---
```c
for (i=1; i<n; i*=2){
    //Do something
}
```
위 코드는 O(logn)의 시간복잡도를 갖는다.  

## 점근적 표기법
알고리즘에 포함된 연산들의 실행 횟수를 표기하는 하나의 기법이다.

### 빅오 표기법 (O, big-O)
![image](https://user-images.githubusercontent.com/80660585/176674584-7ab9503c-038a-4fb7-853e-f26b84808b31.png)

(n이 n0 이상일 때) g(n)에 적절한 상수 c를 곱하면 f(n)에 대하여 upper bound가 되는것을 표현한다. 즉 최악의 경우를 표현하는 것이다.

### 빅-오메가 표기법 (Big-Omega)
![image](https://user-images.githubusercontent.com/80660585/176675236-b61f494f-b0c1-44b8-9d71-d09d9bdeeb11.png)  
빅오 표기법과 반대의 의미인 lower bound가 되는것을 표현한다. 즉 최선의 경우를 표현하는 것이다.

### 세타 표기법 (Theta) 
![image](https://user-images.githubusercontent.com/80660585/176675587-a6d0f649-c780-47d6-810d-4c586250daca.png)  
(n이 n0 이상일 때) g(n)에 적절한 상수 c를 곱하면 f(n)에 대하여 upper bound가 될 수도 있고, lower bound가 될 수도 있는것을 표현한다. 즉 최선과 최악의 중간적인 평균적인 복잡도 있다.  
다시 말해 f(n)과 g(n)은 **동일차수**인 것이다.

## 이진검색의 시간복잡도
```python
def binary_search(l,target):
    start=0
    end=len(l)-1
    while start<=end:
        mid = (start+end)//2
        if target==l[mid]:
            return mid
        elif target > l[mid]:
            start = mid+1
        elif target < l[mid]:
            end = mid-1
    return False
```
> 이진검색을 하려면 배열(리스트)에 데이터들이 오름차순으로 정렬되어 있어야 한다.

한 번 비교할 때마다 남아있는 데이터가 절반으로 줄어든다. 원래 데이터가 n개 있다면 최악의 경우 몇번 반복해야 1개가 되는가? 로 생각 할 수 있다.  
즉 O(log n)의 시간복잡도를 갖는다.

순차검색의 시간복잡도는 O(n)이고 이진검색의 시간복잡도는 O(log n)인데 이 둘의 차이는 매우 큰 것이다.   
따라서 이진검색을 할 수 있는 조건(데이터가 정렬되어 있음)에서는 순차검색 보단 이진검색을 하는것이 시간복잡도 측면에서 훨씬 유리한 것이다.

## 버블 정렬의 시간복잡도
```c
void bubbleSort(int data[], int n){
    for(int i=n-1; i>0; i--){
        for(int j=0; j<i; j++){
            if(data[j]>data[j+1]){
                int tmp=data[j];
                data[j]=data[j+1];
                data[j+1]=tmp;
            }
        }
    }
}
```
`if(data[j]>data[j+1])`문이 가장 자주실행되는 문장이다.  
i=n-1 일 경우 n-1번 반복  
i=n-2 일 경우 n-2번 반복  
.  
.  
.  
i=2 일 경우 2번 반복  
i-1 일 경우 1번 반복  

1+2+...+n-1 = n(n-1)/2   
즉 O(n**2)의 시간복잡도를 갖는다.

## 삽입정렬의 시간복잡도
```c
void insertion_sort(int n, int data[]){
    for(int i=1;i<n;i++){
        int tmp=data[i];
        int j=i-1;
        while(j>=0 && data[j]>tmp){
            data[j+1]=data[j];
            j--;
        }
        data[j+1]=tmp;
    }
}
```
![image](https://user-images.githubusercontent.com/80660585/176682777-20dffc49-63e9-41ff-951b-c42d03eadc99.png) 

최선의 경우(정렬되어 있는경우) while문을 1번만 실행하므로 O(n)의 시간복잡도를 갖는다.  
최악의 경우(반대로 정렬되어 있는 경우) n(n-1)/2번 실행될 것이므로 O(n**2)의 시간복잡도를 갖는다.  







