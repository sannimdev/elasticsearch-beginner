# Histograms

-   주문 수량에 대해 간격을 25로(적분) 지정할 경우 어떻게 해야 할까?
    일부 ORU에서 누락되는 것을 받아들일 수 없는 한 최소 금액과 최대 금액을 알아야 한다. (그러나 이것은 현실적으로 불가능하다)
-   그래서 많은 범위를 명시적으로 일일이 추가해주는 수밖에 없을 텐데 이를 Elasticsearch로 보내는 애플리케이션 내에서 자동화하는 방식으로 해결해야 한다.
-   그런데 다행히도 히스토그램이라는 것을 사용하면 좀 더 쉽게게 해결할 수 있다.
    지정된 간격을 기준으로 동적으로 구성할 수 있다.

히스토그램 집계를 사용하여 주문량의 분폴르 알고싶을 경우 25의 적분을 지정할 수 있다.
Elasticsearch는 동적으로 필드의 최솟값과 최댓값 사이의 간격의 각 단계에 대해서 버킷을 만든다.

예를들어

1. 간격이 25로 설정된 상태에서 ES는 5개의 버킷을 만든다
2. 0, 25, 50, 75, 100의 키가 있다
3. 그리고 doucment가 버킷에 채워진다

```json
GET /orders/_search
{
  "size": 0,
  "aggs": {
    "amount_distribution": {
      "histogram": {
        "field": "total_amount",
        "interval": 25,
        "min_doc_count": 1
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
    "amount_distribution" : {
      "buckets" : [
        {
          "key" : 0.0,
          "doc_count" : 42
        },
        {
          "key" : 25.0,
          "doc_count" : 122
        },
        {
          "key" : 50.0,
          "doc_count" : 153
        },
        {
          "key" : 75.0,
          "doc_count" : 194
        },
        {
          "key" : 100.0,
          "doc_count" : 124
        },
        {
          "key" : 125.0,
          "doc_count" : 125
        },
        {
          "key" : 150.0,
          "doc_count" : 89
        },
        {
          "key" : 175.0,
          "doc_count" : 71
        },
        {
          "key" : 200.0,
          "doc_count" : 42
        },
        {
          "key" : 225.0,
          "doc_count" : 33
        },
        {
          "key" : 250.0,
          "doc_count" : 4
        },
        {
          "key" : 275.0,
          "doc_count" : 1
        }
      ]
    }
  }
}
```
