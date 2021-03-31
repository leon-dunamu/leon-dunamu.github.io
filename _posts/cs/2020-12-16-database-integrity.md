---
layout: post
title: "database 무결성"
subtitle: "frontend 면접 대비 컴퓨터 기초 학습"
author: "Seog"
header-style: text
tags: 
  - Frontend
  - ComputerScience
---

데이터의 무결성은 데이터의 <span style="background-color:rgb(231, 194, 255);">정확성, 일관성, 유효성이 유지</span>되는 것을 말한다. 데이터의 무결성을 유지하는 것은 데이터베이스 관리시스템 (DBMS)의 중요한 기능이며, 주로 데이터에 적용되는 <span style="background-color:rgb();">연산에 제한을 두어 데이터의 무결성을 유지한다. 데이터베이스에서 말하는 무결성에는 다음과 같은 4가지 종류가 있다.

<br/>

### 1. 개체 무결성

> 모든 테이블이 기본 <span style="background-color:rgb(255, 215, 122);">키 (primary key)</span>로 선택된 필드 (column)를 가져야 한다. 기본 키로 선택된 필드는 <span style="background-color:rgb(117, 205, 255);">고유한 값</span>을 가져야 하며, 빈 값은 허용하지 않는다.

### 2. 참조 무결성

> 관계형 데이터베이스 모델에서 <span style="background-color:rgb(255, 171, 179);">참조 무결성은 참조 관계에 있는 두 테이블의 데이터가 항상 일관된 값을 갖도록 유지되는 것</span>을 말한다. 아래의 [그림 1]은 관계형 데이터베이스 모델에서 참조 무결성이 깨지는 경우를 나타낸다. 이 예시에서는 department 테이블을 참조하는 student 테이블을 보여주고 있다. 이러한 참조 관계에서 만약 department 테이블에서 id 값이 310인 <span style="background-color:rgb(117, 205, 255);">레코드가 삭제되면</span> student 테이블의 3번째 레코드는 <span style="background-color:rgb(206, 255, 196);">더 이상 존재하지 않는 데이터를 참조</span>하게 된다.

![image](https://user-images.githubusercontent.com/77267603/107911624-4b143c80-6fa0-11eb-89f6-f4adc12d9e02.png)

### 3. 도메인 무결성

> 도메인 무결성은 테이블에 존재하는 <span style="background-color:rgb(255, 215, 122);">필드의 무결성을 보장</span>하기 위한 것으로 필드의 타입, NULL값의 허용 등에 대한 사항을 정의하고, 올바른 데이터의 입력 되었는지를 확인하는 것이다. 예를 들어, 주민등록번호 필드에 알파벳이 입력되는 경우는 도메인 무결성이 깨지는 경우라고 볼 수 있다. <span style="background-color:rgb(206, 255, 196);">DBMS의 기본값 설정, NOT NULL 옵션</span> 등의 제약 사항으로 도메인 무결성을 보장할 수 있다.

### 4. 무결성 규칙

> 데이터베이스에서 무결성 규칙은 데이터의 <span style="background-color:rgb(255, 171, 179);">무결성을 지키기 위한 모든 제약</span> 사항들을 말한다. 비즈니스 규칙 (business rule)은 데이터베이스를 이용하는 각각의 유저에 따라 서로 다르게 적용되지만, 무결성 규칙은 데이터베이스 전체에 공통적으로 적용되는 규칙이다.

<br/>
<br/>

<span style="font-size:13px;">출처 : [티스토리 블로그](https://untitledtblog.tistory.com/123)</span>
