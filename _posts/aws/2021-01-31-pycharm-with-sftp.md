---
layout: post
title: "EC2에 pycharm으로 sftp 원격 연결하기"
subtitle: "로컬에서 ec2 인스턴스 코드 수정 반영하기"
author: "Seog"
header-style: text
tags: 
  - aws
---

## Pycharm IDE에서 "Run on remote server" 기능을 사용하기 위함.

로컬에서 코드를 작성하고 원격 서버에서 해당코드를 실행하는 기능

<br/>

## Remote Interpreter 설정

로컬에서 작성한 코드를 서버에서 실행하기 위해 서버에 설치된 파이썬 Interpreter를 지정해야 함.

### 1. Pycharm에서 연결할 프로젝트로 이동

### 2. Project Interpreter 설정창 열기

> Menu -> File -> Settings -> Project -> Project Interpreter

### 3. Remote Interpreter 연결하기

> 톱날버튼 클릭 후 -> Add Remote -> SSH Interpreter

### 4. Remote Interpreter를 연결하기 위해 우선 원격 서버를 추가

> host에 public ip와 username 작성 후 Next

### 5. 접속을 위한 Key 설정

> Key pair 선택 후 pem key의 위치로 첨부 후 Next

### 6. 서버에 적용할 프로젝트 선택

> Running code on the remote server / Sync folder에서 폴더모양 아이콘 클릭 후 경로 설정

### 7. 현재 Pycharm에서 파일을 수정하거나 삭제하였을 때 서버도 자동으로 동기화 되는 것 적용

> Authmatically upload project files to the server 체크 후 Next
