# Indexing Documents

## 1) 상품 입력하기

-   쿼리

    ```json
    POST /products/_doc
    {
        "name": "Coffee Maker",
        "price": 64,
        "in_stock": 10
    }
    ```

-   결과

    ```json
    #! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.15/security-minimal-setup.html to enable security.
    {
    "_index" : "products",
    "_type" : "_doc",
    "_id" : "mWFiC34BCa8sV_CAOAkt",
    "_version" : 1,
    "result" : "created",
    "_shards" : {
        "total" : 3, // index는 2개의 샤드를 포함하고 2개의 replica 샤드를 포함한다.
        "successful" : 1,
        "failed" : 0
    },
    "_seq_no" : 0,
    "_primary_term" : 1
    }
    ```

-   Document는 그래서 문서는 주 샤드와 두 개의 복제 샤드에 추가되었고, 함께 복제 그룹을 만들었다. (2+1)

## 2) 상품 입력하기

-   쿼리
    ```json
    PUT /products/_doc/100 👈
    {
        "name": "Toaster",
        "price": 49,
        "in_stock": 4
    }
    ```
-   결과

    ```json
    {
        "_index": "products",
        "_type": "_doc",
        "_id": "100",  👈
        "_version": 1,
        "result": "created",
        "_shards": {
            "total": 3,
            "successful": 1,
            "failed": 0
        },
        "_seq_no": 0,
        "_primary_term": 1
    }
    ```

또 하나 눈여겨볼 점은 우리가 미리 인덱스를 만들 필요도 없었다는 점이다.
