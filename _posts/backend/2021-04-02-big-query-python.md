---
layout: post
title: "BigQuery with Python"
subtitle: "파이썬에서 Big Query 사용하기"
author: "Seog"
header-style: text
tags: 
  - Backend
---

## 0. 패키지 설치

```bash
$ pip install --upgrade google-cloud-bigquery
```

## 1. 사용법

### 1-1. 이름 지정 매개변수

```python
from google.cloud import bigquery

# Construct a BigQuery client object.
client = bigquery.Client()

query = """
    SELECT word, word_count
    FROM `bigquery-public-data.samples.shakespeare`
    WHERE corpus = @corpus
    AND word_count >= @min_word_count
    ORDER BY word_count DESC;
"""
job_config = bigquery.QueryJobConfig(
    query_parameters=[
        bigquery.ScalarQueryParameter("corpus", "STRING", "romeoandjuliet"),
        bigquery.ScalarQueryParameter("min_word_count", "INT64", 250),
    ]
)
query_job = client.query(query, job_config=job_config)  # Make an API request.

for row in query_job:
    print("{}: \t{}".format(row.word, row.word_count))
```

### 1-2. 위치 매개변수 사용법

```python
from google.cloud import bigquery

# Construct a BigQuery client object.
client = bigquery.Client()

query = """
    SELECT word, word_count
    FROM `bigquery-public-data.samples.shakespeare`
    WHERE corpus = ?
    AND word_count >= ?
    ORDER BY word_count DESC;
"""
# Set the name to None to use positional parameters.
# Note that you cannot mix named and positional parameters.
job_config = bigquery.QueryJobConfig(
    query_parameters=[
        bigquery.ScalarQueryParameter(None, "STRING", "romeoandjuliet"),
        bigquery.ScalarQueryParameter(None, "INT64", 250),
    ]
)
query_job = client.query(query, job_config=job_config)  # Make an API request.

for row in query_job:
    print("{}: \t{}".format(row.word, row.word_count))
```