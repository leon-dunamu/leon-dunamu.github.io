---
layout: post
title: "database 다중화"
subtitle: "frontend 면접 대비 컴퓨터 기초"
author: "Seog"
header-style: text
tags: 
  - Frontend
  - ComputerScience
---

## 기본적인 다중화

DB 서버만을 다중화하고 저장소는 하나만 두는 구성. 이 경우 데이터가 보존되는 저장소가 1개라서

정합성을 신경 쓸 필요가 없다. (정합성 = 데이터 무결성(data integrity))

<br/>

## Active-Active Clustering

DB 서버를 여러 개 구성하는 데, 각 서버를 Active 상태로 둠

![image](https://user-images.githubusercontent.com/49581472/107919074-1ad39a80-6fae-11eb-984b-6534d609ab7e.png)

- 장점

  서버 하나가 죽더라도 다른 서버가 역할을 바로 수행하므로 서비스 중단이 없음

  CPU와 메모리 이용률을 올릴 수 있음

- 단점

  저장소 하나를 공유하게 되면 병목현상이 발생할 수 있음

  서버를 여러대 한꺼번에 운영하므로 비용이 더 발생

<br/>

## Active-Standby Clustering

서버를 하나만 운영하고 나머지 서버는 Standby 상태로 둠

운영하고 있는 서버가 다운되었을 시에 Standby 상태의 서버를 Active상태로 전환

![image](https://user-images.githubusercontent.com/49581472/107919169-38086900-6fae-11eb-9d9a-480082e2d791.png)

- 장점

  Active-Active 클러스터링에 비해 적은 비용

- 단점

  서버가 다운되었을 때 Standby 상태의 서버를 Active상태로 전환 시 시간이 듬

<br/><br/>

## **Active-Standby 구성에서 장애가 일어났을 때 Standby DB 서버는 어떻게 Active DB 서버에 장애가 일어난 것을 알 수 있을까?**

**A.** Standby DB 서버는 일정 간격으로 Active DB에 이상이 없는지를 조사하기 위한 통신을 하고 있다.

이 통신을 '**Heartbeat**'라고 한다. Active DB에 장애가 발생하면 이 신호가 끊기기 때문에

Standby 측은 Active가 '죽었다'는 것을 알 수 있다.
