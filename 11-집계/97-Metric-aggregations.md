# Metric Aggregations

아마도 친숙하다면 관계형 데이터베이스로부터 이것들을 대부분 알아볼 수 있다.

## Single value numeric metric aggregations

-   간단히 말해 단 하나의 값을 산출하는 것이다
-   평균 숫자의 합 산출하기 등

### 합, 평균, 최솟값, 최댓값 구하기

```json
GET /orders/_search
{
  "size": 0,
  "aggs": {
    "total_sales": {
      "sum": {
        "field": "total_amount"
      }
    },
    "avg_sale": {
      "avg":{
        "field": "total_amount"
      }
    },
    "min_sale": {
      "min":{
        "field": "total_amount"
      }
    },
    "max_sale": {
      "max":{
        "field": "total_amount"
      }
    }
  }
}

#! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.15/security-minimal-setup.html to enable security.
{
  "took" : 4,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 1000,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [ ]
  },
  "aggregations" : {
    "max_sale" : {
      "value" : 281.77
    },
    "avg_sale" : {
      "value" : 109.20961
    },
    "min_sale" : {
      "value" : 10.27
    },
    "total_sales" : {
      "value" : 109209.61
    }
  }
}
```

### 카디널리티 개수 구하기

```json
GET /orders/_search
{
  "size": 0,
  "aggs": {
    "total_salesmen": {
      "cardinality": {
        "field": "salesman.id"
      }
    }
  }
}

#! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.15/security-minimal-setup.html to enable security.
{
  "took" : 13,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 1000,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [ ]
  },
  "aggregations" : {
    "total_salesmen" : {
      "value" : 100
    }
  }
}
```

```json
GET /orders/_search
{
  "size": 0,
  "aggs": {
    "value_count": {
      "value_count": {
        "field": "total_amount"
      }
    }
  }
}
#! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.15/security-minimal-setup.html to enable security.
{
  "took" : 1,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 1000,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [ ]
  },
  "aggregations" : {
    "value_count" : {
      "value" : 1000
    }
  }
}

```

## Multi value numeric metric aggregations

-   Multi value aggregations는 말 그대로 여러 값을 산출하는 것이다.
-   이 강의에서 보여줄 것

```json

GET /orders/_search
{
  "size": 0,
  "aggs": {
    "amount_stats": {
      "stats": {
        "field": "total_amount"
      }
    }
  }
}

#! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.15/security-minimal-setup.html to enable security.
{
  "took" : 3,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 1000,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [ ]
  },
  "aggregations" : {
    "amount_stats" : {
      "count" : 1000,
      "min" : 10.27,
      "max" : 281.77,
      "avg" : 109.20961,
      "sum" : 109209.61
    }
  }
}

```
