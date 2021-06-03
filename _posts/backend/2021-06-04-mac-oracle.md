---
layout: post
title: "64-bit Oracle Client library cannot be loaded in mac"
subtitle: "mac에서 oracle사용시 발생하는 에러 해결"
author: "Seog"
header-style: text
tags: 
  - Backend
  - database
---

`mac`에서 docker를 사용하여 oracle을 다운 받는데에는 성공하였지만,
막상 `python + flask`에서 실행해보니 에러를 맞이하였다. 

## 나의 환경

- OS : MACOS (Intel)
- oracle : oracle-xe-11g (jaspeen/oracle-xe-11g)

## 해결법

우선 출처는 [이곳](https://cx-oracle.readthedocs.io/en/latest/user_guide/installation.html#installing-cx-oracle-on-macos)이다.

설명이 잘 되어있으니 보고 따라해도 무방하다!
하지만 영어가 무서울 수 있으므로, 그냥 콕 찝어 정리하겠다.

### 1. 경로 이동

```bash
cd $HOME/Downloads
```

### 2. library dmg 다운로드 및 적용

```bash
curl -O https://download.oracle.com/otn_software/mac/instantclient/198000/instantclient-basic-macos.x64-19.8.0.0.0dbru.dmg

hdiutil mount instantclient-basic-macos.x64-19.8.0.0.0dbru.dmg

/Volumes/instantclient-basic-macos.x64-19.8.0.0.0dbru/install_ic.sh

hdiutil unmount /Volumes/instantclient-basic-macos.x64-19.8.0.0.0dbru
```

이로써 끝.