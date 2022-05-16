# Querying with boolean logic

-   쿼리는 쿼리 컨텍스트 또는 필터 컨텍스트에서 실행될 수 있다.
-   차이는 쿼리 컨텍스트 내에서 쿼리를 이해하는 데 필수적

## 점수는 어떻게 계산되고 document가 정렬될까?

### 필터 컨텍스트

-   조회 메트릭은 document가 일치하는지 여부만 결정된다
-   쿼리와 얼마나 잘 일치하는지 여부만 결정하고 얼마나 잘 일치하는지의 여부는 결정하지 않는다.
    ```json
    GET /recipe/default/\_search
    {
        "query": {
            "bool": {
                "must": [
                    {
                        "match": {
                            "ingredients.name": "parmesan"
                        }
                    },
                    {
                        "range": {
                            "preparation_time_minutes": {
                                "lte": 15
                            }
                        }
                    }
                ]
            }
        }
    }
    ```
-   Elasticsearch는 미스터리 내에 배치될 때 범위 쿼리에 1의 일정한 점수를 적용할 수 있을 만큼
    충분히 똑똑하지만, 필터 개체는 성능 면에서 우위를 갖는다.
    GET /recipe/\_search
    ```json
    {
        "query": {
            "bool": {
                "must": [
                    {
                        "match": {
                            "ingredients.name": "parmesan"
                        }
                    }
                ],
                "must_not": [
                    {
                        "match": {
                            "ingredients.name": "tuna"
                        }
                    }
                ],
                "should": [
                    {
                        "match": {
                            "ingredients.name": "parsley"
                        }
                    }
                ],
                "filter": [
                    {
                        "range": {
                            "preparation_time_minutes": {
                                "lte": 15
                            }
                        }
                    }
                ]
            }
        }
    }
    ```
