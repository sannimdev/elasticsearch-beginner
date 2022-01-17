# Replacing Documents

-   새로운 document를 특정 ID로 인덱싱할 때 사용했다.
-   실제로 존재하는 documents를 대체하는 것과 같은 쿼리이다.

## 1. 조회

```json
GET /products/_doc/100

{
  "_index" : "products",
  "_type" : "_doc",
  "_id" : "100",
  "_version" : 7,
  "_seq_no" : 6,
  "_primary_term" : 3,
  "found" : true,
  "_source" : {
    "name" : "Toaster",
    "price" : 49,
    "in_stock" : 6,
    "tags" : [
      "electronics"
    ]
  }
}
```

## 2. PUT

-   PUT을 이용하여 다음의 쿼리문을 실행
    ```json
    PUT /products/_doc/100
    {
    "name": "Toaster",
    "price": 79,
    "in_stock": 4
    }
    #! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.15/security-minimal-setup.html to enable security.
    {
    "_index" : "products",
    "_type" : "_doc",
    "_id" : "100",
    "_version" : 8,
    "result" : "updated",
    "_shards" : {
        "total" : 3,
        "successful" : 1,
        "failed" : 0
    },
    "_seq_no" : 9,
    "_primary_term" : 4
    }
    ```
-   쿼리문을 실행한 이후 document가 대체되었다.

    ```json
    {
        "_index": "products",
        "_type": "_doc",
        "_id": "100",
        "_version": 8,
        "_seq_no": 9,
        "_primary_term": 4,
        "found": true,
        "_source": {
            "name": "Toaster",
            "price": 79,
            "in_stock": 4
        }
    }
    ```

-   tags 항목이 사라진 것을 확인할 수 있다. (왜냐하면 document가 대체되는 것이니까)
