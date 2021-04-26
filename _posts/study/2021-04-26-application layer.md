---
title: "애플리케이션 계층"
layout: post
date: '2021-04-26 22:31:51 +0900'
categories: study
tags: githubpage
comments: true

---

# 개요

- 목차
    - [1. 애플리케이션 계층이란?](#1-애플리케이션-계층이란)
    - [2. 소켓](#2-소켓)
    - [3. HTTP](#3-http)
 
<br>
<br>
<br>

## 1. 애플리케이션 계층이란?
---
프로세스 간 통신 접속을 위해 설계되어 여러 하위 통신 프로토콜 개체에 대하여<br>
사용자 관점의 사용자 인터페이스를 제공

1. 클라이언트 - 서버 구조<br>
서버는 클라이언트 요청을 받을 때까지 대기하고 <br>
클라이언트는 서버에 원하는 요청을 보내 서비스를 제공받는 아키텍쳐<br>
서버는 영구적인 IP 주소를 사용하지만, 클라이언트는 동적 IP를 사용할 수 있다.<br> 

2. P2P 구조<br>
같은 망을 구성하는 노드들이 서로 클라이언트, 서버 역할을 동시에 하는 아키텍쳐

<br>
<br>
<br>

## 2. 소켓
---
프로세스 간에 통신이 가능하도록 연결해주는 연결부 역활.<br>
<br>
통신을 하기 위해서 서버와 클라이언트 모두 소켓이 생성되어 있어야함.<br>
<br>
각 프로세스의 목적지는 IP Address와 Port 번호의 조합으로 구성된다.<br>
<br>
Ex) IP : 127.0.0.1 / Port :8080<br>

<br>
<br>
<br>

## 3. HTTP
---
**HyperText Transfer Protocol**

애플리케이션 계층 프로토콜, 클라이언트와 서버 간의 Request/Response 프로토콜
주로<br>
**TCP를 전송 프로토콜**로 사용하며 **포트는 80번**을 사용한다.

1. non-persistent HTTP<br>
하나의 TCP를 통해 Request/Response 작업 후 닫히는 HTTP<br>
Hypertext에 여러 하이퍼 링크가 존재하면 여러 번의 TCP 연결이 발생함. (RTT ↑)<br> 

2. persistent HTTP<br>
하나의 TCP를 통해 여러 Request/Response 작업을 처리할 수 있는 HTTP (RTT ↓)<br>

※ RTT (Round Trip Time) : 클라이언트에서 Request 후 Response를 받기까지 걸리는 시간 


