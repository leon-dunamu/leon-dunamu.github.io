---
layout: post
title: "database 색인"
subtitle: "frontend 면접 대비 컴퓨터 기초"
author: "Seog"
header-style: text
tags: 
  - Frontend
  - ComputerScience
---

![image](https://user-images.githubusercontent.com/49581472/107919819-42773280-6faf-11eb-98c6-6012f73be449.png)

## 검색이란

대량의 문서 집합에서 <span style="background-color:#fab1a0;">특정 키워드가 포함된 문서를 찾아</span>내는 것

- 조건 : 모두 찾아 내야하고 더 적합한 것이 먼저 나와야함
- 검색 성능 평가 요소

  - Recall

    모두 나와야하는 것 중에 검색 결과에 포함된 것의 비율 - 재현율

  - Precision

    검색 결과에 포함된 것 중에 제대로 나온 것의 비율 - 정확률

<span style="background-color:#ffeaa7;">실시간 검색의 문제점[속도가 느림]을 보완</span>하기 위해 역색인 구축 필요

<br/><br/>

# 색인

빠른 검색을 위하여 문서에서 <span style="background-color:#fdcb6e;">검색의 될 만한 것(Lexicon)들의 존재여부와 위치(positing)</span>를 미리 추출하여

빠른 탐색 자료구조를 구축하여 놓는 것

<br/>

# 역색인

어떠한 키워드가 주어졌을 때, 어떠한 문서에서 나타났는지 알려주는 자료구조

쉽게 말해서 색인이 문서들에서 키워드를 뽑아내는 과정이라면 역색인은 어떠한 키워드에대해 요청(찾아달라고)이 들어왔을때 그 뽑아낸 <span style="background-color:#74b9ff;">키워드들을 바탕으로</span> 그 키워드가 포함된 <span style="background-color:#74b9ff;">문서를 찾아내는 것</span>

- 장점

  단어가 나타나는 문서를 빨리 찾을 수 있음

- 단점

  마지막으로 인덱싱 이후 파일이 없어지거나 새로 생기거나 <span style="background-color:#a29bfe;">내용이 바뀌었다면 정확한 검색 불가</span>
