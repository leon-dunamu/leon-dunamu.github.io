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

## 혹시 flask를 사용하시나요 ..?

그렇다면 `pip3 install cx-Oracle`을 하셨겠군요.

이번 포스트의 파일들을 잘 다운 받았으면 아래 또한 적용해줘야합니다.
다만 한번 적용해주면 됩니다!
(매 connection/close마다 적용할 필요 없음)

```python
import cx_Oracle

cx_Oracle.init_oracle_client(lib_dir="/Users/{user_name}/Downloads/instantclient_19_8")
```

`{user_name}`에는 자신의 PC 네임이 들어가야합니다.

모르겠다면 터미널에서 `pwd`를 이용하여 확인하십숑