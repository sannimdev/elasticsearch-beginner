# Debugging bool queries with named quries

-   문서의 쿼리와 일치하거나 일치하지 않은 이유 이해하기

```json
GET /recipe/default/_search
{
    "query": {
        "bool": {
            "must": [
                {
                    "match": {
                        "ingredients": {
                            "query": "parmesan",
                            "_name": "parmesan_must"
                        }
                    }
                }
            ],
            "must_not": [
                {
                    "match": {
                        "ingredients": {
                            "query": "tuna",
                            "_name": "tuna_must_not"
                        }
                    }
                }
            ],
            "should": [
                {
                    "match": {
                        "ingredients": {
                            "query": "parsley",
                            "_name": "parsley_should"
                        }
                    }
                }
            ],
            "filter": [
                {
                    "range": {
                        "preparation_time_minutes": {
                            "lte": 15,
                            "_name": "prep_time_filter"
                        }
                    }
                }
            ]
        }
    }
}
```
