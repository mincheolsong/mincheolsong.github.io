---
layout : single
title:  "sign language"
last_modified_at : 2022-06-04
categories: signLanguage
---

## 제작동기
**[청각장애인을 위한 인공지능]**    
&nbsp;인공지능 기술이 급속도로 발전하고 있는 현재, 이를 활용하여 일상생활에 불편을 느끼는 장애인 분들을 위한 제품을 만들어 보고 싶었다. 그 중 청각장애인을 위한 수화인식 프로그램을 만들면 좋겠다고 생각하게 되었다.    
&nbsp;MediaPipe 라이브러리를 사용하여 손의 joint를 검출한다. 그 출력 결과를 단일 동작으로 분류 할 수 있는 KNN(K-Nearest Neighbor)과 연속 동작을 분류 할 수 있는 RNN(Recurrent Neural Network)의 입력으로 학습하여 지화를 실시간으로 인식하고 출력한다.

## 이론적 배경
### MediaPipe  
&nbsp;MediaPipe란 구글에서 제공하는 AI 프레임워크로써, 비디오 형식 데이터를 이용한 다양한 비전 AI기능을 파이프라인 형태로 손쉽게 사용할 수 있도록 제공된다. AI 모델 개발 및 수많은 데이터셋을 이용한 학습도 마친 사태로 제공되므로 라이브러리를 불러 사용하듯이 간편하게 호출하여 사용하기만 하면 되는 형태로, 비전 AI 기능을 개발 할 수 있다. 

![Alt text](/img/signlanguage/hand_crops.png)
![Alt text](/img/signlanguage/mediapipe.png)
&nbsp;다음과 같이 손을 인식하여 준다. 이때 빨간점으로 표시된 마디(joint)를 사용하여 벡터를 만들고, 그 벡터들 사이의 각도를 이용해서 손 모양을 학습시킬 수 있다.  

### KNN(K-Nearest Neighbor)       
knn은 최근접 이웃 알고리즘으로 *지도학습* 알고리즘 중 하나이다.
>* **지도학습** : 여러 문제와 답을 같이 학습함으로써 미지의 문제애 대한 올바른 답을 예측하고자 하는 방법이다. 따라서 지도학습에는 문제와 함께 그 정답까지 알고있는 데이터가 제공되어야 한다.
>* **비지도학습** : 지도학습과는 달리 **정답 라벨이 없는 데이터**를 비슷한 특징끼리 군집화 하여 새로운 데이터에 대한 결과를 예측하는 방법이다.

새로운 데이터를 입력 받았을때, 해당 데이터와 가장 가까이에 있는 k개의 데이터를 확인해, 새로운 데이터의 특성을 파악하는 방법이다.
<center><img src="/img/signlanguage/knn.png" width="500" height="300"></center>

&nbsp; **위 그림에서 '?'의 주변에 가장 가깝고 많이 있는것이 무엇인가** 를 확인하여 '?'가 삼각형인지 원인지 판별하는 것이다.
* K가 1일경우 '?'주변에는 가장 가깝고 많이 있는것은 원 하나이므로 '?'는 원으로 판별한다.
* K가 4일 경우 '?'주변에 가장가깝고 많이 있는것은 삼각형이므로 '?'는 삼각형으로 판별한다.

**그러면 K는 몇으로 설정하는게 가장 좋을까?**
* k가 낮다 → 적은 이웃 수로 판단한다 → 불안정한 결과 
* k가 높다 → 많은 이웃 수로 판단한다 → 지나친 일반화   

* **가장 좋은 성능을 내는 k를 선택**
    * k의 값을 1부터 증가시켜가며 각 점들에 대해 knn으로 분류해보고 오류 계산 
    * 가장 오류가 적은 k값을 선택

### RNN  
RNN은 음성, 문자와 같이 순차적으로 진행하는 데이터 처리에 적합한 알고리즘 이다.
<center><img src="/img/signlanguage/rnn_loop.png"></center>
&nbsp;그림과 같이 hidden layer의 결과가 다시 hidden layer의 입력으로 들어가는 순환되는 구조를 가졌기 때문에 재귀 인공 신경망이라고 불린다.  
&nbsp;hidden layer의 결과가 다시 hidden layer의 입력으로 들어가는 특성 때문에 RNN은 현재 들어오는 입력 데이터와 전 단계에서 나온 결과를 동시에 고려하게 됨으로써 기억 능력이 있다고 할 수 있게 되고, sequence 또는 시계열 데이터를 분석하는데 굉장이 효과적인 네트워크이다.

## 구현 과정   

### 1. media pipe가 제공해 주는 손의 joint를 활용하여 벡터사이의 각도를 구한다.
<center><img src="/img/signlanguage/joint.png"></center>

1. 점과 점사이의 뺄셈 연산을 통해 0번에서 1번으로 가는 벡터, 1번에서 2번으로 가는 벡터 등등 많은 벡터를 만들어낸다.
```python
if result.multi_hand_landmarks:
        for hand_landmarks in result.multi_hand_landmarks: # 여러개의 손을 인식 할 수 있으니까, for문 반복
            joint = np.zeros((21,4)) # 손 관절 (joint) 넘파이 배열로 생성
            for j, lm in enumerate(hand_landmarks.landmark): # media pipe의 landmark를 반복하며 joint에 대입
                joint[j] = [lm.x,lm.y,lm.z,lm.visibility]

            v1 = joint[[0,1,2,3,0,5,6,7,0, 9,10,11, 0,13,14,15, 0,17,18,19],:3] # 벡터를 구하기 위해 생성하는 v1,v2 (v2에서 v2을 빼면 v백터가 된다)
            v2 = joint[[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20],:3]

            v = v2-v1
            v = v / np.linalg.norm(v,axis=1)[:,np.newaxis] # 정규화
```
2. 만들어진 벡터들 사이의 각도를 구한다  
```python
compareV1 = v[[0,1,2,4,5,6,8,9,10,12,13,14,16,17,18],:] #각 벡터의 각도를 비교하기 위해 생성하는 compare벡터
compareV2 = v[[1,2,3,5,6,7,9,10,11,13,14,15,17,18,19],:]

angle = np.arccos(np.einsum('nt,nt->n',compareV1,compareV2)) # compare벡터를 사용하여 각도를 구함
angle = np.degrees(angle)
```
3. 구해진 각도를 KNN알고리즘으로 어떤 제스쳐를 뜻하는지 학습시킨다. 
이때 구한 각도는 데이터셋을 활용하여 학습을 시키는데, 이 데이터셋은 직접 모을 수 있게 구현하였다.












 








