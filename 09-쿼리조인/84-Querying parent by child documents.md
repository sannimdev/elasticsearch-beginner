# Querying parent by child documents

## Score mode

-   min: 하위 document가 부모와 매핑되는 가장 낮은 점수
-   max: 하위 document가 부모와 매핑되는 가장 높은 점수
-   sum: 하위 document의 점수를 총합한 것
-   avg: 하위 document의 점수에 평균
-   none: child document의 점수는 무시됨. (기본)

```json
GET /department/\_search
{
    "query": {
        "has_child": {
            "type": "employee",
            "score_mode": "sum",
            "max_children": 10,
            "min_children": 1,
            "query": {
                "bool": {
                    "must": [
                        {
                            "range": {
                                "age": {
                                    "gte": 20
                                }
                            }
                        }
                    ],
                    "should": [
                        {
                            "term": {
                                "gender.keyword": "M"
                            }
                        }
                    ]
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
                "_id": "1",
                "_score": 1.0,
                "_source": {
                    "name": "Development",
                    "join_field": "department"
                }
            }
        ]
    }
}
```
