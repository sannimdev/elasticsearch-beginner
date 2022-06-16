# Proximity searches

```json
PUT /proximity/_doc/1
{
  "title": "Spicy Sauce"
}
PUT /proximity/_doc/2
{
  "title": "Spicy Tomato Sauce"
}
PUT /proximity/_doc/3
{
  "title": "Spicy Tomato and Garlic Sauce"
}
PUT /proximity/_doc/4
{
  "title": "Tomato Sauce (spicy)"
}
PUT /proximity/_doc/5
{
  "title": "Spicy and very delicious Tomato Sauce"
}

GET /proximity/_search
{
  "query": {
    "match_phrase": {
      "title": {
        "query": "spicy sauce",
        "slop": 1 <- proximity
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
      "value" : 2,
      "relation" : "eq"
    },
    "max_score" : 0.21585016,
    "hits" : [
      {
        "_index" : "proximity",
        "_type" : "_doc",
        "_id" : "1",
        "_score" : 0.21585016,
        "_source" : {
          "title" : "Spicy Sauce"
        }
      },
      {
        "_index" : "proximity",
        "_type" : "_doc",
        "_id" : "2",
        "_score" : 0.12672737,
        "_source" : {
          "title" : "Spicy Tomato Sauce"
        }
      }
    ]
  }
}


```
