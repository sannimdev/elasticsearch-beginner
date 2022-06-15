# Missing field values

```json
POST /orders/_doc/1001
{
  "total_amount": 100
}

POST /orders/_doc/1002
{
  "total_amount": 200,
  "status": null
}

GET /orders/_search
{
  "size": 0,
  "aggs": {
    "orders_without_status": {
      "missing": {
        "field": "status"
      }
    }
  }
}

#! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.15/security-minimal-setup.html to enable security.
{
  "took" : 2,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 1002,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [ ]
  },
  "aggregations" : {
    "orders_without_status" : {
      "doc_count" : 2
    }
  }
}


DELETE /orders/_doc/1001
DELETE /orders/_doc/1002
```
