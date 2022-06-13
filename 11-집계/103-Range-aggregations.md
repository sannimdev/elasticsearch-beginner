# Range Aggregations

## 1. Named range

```json
GET /orders/_search
{
  "size": 0,
  "aggs": {
    "amount_distribution": {
      "range": {
        "field": "total_amount",
        "ranges": [
          {
            "to": 50
          },
          {
            "from": 50,
            "to": 100
          },
          {
            "from": 100
          }
        ]
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
          "key" : "*-50.0",
          "to" : 50.0,
          "doc_count" : 164
        },
        {
          "key" : "50.0-100.0",
          "from" : 50.0,
          "to" : 100.0,
          "doc_count" : 347
        },
        {
          "key" : "100.0-*",
          "from" : 100.0,
          "doc_count" : 489
        }
      ]
    }
  }
}

```

## 2. Named date underscore range

```json
GET /orders/_search
{
  "size": 0,
  "aggs": {
    "amount_distribution": {
      "range": {
        "field": "purchased_at",
        "ranges": [
          {
            "from": "2016-01-01",
            "to": "2016-01-01||+6M"
          },
          {
            "from": "2016-01-01||+6M",
            "to": "2016-01-01||+1y"
          }
        ]
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
    "amount_distribution" : {
      "buckets" : [
        {
          "key" : "2016-01-01T00:00:00.000Z-2016-07-01T00:00:00.000Z",
          "from" : 1.4516064E12,
          "from_as_string" : "2016-01-01T00:00:00.000Z",
          "to" : 1.4673312E12,
          "to_as_string" : "2016-07-01T00:00:00.000Z",
          "doc_count" : 481
        },
        {
          "key" : "2016-07-01T00:00:00.000Z-2017-01-01T00:00:00.000Z",
          "from" : 1.4673312E12,
          "from_as_string" : "2016-07-01T00:00:00.000Z",
          "to" : 1.4832288E12,
          "to_as_string" : "2017-01-01T00:00:00.000Z",
          "doc_count" : 519
        }
      ]
    }
  }
}

```
