# Defining bucket rules with filters

-   분리를 해제하면 버킷의 document가 들어갈 문서의 규칙을 정의할 수 잇다
-   이 작업은 버킷 이름과 document가 일치해야 하는 클래스를 재ㅣ정하여 수행된다.

-   두 개의 버킷을 가지는 작업
    -   파스타
    -   스파게티

```json
GET /recipe/_search
{
  "size": 0,
  "aggs": {
    "my_filter": {
      "filters": {
        "filters": {
          "pasta": {
            "match": {
              "title": "pasta"
            }
          },
          "spaghetti": {
            "match": {
              "title": "spaghetti"
            }
          }
        }
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
      "value" : 21,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [ ]
  },
  "aggregations" : {
    "my_filter" : {
      "buckets" : {
        "pasta" : {
          "doc_count" : 9
        },
        "spaghetti" : {
          "doc_count" : 4
        }
      }
    }
  }
}
```
