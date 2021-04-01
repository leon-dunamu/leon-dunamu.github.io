---
layout: post
title: "EC2 git pull 자동화"
subtitle: "git hooks로 pull 자동화"
author: "Seog"
header-style: text
tags: 
  - aws
---

## git hooks 변경하기

### 1-1. 우선 프로젝트 root로 이동 후

```bash
cd .git
cd hooks
mv post-update.sample post-update    # .sample 지우기
vi post-update
```

### 1-2. vi로 post-update 수정

```bash
cd /data/<project_name>
unset GIT_DIR
git pull origin master
```

이후 파일 수정 후 push하면 자동으로 pull됨

<span style="background-color:#ffe268; padding: 2px 4px; border-radius: 3px;"> 하지만 위의 방법은 ssh 접근을 통제하지 않았기에 보안상 취약 !</span>


### 2-1. 마찬가지로 우선 프로젝트 root로 이동 후

```bash
cd .git
cd hooks
mv post-receive.sample post-receive   # .sample 지우기
vi post-receive
```

### 2-2. vi로 수정

```bash
# post-receive 작성

#!/bin/bash
while read oldrev newrev ref
do
  if [[ $ref = ~.*/master$ ]];
  then
    echo "success"
    git --work-tree=/data/<project_name> --git-dir=/data/<project_name>.git checkout -f
  else
    echo "fail"
  fi
done
```

이후 파일 수정 후 push하면 자동으로 pull됨