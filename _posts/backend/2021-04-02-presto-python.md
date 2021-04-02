---
layout: post
title: "Presto with Python"
subtitle: "파이썬에서 프레스토로 query 사용하기"
author: "Seog"
header-style: text
tags: 
  - Backend
---

파이썬에서 프레스토로 query 사용하기

## 0. 패키지 설치

```bash
$ pip install pyhive
```

Presto 혹은 Hive에 접속하고 Query를 실행해서 결과를 pandas DataFrame으로 가져오는 사용자 정의함수를 정의

## 1. 실행 코드

```python
def presto_query(query):

     from pyhive import presto
     import pandas as pd

     cursor = presto.connect('12.34.567.890').cursor()
    
     # execute a query and get a result as a DataFrame

     cursor.execute(query)
     col_names = [ desc[0] for desc in cursor.description ]
     result = pd.DataFrame(cursor.fetchall(), columns=col_names)

     cursor.close()
 
     return result
```

* query 예시참고

```python

query = """
    WITH
        t1 AS (SELECT a, MAX(b) AS b FROM x GROUP BY a),
        t2 AS (SELECT a, AVG(d) AS d FROM y GROUP BY a)
    SELECT t1.*, t2.* FROM t1 JOIN t2 ON t1.a = t2.a;
    """

result = presto_query(query)
```


