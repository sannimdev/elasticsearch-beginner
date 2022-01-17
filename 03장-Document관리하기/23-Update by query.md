# Update by query

## 소개

-   이미 document 한 개를 업데이트하는 것은 지난 시간에 다뤘다.
-   여러 개의 documents를 하나의 쿼리를 이용하여 업데이트 하는 작업을 이번에 다룰 것이다.
    -   RDBMS의 `UPDATE WHERE` 구문과 유사하다.
-   이미 다뤘던 3개의 개념을 이용하여 update한다
    -   Primary terms
    -   Sequence numbers
    -   Optimistic concurrency control

## 실습

-   적용시킬 document의 조건을 입력하지 않으면 모든 document에 적용된다는 점을 유의한다.

    ```json
    POST /products/\_update_by_query
    {
        "script": {
            "source": "ctx._source.in_stock--"
        },
        "query": {
            "match_all": {}
        }
    }
    ```

-   결과화면은 다음과 같다

    ```json
    #! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.15/security-minimal-setup.html to enable security.
    {
        "took": 35,
        "timed_out": false,
        "total": 2,
        "updated": 2,
        "deleted": 0,
        "batches": 1,
        "version_conflicts": 0,
        "noops": 0,
        "retries": {
            "bulk": 0,
            "search": 0
        },
        "throttled_millis": 0,
        "requests_per_second": -1.0,
        "throttled_until_millis": 0,
        "failures": []
    }
    ```

-   데이터를 조회하면 다음과 같이 줄어 있는 것을 확인할 수 있다.

    ```json
    GET /products/_search
    {
        "query": {
            "match_all": {}
        }
    }
    ```

    ```json
    #! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.15/security-minimal-setup.html to enable security.
    {
        "took": 2,
        "timed_out": false,
        "_shards": {
            "total": 2,
            "successful": 2,
            "skipped": 0,
            "failed": 0
        },
        "hits": {
            "total": {
                "value": 2,
                "relation": "eq"
            },
            "max_score": 1.0,
            "hits": [
                {
                    "_index": "products",
                    "_type": "_doc",
                    "_id": "100",
                    "_score": 1.0,
                    "_source": {
                        "price": 79,
                        "name": "Toaster",
                        "in_stock": 122
                    }
                },
                {
                    "_index": "products",
                    "_type": "_doc",
                    "_id": "mWFiC34BCa8sV_CAOAkt",
                    "_score": 1.0,
                    "_source": {
                        "price": 64,
                        "name": "Coffee Maker",
                        "in_stock": 9
                    }
                }
            ]
        }
    }
    ```

## 스냅숏이 어떻게 사용되는가

-   스냅숏을 찍은 이후 덮어쓰기로 인해 값이 변하는 것을 방지
    -   많은 문서를 업데이트하는 경우 쿼리를 완료하는 데 시간이 걸릴 수 있음
-   각각의 document의 primary term과 sequence number가 사용된다.
    -   스냅숏의 값과 일치하는 경우에만 문서가 업데이트된다
    -   알다시피, 이것을 `Optimistic Concurrency Control(낙관적 동시성 제어)`이라 한다.
-   `#` 버전 충돌의 경우 `version_conflicts` 키가 반환된다.
