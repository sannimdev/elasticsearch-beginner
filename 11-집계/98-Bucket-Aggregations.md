# Bucket aggregations

-   Metro 계산보다 좀 더 복합적인 것
-   그렇지만, 매우 강력하고 멋진 기능이다.
-   필드에 대한 메트릭을 계산하는 것 대신에, bucket applications는 documents의 버킷을 만들어낸다.
    (sets of documents)
-   몇몇 총계는 싱글 버킷으로 산출해내고 이외의 것은 숫자가 고정되어 있다.
    일부는 여러 개의 버킷을 동적으로 만들어낸다

```json
GET /orders/_search
{
  "size": 0,
  "aggs": {
    "status_terms": {
      "terms": {
        "field": "status",
        "missing": "N/A",
        "min_doc_count": 0,
        "order": {
          "_term": "desc"
        }
      }
    }
  }
}

#! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.15/security-minimal-setup.html to enable security.
#! Deprecated aggregation order key [_term] used, replaced by [_key]
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
    "status_terms" : {
      "doc_count_error_upper_bound" : 0,
      "sum_other_doc_count" : 0,
      "buckets" : [
        {
          "key" : "processed",
          "doc_count" : 209
        },
        {
          "key" : "pending",
          "doc_count" : 199
        },
        {
          "key" : "confirmed",
          "doc_count" : 192
        },
        {
          "key" : "completed",
          "doc_count" : 204
        },
        {
          "key" : "cancelled",
          "doc_count" : 196
        },
        {
          "key" : "N/A",
          "doc_count" : 0
        }
      ]
    }
  }
}


```
