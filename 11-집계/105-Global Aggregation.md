# Global Aggregation

```json
GET /orders/_search
{
  "query": {
    "range": {
      "total_amount": {
        "gte": 100
      }
    }
  },
  "size": 0,
  "aggs": {
    "stats_expensive": {
      "stats": {
        "field": "total_amount"
      }
    },
    "all_orders": {
      "global": {},
      "aggs": {
        "stats_amount": {
          "stats": {
            "field": "total_amount"
          }
        }
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
      "value" : 489,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [ ]
  },
  "aggregations" : {
    "all_orders" : {
      "doc_count" : 1000,
      "stats_amount" : {
        "count" : 1000,
        "min" : 10.27,
        "max" : 281.77,
        "avg" : 109.20961,
        "sum" : 109209.61
      }
    },
    "stats_expensive" : {
      "count" : 489,
      "min" : 100.05,
      "max" : 281.77,
      "avg" : 157.32703476482618,
      "sum" : 76932.92
    }
  }
}


```
