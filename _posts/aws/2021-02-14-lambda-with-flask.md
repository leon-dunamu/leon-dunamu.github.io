---
layout: post
title: "lambda에서 zappa를 사용하여 flask 설치하기"
description: "aws lambda에서 flask를 통해 프로젝트를 생성"
author: "Seog"
header-style: text
tags: 
  - aws
---

ec2는 프리티어가 1년 제한이기에, 월 사용량 한도 내에 무료인 `aws lambda`를 활용하여 api를 제작하기 위함

이는 aws 계정이 있다는 전제 하에 진행됨

<br/>

### 1. 프로젝트를 vscode로 오픈

```bash
➜  ~ (master) ✗ cd Documents
➜  Documents (master) ✗ mkdir awssample
➜  Documents (master) ✗ cd awssample
➜  awssample (master) ✗ code .
```

<br/>

### 2. aws key 설정

- credentials 파일 생성 및 키 삽입

  ```bash
  ➜  awssample (master) ✗ touch credentials
  ```

  credentials 설정

  ```jsx
  [default]
  aws_access_key_id = FKEFLSJEFL...
  aws_secret_access_key = xcw3f09fw09jw...
  ```

  credentials 파일 이동

  ```bash
  ➜  awssample (master) ✗ mv credentials ~/.aws/
  ```

  vi 편집기로 옮겨진 것 확인

  ```bash
  ➜  awssample (master) ✗ vi ~/.aws/credentials
  ```

<br/>

### 3. pipenv로 실행

- pipenv가 없으면 아래 커멘드로 설치 요망

  ```bash
  ➜  awssample (master) ✗ sudo pip install pipenv
  ```

- 사용할 패키지 pipenv로 다운 후 가상환경 오픈

  ```bash
  ➜  awssample (master) ✗ pipenv install flask
  ➜  awssample (master) ✗ pipenv shell
  ```

  `zsh: command not found: pipenv`가 발생한다면,

  ```bash
  ➜  awssample (master) ✗ python -m pipenv
  ```

<br/>

### 4. 예제 파일 생성

- app.py 생성

  ```bash
  (awssample) ➜  awssample (master) ✗ touch app.py
  ```

- app.py 작성

  ```python
  from flask import Flask, render_template

  app = Flask(__name__)


  @app.route("/")
  def index():
      return render_template('index.html')


  @app.route("/api")
  def api():
      return {"hello": "world"}
  ```

- 프로젝트에 html template 생성

  vscode에서 폴더와 index.html 생성

  ![image](https://user-images.githubusercontent.com/49581472/107871220-fd85ca00-6ee2-11eb-873b-c8eacb6bb2af.png)

  index.html은 다음과 같음

  ```html
  <!DOCTYPE html>
  <html lang="en">
    <head>
      <meta charset="UTF-8" />
      <meta http-equiv="X-UA-Compatible" content="IE=edge" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <title>Document</title>
    </head>
    <body>
      <h1>Hello</h1>
    </body>
  </html>
  ```

<br/>

### 5. flask 실행 및 확인

```bash
(awssample) ➜  awssample (master) ✗ flask run
```

그러면 `http://127.0.0.1:5000/`로 접근 가능

잘 뜨는 것을 확인하면 `ctrl + C`로 서버 종료

<br/>

### 6. zappa 설치와 설정

```bash
(awssample) ➜  awssample (master) ✗ pipenv install zappa
```

설치 완료 후

```bash
(awssample) ➜  awssample (master) ✗ zappa init
```

이후 물어보는 사항들에 모두 `enter`입력

생성된 _*zappa_settings.json*_ 수정

```json
{
  "dev": {
    "app_function": "app.app",
    "profile_name": "default",
    "project_name": "awssample",
    "runtime": "python3.8",
    "s3_bucket": "zappa-bfkygpmzw",
    "aws_region": "ap-northeast-2", // 지역
    "log_level": "WARNING",
    "memory_size": 128,
    "timeout_seconds": 30
  }
}
```

<br/>

### 7. zappa 배포

```bash
(awssample) ➜  awssample (master) ✗ zappa deploy dev
```

![image](https://user-images.githubusercontent.com/49581472/107871383-543fd380-6ee4-11eb-982e-78b036061b21.png)

배포 완료된 모습으로, `Deployment complete!`의 URL을 통해 접근 가능

dev은 배포 키워드이며, 이를 변경시 **zappa_settings.json**의 `"dev"`부분도 수정해야 함

   <br/>
   코드를 수정하고 업데이트 방법은

```bash
(awssample) ➜  awssample (master) ✗ zappa update
```
