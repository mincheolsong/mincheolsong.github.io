---
layout : single
title:  "sign language"
last_modified_at : 2022-05-31
categories: signLanguage
---

## 제작동기
**[청각장애인을 위한 인공지능]**    
&nbsp;인공지능 기술이 급속도로 발전하고 있는 현재, 이를 활용하여 일상생활에 불편을 느끼는 장애인 분들을 위한 제품을 만들어 보고 싶었다. 그 중 청각장애인을 위한 수화인식 프로그램을 만들면 좋겠다고 생각하게 되었다.    
&nbsp;MediaPipe 라이브러리를 사용하여 손의 joint를 검출한다. 그 출력 결과를 단일 동작으로 분류 할 수 있는 KNN(K-Nearest Neighbor)과 연속 동작을 분류 할 수 있는 RNN(Recurrent Neural Network)의 입력으로 학습하여 지화를 실시간으로 인식하고 출력한다.

## 이론적 배경 
**MediaPipe**  
&nbsp;MediaPipe란 구글에서 제공하는 AI 프레임워크로써, 비디오 형식 데이터를 이용한 다양한 비전 AI기능을 파이프라인 형태로 손쉽게 사용할 수 있도록 제공된다. AI 모델 개발 및 수많은 데이터셋을 이용한 학습도 마친 사태로 제공되므로 라이브러리를 불러 사용하듯이 간편하게 호출하여 사용하기만 하면 되는 형태로, 비전 AI 기능을 개발 할 수 있다.







