# Deleting Documents

-   지우는 쿼리는 `DELETE` 메서드 사용하기

    ```json
    DELETE /products/_doc/101
    # DELETE /products/_doc/101
    #! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.15/security-minimal-setup.html to enable security.
    {
    "_index" : "products",
    "_type" : "_doc",
    "_id" : "101",
    "_version" : 3,
    "result" : "deleted",
    "_shards" : {
        "total" : 3,
        "successful" : 1,
        "failed" : 0
    },
    "_seq_no" : 10,
    "_primary_term" : 4
    }

    # GET /products/_doc/101
    #! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.15/security-minimal-setup.html to enable security.
    {
    "_index" : "products",
    "_type" : "_doc",
    "_id" : "101",
    "found" : false
    }

    ```

-   지워진 document 조회를 시도한 결과

    ```json
    GET / products / _doc / 101

    #! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.15/security-minimal-setup.html to enable security.
    {
    "_index" : "products",
    "_type" : "_doc",
    "_id" : "101",
    "found" : false
    }

    ```
