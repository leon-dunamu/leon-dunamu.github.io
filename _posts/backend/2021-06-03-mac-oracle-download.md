---
layout: post
title: "맥에 오라클 설치하기"
subtitle: "docker를 사용하여 맥에 오라클 설치!"
author: "Seog"
header-style: text
tags: 
  - Backend
  - database
---

학교 프로젝트로 oracle을 사용하래서 다운하려했더니 docker를 쓰랜다..

### 1. docker 설치

[도커 사이트](https://hub.docker.com/editions/community/docker-ce-desktop-mac)가서 다운로드.
대부분 intel칩이겠죠...ㅎㅅㅎ

설치는 여느 다른 앱들과 다르지 않습니다.

### 2. oracle 다운로드

도커를 다운로드 후 실행하였으면 터미널을 킵니다.

* 꼭 실행하셔야 합니다

```bash
docker search oracle-xe-11g

docker pull jaspeen/oracle-xe-11g
```

이렇게 다운 받았으면 아래와 같이 확인합니다.

```bash
docker images
```

그렇다면 아래와 비슷한 결과를 받을 것입니다.

> REPOSITORY              TAG       IMAGE ID       CREATED       SIZE <br/>
> jaspeen/oracle-xe-11g   latest    52fbd1fe2d7a   5 years ago   792MB

### 3. 시작

```bash
docker run --name <project-name> -d -p 8080:8080 -p 1521:1521 jaspeen/oracle-xe-11g
```

`<project-name>`은 원하는 이름 하시면 됩니다.
저는 `database-oracle-project`로 진행하였는데요, 저는 다음과 같이 입력하였습니다.

```bash
docker run --name database-oracle-project -d -p 8080:8080 -p 1521:1521 jaspeen/oracle-xe-11g
```

이후 실행시켜 줍니다.
```bash
docker exec -it database-oracle-project sqlplus
```

그러면 아래와 같이 `SQL`을 작성할 수 있게 나올텐데요,
`select * from tab`을 사용하여 테스트하여 봅니다.

```bash
SQL> select * from tab
```

그렇다면 엄청 긴 내용들이 나옵니다!
이상으로 설치는 끝입니다.

이후 저는 `DBeaver`를 통해서 DB관리를 진행했습니다.