# Querying child documents by parent

-   ID기반으로 child document를 찾는 방법
-   has_parent

```json
GET /department/\_search
{
    "query": {
        "has_parent": {
            "parent_type": "department",
            "query": {
                "term": {
                    "name.keyword": "Development"
                }
            }
        }
    }
}
```

```json
{
    "took": 1,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 1,
            "relation": "eq"
        },
        "max_score": 1.0,
        "hits": [
            {
                "_index": "department",
                "_type": "_doc",
                "_id": "3",
                "_score": 1.0,
                "_routing": "1",
                "_source": {
                    "name": "Bo Andersen",
                    "age": 28,
                    "gender": "M",
                    "join_field": {
                        "name": "employee",
                        "parent": 1
                    }
                }
            }
        ]
    }
}
```

-   score 항목 추가

```json

GET /department/_search
{
  "query": {
    "has_parent": {
      "parent_type": "department",
      "score": true,
      "query": {
        "term": {
          "name.keyword": "Development"
        }
      }
    }
  }
}
```

```json
{
    "took": 4,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 1,
            "relation": "eq"
        },
        "max_score": 1.89712,
        "hits": [
            {
                "_index": "department",
                "_type": "_doc",
                "_id": "3",
                "_score": 1.89712,
                "_routing": "1",
                "_source": {
                    "name": "Bo Andersen",
                    "age": 28,
                    "gender": "M",
                    "join_field": {
                        "name": "employee",
                        "parent": 1
                    }
                }
            }
        ]
    }
}
```
