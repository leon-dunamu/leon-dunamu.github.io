---
layout: post
title: "EC2 자동화 user data 작성하기 "
subtitle: "cloud-init으로 자동화 스크립트 실행"
author: "Seog"
header-style: text
tags: 
  - aws
---

## user data에 스크립트 작성하기

1. ec2 인스턴스를 중지 

> 인스턴스 클릭 > 작업 > 인스턴스 설정 > 사용자 데이터 보기/변경

2. 사용자 데이터 부분에 아래 스크립트 삽입

```text
Content-Type: multipart/mixed; boundary="//"
MIME-Version: 1.0

--//
Content-Type: text/cloud-config; charset="us-ascii"
MIME-Version: 1.0
Content-Transfer-Encoding: 7bit
Content-Disposition: attachment; filename="cloud-config.txt"

#cloud-config
cloud_final_modules:
- [scripts-user, always]

--//
Content-Type:
    text/x-shellscript; charset="us-ascii"
MIME-Version: 1.0
Content-Transfer-Encoding: 7bit
Content-Disposition: attachment; filename="userdata.txt"

#!/bin/bash

--//
```

위 텍스트의 하단 부분<`#!/bin/bash`부터 `--//`까지>에 원하는 스크립트를 작성한다.

```text
#cloud-config
cloud_final_modules:
- [scripts-user, always]
```

이 부분은 스크립트가 인스턴스 `재부팅마다 실행`되게 하는 구문이다.


3. 저장 후, 인스턴스 다시 시작시킨다.

