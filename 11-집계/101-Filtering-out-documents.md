# Filtering out documents

-   집계를 사용하는 문서는 집계의 컨텍스트에 따라 다르게 정의되어 있다.
-   그래서 때때로 지정된 집계에 대한 문서를 필터링해야 할 때가 있다.

```json
GET /orders/_search
{
  "size": 0,
  "aggs": {
    "low_value": {
      "filter": {
        "range": {
          "total_amount": {
            "lt": 50
          }
        }
      },
      "aggs": {
        "avg_amount": {
          "avg": {
            "field": "total_amount"
          }
        }
      }
    }
  }
}

#! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.15/security-minimal-setup.html to enable security.
{
  "took" : 15,
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
    "low_value" : {
      "doc_count" : 164,
      "avg_amount" : {
        "value" : 32.59371951219512
      }
    }
  }
}

```
