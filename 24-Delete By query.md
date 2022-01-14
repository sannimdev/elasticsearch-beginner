# Delete by query

## 소개

-   우리는 하나의 document를 지우는 것을 배웠다.
-   그런데 이번에는 쿼리 하나를 가지고 다수 의 documents를 지우는 방법을 배울 것이다
-   Query는 Update Query API와 유사하다.
    -   조건을 명시하지 않으면 모든 documents가 삭제된다.

```json
POST /products/_delete_by_query
{
  "query": {
    "match_all": {}
  }
}
```

```json
#! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.15/security-minimal-setup.html to enable security.
{
  "took" : 21,
  "timed_out" : false,
  "total" : 2,
  "deleted" : 2,
  "batches" : 1,
  "version_conflicts" : 0,
  "noops" : 0,
  "retries" : {
    "bulk" : 0,
    "search" : 0
  },
  "throttled_millis" : 0,
  "requests_per_second" : -1.0,
  "throttled_until_millis" : 0,
  "failures" : [ ]
}

```
