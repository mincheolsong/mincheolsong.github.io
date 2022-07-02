---
layout : single
title:  "[알고리즘]카운팅 정렬(Counting Sort)"
last_modified_at : 2022-07-02
categories: algorithm-concept
toc: true
toc_sticky: true
---

# 카운팅 정렬(Counting Sort)

## 과정
배열에 존재하는 수의 개수를 세어서, 이를 바탕으로 정렬을 수행한다.  

### 1.배열에 존재하는 각 값의 갯수를 저장하는 count 변수를 생성한다.
예를들어 `count[1]=4` 라면 배열에는 `1`의 값이 4개 있다는 것을 의미한다.

- count는 배열 원소의 최댓값까지를 인덱스로 사용한다.

```python
arr = [4, 7, 9, 1, 3, 5, 2, 3, 4]

cnt = [0] * (max(arr) + 1)

for num in arr:
    cnt[num] += 1
    
print(cnt)

# [0, 1, 1, 2, 2, 1, 0, 1, 0, 1]
```

### 2. count배열을 누적합으로 계산하여 갱신하여 준다.  
누적합으로 갱신하는 이유는 리턴 할 정렬된 배열 `answer`의 적절한 위치에 삽입하기 위함이다.  
예를들어 `cnt=[0,1,2,4,6]`이라고 하자.  
cnt[3]=4 인데, 이를 이용하면 3이 정렬 되었을 때 4번째에 위치한다는것을 알 수 있다.

```python
for i in range(1,len(cnt)):
    cnt[i]+=cnt[i-1]

print(cnt)
# [0, 1, 2, 4, 6, 7, 7, 8, 8, 9]
```

### 3. cnt를 사용하여 정렬된 배열 result를 생성한다.
배열 result는 arr과 똑같은 크기로 생성한다.  
그리고 cnt를 사용하여 result의 올바른 위치에 arr의 값을 삽입한다.  
여기서 idx=cnt[i]-1로 하는 이유는 인덱스는 0번 부터 시작하기 때문이다.   
숫자 하나를 result에 적절히 삽입하였다면, cnt의 갯수도 하나 줄여준다.  
```python
result=[0]*len(arr)

for i in arr:
    idx=cnt[i]-1
    rest[idx]=i
    cnt[i]-=1

print(result)
# [1, 2, 3, 3, 4, 4, 5, 7, 9]
```

## 시간복잡도
 O(n + k)의 시간복잡도를 가진다.  
 여기서 k는 정렬할 배열의 가장 큰 값을 의미한다.  
 k에 따라 시간복잡도에 영향을 미치기 때문에 k가 단순 상수로 취급되어 생락되지 않고 남아있는 것이다.    
 k가 n보다 작다면 O(n)이지만, k가 매우크다면 시간복잡도는 O(k)가 될 것이다.

## 단점
정렬하고자 하는 배열의 최댓값이 매우 클 때 cnt배열을 그 크기만큼 생성해야 하기때문에 메모리를 비효율적으로 사용하게 된다.    
또한 cnt배열의 인덱스를 통해 arr배열 원소의 갯수를 세어주기 때문에 arr의 원소값 중 음수가 있다면 사용 할 수 없다.

## 전체코드

```python
def counting_sort(arr):
    cnt=[0]*(max(arr)+1)
    result=[0]*len(arr)

    for i in arr:
        cnt[i]+=1
    
    for i in range(1,len(cnt)):
        cnt[i]+=cnt[i-1]
    
    for i in arr:
        idx=cnt[i]-1
        result[idx]=i
        cnt[i]-=1
    
    return result
```