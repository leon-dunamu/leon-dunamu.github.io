---
layout: post
title: "database 파티셔닝, 샤딩"
subtitle: "frontend 면접 대비 컴퓨터 기초"
author: "Seog"
header-style: text
tags: 
  - Frontend
  - ComputerScience
---

소프트웨어적으로 데이터베이스를 분산 처리하여 성능이 저하되는 것을 방지하고 관리를 보다 수월하게 할 수 있음

큰 table이나 index를, 관리하기 쉬운 partition이라는 <span style="background-color:#74b9ff;">작은 단위로 물리적으로 분할</span>하는 것을 의미

<span style="background-color:#ffeaa7;">샤딩은 수평적 파티셔닝</span>

# 목적

1. 성능(Performance)

   특정 DML과 Query의 성능을 향상시킨다.

   주로 대용량 Data WRITE 환경에서 효율적이다.

   특히, Full Scan에서 데이터 Access의 범위를 줄여 성능 향상을 가져온다.

   많은 INSERT가 있는 OLTP 시스템에서 INSERT 작업을 작은 단위인 partition들로 분산시켜 경합을 줄인다.

2. 가용성(Availability)

   물리적인 파티셔닝으로 인해 전체 데이터의 훼손 가능성이 줄어들고 데이터 가용성이 향상된다.

   각 <span style="background-color:#fab1a0;">분할 영역(partition별로)을 독립적으로 백업하고 복구</span>할 수 있다.

   table의 partition 단위로 Disk I/O을 분산하여 경합을 줄이기 때문에 UPDATE 성능을 향상시킨다.

3. 관리용이성(Manageability)

   큰 table들을 제거하여 관리를 쉽게 해준다.

# 장점

- 관리적 측면

  partition 단위 백업, 추가, 삭제, 변경

  전체 데이터를 손실할 가능성이 줄어들어 데이터 가용성이 향상된다.

  partition별로 백업 및 복구가 가능하다.

  partition 단위로 I/O 분산이 가능하여 UPDATE 성능을 향상시킨다.

- 성능적 측면 : partition 단위 조회 및 DML수행

  데이터 전체 검색 시 필요한 부분만 탐색해 성능이 증가한다.

  즉, Full Scan에서 데이터 Access의 범위를 줄여 성능 향상을 가져온다.

  필요한 데이터만 빠르게 조회할 수 있기 때문에 쿼리 자체가 가볍다.

# 단점

table간 <span style="background-color:#00b894;">JOIN에 대한 비용이 증가</span>한다.

table과 index를 별도로 파티셔닝할 수 없다.

table과 index를 같이 파티셔닝해야 한다.
