---
title: "트랜스포트(전송) 계층"
layout: post
date: '2021-04-29 22:19:51 +0900'
categories: study
tags: network
comments: true
---

- 목차
    - [1. 전송 계층이란?](#1-전송-계층이란)
    - [2. UDP](#2-udp)
    - [3. TCP](#3-tcp)
<br>
<br>
<br>

## 1. 전송 계층이란?
---
전송 계층(Transport layer)는 프로토콜 내에서 송신자와 수신자를 연결하는 통신 서비스를 제공한다.<br>
연결 지향의 **전송 제어 프로토콜(TCP)**와 비연결성인 **사용자 데이터 프로토콜(UDP)**가 존재.<br> 
응용 계층의 여러 프로토콜들은 사용쓰임에 따라 TCP 혹은 UDP로 되어있다.<br>

![ex_screenshot](/assets/img/protocol.PNG)<br>


## 2. UDP
---

![ex_screenshot](/assets/img/udp header.PNG)<br>

 - 비연결형 서비스로 데이터그램 방식을 제공.
 - 헤더 정보는 8바이트로 구성되면 4가지 정보를 가진다.(src / dest port, length, checkSum)
 - 일부 손실이 생겨도 손실된 데이터를 보장해주지 않음.

<br>
<br>
<br>

## 3. TCP
---
**출처 : http://www.ktword.co.kr/abbr_view.php?m_temp1=1889**<br>
![ex_screenshot](/assets/img/tcp header.PNG)<br>

 - 연결형 서비스로 세그먼트 방식을 제공
 - 헤더 정보는 20 ~ 60 바이트로 구성되면 신뢰성과 연결 지향성을 위해 다양한 필드가 존재.
 - 신뢰성있는 전송을 위해서 시퀀스 번호를 가지고 있음.
 - 데이터 수신을 확인하기 위한 확인응답번호를 가지고 있음.
 - 다양한 flag 정보로 동기화 및 확인, 종료, 초기화, 긴급을 표시할 수 있음.
 - 수신측 전송 트래픽을 고려하기 위해 전송 데이터 양을 결정하는 window 필드 존재.