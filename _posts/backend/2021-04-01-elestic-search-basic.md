---
layout: post
title: "Elastic Search 기초"
subtitle: "Elastic Search 개념 학습하기"
author: "Seog"
header-style: text
tags: 
  - Backend
---


# Elastic Search 기본 개념

Apache Lucene 기반의 Java 오픈소스 <span style="background-color:#eaeaea; padding: 2px 4px; border-radius:4px;">분산 검색 엔진</span>

Elasticsearch를 통해 루씬 라이브러리를 단독으로 사용할 수 있게 되었으며, 방대한 양의 데이터를 신속하게, 거의 실시간( NRT, Near Real Time )으로 저장, 검색, 분석할 수 있음

Elasticsearch는 검색을 위해 단독으로 사용되기도 하며, ELK( Elasticsearch / Logstatsh / Kibana )스택으로 사용됨


### ELK 스택이란?

- Elasticsearch

    > Logstash로부터 받은 데이터를 검색 및 집계를 하여 필요한 관심 있는 정보를 획득

- Logstash

    > 다양한 소스( DB, csv파일 등 )의 로그 또는 트랜잭션 데이터를 수집, 집계, 파싱하여 Elasticsearch로 전달

- Kibana

    > Elasticsearch의 빠른 검색을 통해 데이터를 시각화 및 모니터링

## 1. Elastic Search vs RDB

<img src="https://user-images.githubusercontent.com/77267603/113251065-9ab68980-92fc-11eb-8700-ee07e8d04b4e.png" width="100%" alt="예제 그림"/>

<div style="text-align:right;">출처: <a href="https://www.slideshare.net/deview/2d1elasticsearch">https://www.slideshare.net/deview/2d1elasticsearch</a></div>


## 2. Elastic Search 특징

* Scale out

> 샤드를 통해 규모가 수평적으로 늘어날 수 있음

* 고가용성

> Replica를 통해 데이터의 안정성을 보장

* Schema Free

> Json 문서를 통해 데이터 검색을 수행하므로 스키마 개념이 없음

* Restful

> 데이터 CURD 작업은 HTTP Restful API를 통해 수행하며, 각각 다음과 같이 대응합니다. 
> |Data CRUD|Elastic Search RESTful|
> |:---:|:---:|
> |SELECT|GET|
> |INSERT|PUT|
> |UPDATE|POST|
> |DELETE|DELETE|

