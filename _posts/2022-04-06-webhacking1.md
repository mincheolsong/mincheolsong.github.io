---
layout : single
title:  "WebHacking 1"
last_modified_at : 2022-04-07
categories: webhacking
---

## 준비 프로그램

### VirtualBox
>가상화 프로그램으로 시스템의 일부자원을 사용하여 새로운 운영체제와 각종 프로그램을 설치 할 수 있다.     
>이때, 원래 모체가 되는 시스템을 host 시스템이라고 하고 새로 운영체제를 설치한 환경을 guest환경이라고 한다.    guest환경을 사용하기 위해 virtualbox를 사용하는 것이다.

### 칼리리눅스
>모의해킹, 보안관련 프로그램들을 모아놓은 리눅스 운영체제

### XAMPP
>아피치웹서버 MYSQL PHP환경을 쉽게 구성할 수 있도록 도와주는 프로그램

### DVWA
>웹해킹을 실습하고 그 대응방안을 학습할 수 있도록 만들어진 웹 어플리케이션

## 웹 아키텍쳐

### HTTP
>웹을 구현하기 위한 네트워크 프로토콜    
**응답코드** 
1XX : 정보전달      
2XX : 성공      
3XX : 리다이렉션      
4XX : 클라이언트 쪽 에러      
5XX : 서버 에러      

### 세션 유지와 쿠키
>- 로그인을 유지시키기 위함
>- **쿠키** : 변수와 값의 쌍으로 구성된 집합
>- 처음 로그인하면 서버는 session id라는 것을 발급하여 쿠키 값으로 만듬    SET COOKIE를 웹브라우저로 전달.
>- SESSION ID는 잘 보호해주어야 한다.

### XSS
>SESSION ID를 빼내는 것을 주 목적으로 하는 공격기법

## HTTP프록시와 Burp Suit

### HTTP프록시
>- HTTP요청과 응답을 가로챌 수 있다
>- 메세지의 내용을 볼 수 있고, 수정해서 보낼 수 있다.

### Burp Suit
> 대표적인 http프록시 프로그램

* target menu
![Alt text](/img/target_menu.png)

    접속한 host들과 url을 tree형태로 보여준다.    **선명하게 표시**된 localhost는 우리가 직접 방문한것을 나타낸다.     
    그 외에 *흐리게 표시*된 부분은 burp suit가 http메시지를 분석해서 안에 포함된 링크들을 자동으로 보여주는 것이다.       

* Proxy->HTTP history
![Alt text](/img/proxy_httphistory.png)

    우리가 이전에 접속했던 localhost와 url들을 표시한다.
    선택을 하면 아래쪽에 요청과 응답 http가 자세히 표시된다. 가장 중요한 부분이 **Cookie**부분이다.

* Intruder
![Alt text](/img/intruder.png)  

    요청 메시지의 일부분을 지정하여 여러개의 데이터를 반복해서 전송하는 기능이다.

* Repeater
![Alt text](/img/repeater.png)   

    Intruder와 비슷하지만 수동으로 테스트 할 때 더 유용한 기능이다.
    Proxy-HTTP history에서 테스트하고 싶은 요청을 마우스 오른쪽 클릭->Send to intruder선택하면 Repeater 탭에 등록된다. 


### 브루트포스 공격
>id와 pw가 일치할 때 까지 무한정 계속해서 반복입력 하는 것이다(무식하게)

### 딕셔너리 공격
>pw파일(통계적으로 많이 사용하는 pw)을 사용하여 공격




