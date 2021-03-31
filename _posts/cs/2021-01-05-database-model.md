---
layout: post
title: "database EK FK ER 모델"
subtitle: "frontend 면접 대비 컴퓨터 기초"
author: "Seog"
header-style: text
tags: 
  - Frontend
  - ComputerScience
---

## 1. Primary Key

테이블에서 각 <span style="background-color:#a29bfe;">행을 유일</span>하게 구분하는 Column-Key

<br/>

## 2. Foreign Key

하나의 테이블에 있는 Column으로는 그 의미를 표현할 수 없는 경우, <span style="background-color:#fab1a0;">다른 테이블의 Primary-Key</span> Column의 값을 <span style="background-color:#ffeaa7;">반드시 참조</span>해야하는 Key

<br/>

## 3. Entity-Relation Model

![image](https://user-images.githubusercontent.com/49581472/107920265-042e4300-6fb0-11eb-9a2d-36385eb82b96.png)

데이터베이스 설계시 사용되는 모델 중 하나

요구사항으로부터 얻어낸 정보들을 개체(Entity), 애트리뷰터(Attribute), 관계성(Relation)으로 기술하는 데이터 모델

- **개체( Entity )**
  - 개체란 <span style="background-color:#ffeaa7;">단독으로 존재하는 객체</span>를 의미하며, 동일한 객체는 존재하지 않습니다.
    - 예를 들어, 학생 정보가 학번, 이름, 학년이 있을 때, 3개의 정보가 모두 같은 학생이 오직 한 명이면 이를 개체라고 합니다.
    - 즉, 학생 한명이 개체가 되는 것입니다.
  - 이 <span style="background-color:#fdcb6e;">개체들의 집합</span>을 **Entity Type**이라고 합니다. 여기서는 Student, Course가 되겠네요.
  - ER 다이어그램에서 Entity Type은 네모로 표현합니다.
- **애트리뷰트, 속성( Attribute )**
  - <span style="background-color:#dfe6e9;">개체가 갖는 속성</span>을 의미합니다.
    - 예를 들어, Student에서 학번, 이름, 학년 같은 정보를 속성이라 합니다.
  - ER 다이어그램에서 Attribute는 원으로 표현합니다.
- **관계 ( Relation )**
  - <span style="background-color:#a29bfe;">Entity Type간의 관계</span>를 의미합니다.
    - 예를 들어, 수강을 뜻하는 Takes는 학생과 과목간의 "수강"이라는 관계를 갖습니다.
    - 이 때 Takes를 **Relation Type**이라 하며, Relation Type 역시 속성을 가질 수 있습니다.
  - ER 다이어그램에서 Relation은 마름모로 표현합니다.
