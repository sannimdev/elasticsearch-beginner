# Querying by parent ID

```json
GET /department/_search
{
  "query": {
    "parent_id": {
      "type": "employee",
      "id": 1
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
        "max_score": 0.2876821,
        "hits": [
            {
                "_index": "department",
                "_type": "_doc",
                "_id": "3",
                "_score": 0.2876821,
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
