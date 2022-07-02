---
layout : single
title:  "[알고리즘]병합 정렬(Merge Sort)"
last_modified_at : 2022-07-02
categories: algorithm-concept
toc: true
toc_sticky: true
---

# 병합 정렬(Merge Sort)

## 정렬 순서
1. 리스트의 길이가 1 이하이면 이미 정렬된 것으로 본다.  
2. 분할(divide) : 정렬되지 않은 배열을 절반으로 잘라 두 배열로 나눈다.  
3. 정복(conquer) : 나눠진 두 배열을 재귀적으로 병합정렬을 사용해서 정렬한다.  
4. 결합(combine) : 두 배열을 다시 하나의 정렬된 배열로 합병한다.  

**분할 정복(divide and conquer)기법과 재귀 알고리즘을 이용하여 정렬하는 방식이다.**

## 수행 과정 예시

다음과 같이 정렬되지 않은 배열이 있다.
```
[6, 5, 3, 1, 8, 7, 2, 4]
```
절반으로 잘라 두 배열로 나눈다.
```
[6, 5, 3, 1] [8, 7, 2, 4]
```
다시 두 배열을 나눠 네 개로 나눈다.
```
[6, 5] [3, 1] [8, 7] [2, 4]
```
마지막으로 네 개의 배열을 여덟 개로 나눈다.
```
[6] [5] [3] [1] [8] [7] [2] [4]
```
더 이상 나눌 수 없으니 두 개씩 합친다. 합칠 때 작은수가 앞에 나오도록 합친다.
```
[5, 6] [1, 3] [7, 8] [2, 4]
```
```
[1, 3, 5, 6] [2, 4, 7, 8]
```
```
[1, 2, 3, 4, 5, 6, 7, 8]
```

## 시간복잡도
O(NlogN)

## 코드
```python

def merge_sort(l):
    n=len(l)

    if n<=1:
        return 
    
    mid=n//2
    left_list=l[:mid]
    right_list=l[mid:]

    merge_sort(left_list)
    merge_sort(right_list)
    
    i=j=0
    now=0
    while i<len(left_list) and j<len(right_list):
        if left_list[i]<right_list[j]:
            l[now]=left_list[i]
            now+=1
            i+=1
        else:
            l[now]=right_list[j]
            now+=1
            j+=1
    while i<len(left_list):
        l[now]=left_list[i]
        now+=1
        i+=1
    while j<len(right_list):
        l[now]=right_list[j]
        now+=1
        j+=1
```
