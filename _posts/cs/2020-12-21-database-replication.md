---
layout: post
title: "database 리플리케이션"
subtitle: "frontend 면접 대비 컴퓨터 기초"
author: "Seog"
header-style: text
tags: 
  - Frontend
  - ComputerScience
---

리플리케이션(Replication)은 복제를 뜻하며 2대 이상의 DBMS를 나눠서 데이터를 저장하는 방식이며, 사용하기 위한 최소 구성은 <span style="background-color:#dfe6e9;">Master / Slave 구성</span>을 하여야 됩니다.

**Master DBMS 역할 :**

웹서버로 부터 데이터 등록/수정/삭제 요청시 <span style="background-color:#74b9ff;">바이너리로그(Binarylog)를 생성하여 Slave 서버로 전달</span>하게 됩니다

(웹서버로 부터 요청한 데이터 <span style="background-color:#fdcb6e;">등록/수정/삭제 기능</span>을 하는 DBMS로 많이 사용됩니다)

**Slave DBMS 역할 :**

Master DBMS로 부터 <span style="background-color:#fab1a0;">전달받은 바이너리로그(Binarylog)를 데이터로 반영</span>하게 됩니다

(웹서버로 부터 요청을 통해 데이터를 <span style="background-color:#ffeaa7;">불러오는 DBMS</span>로 많이 사용됩니다)

## 사용 목적

1. 데이터 백업
2. Select 구문 분리로 사용자 부하 분산
