# Affecting relevance scoring with proximity

-   관련 점수가 어떻게 계산되는지느 ㄴ간단하지 않다
-   document가 가장 근접성이 낮은 용어라는 보장은 없다.
-   관련 요소를 계산할 때 다른 여러가지 요소들도 고려하기 때문이다
-   근접성이라는 용어는 그중 하나일 분이다.

따라서 효과는 있지만, 관련 데이터를 계산할 때 사용되는 유일한 요소는 아니다
점수가 근접이라고 해서 반드시 반영된다는 보장은 없다

```json
GET /proximity/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "title": {
              "query": "spicy sauce"
            }
          }
        }
      ]
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
      "value" : 5,
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
        "_score" : 0.19042279,
        "_source" : {
          "title" : "Spicy Tomato Sauce"
        }
      },
      {
        "_index" : "proximity",
        "_type" : "_doc",
        "_id" : "4",
        "_score" : 0.19042279,
        "_source" : {
          "title" : "Tomato Sauce (spicy)"
        }
      },
      {
        "_index" : "proximity",
        "_type" : "_doc",
        "_id" : "3",
        "_score" : 0.15411335,
        "_source" : {
          "title" : "Spicy Tomato and Garlic Sauce"
        }
      },
      {
        "_index" : "proximity",
        "_type" : "_doc",
        "_id" : "5",
        "_score" : 0.14069924,
        "_source" : {
          "title" : "Spicy and very delicious Tomato Sauce"
        }
      }
    ]
  }
}

```
