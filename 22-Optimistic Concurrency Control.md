# Optimistic Concurrency Control

-   낙관적 동시성 제어
-   DBMS에 트랜잭션이 있다면 Elasticesarch에는 동시성 제어가 있다!

## 소개

-   동시성 제어 연산으로 인해 의도치 않게 덮어씌워지는 현상을 방지
-   일어날 수 있는 많은 시나리오가 있다
    -   웹앱에 동시에 방문한 사용자를 제어할 때

## 이전 방식의 동시성 제어

-   사용자 A, B가 동시에 접속하여 production을 요청
    ```json
    {
        "price": 99,
        "in_stock": 6,
        "_version": 1
    }
    ```
-   사용자 A도 B도 모두 재고를 0으로 만들어 소진하기를 원함.
    -   A가 먼저 도착하였음. (`POST /products/_update/100?version=1`)
        -   `_version`이 2로 변경됨
    -   B가 먼저 도착하였음. (`POST /products/_update/100?version=2`)
        -   오류
-   `_version`

## 현재 방식 (시퀀스 번호)

1. 데이터를 조회

    ```json
    GET /products/_doc/100
    #! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.15/security-minimal-setup.html to enable security.
    {
        "_index" : "products",
        "_type" : "_doc",
        "_id" : "100",
        "_version" : 9,
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

2. `POST /products/_update/100?if_primary_term=1&if_seq_no=71` 로 데이터 요청

    ```json
    {
        "_index": "products",
        "_type": "_doc",
        "_id": "100",
        "_version": 9,
        "result": "updated",
        "_shards": {
            "total": 3,
            "successful": 1,
            "failed": 0
        },
        "_seq_no": 11,
        "_primary_term": 7
    }
    ```

3. 2번에서 요청 시 오류가 난 경우

    ```json
    {
        "error": {
            "root_cause": [
                {
                    "type": "version_conflict_engine_exception",
                    "reason": "[100]: version conflict, required seqNo [9], primary term [4]. current document has seqNo [11] and primary term [7]",
                    "index_uuid": "kKAUPpLBRCixlmQu9RySCg",
                    "shard": "0",
                    "index": "products"
                }
            ],
            "type": "version_conflict_engine_exception",
            "reason": "[100]: version conflict, required seqNo [9], primary term [4]. current document has seqNo [11] and primary term [7]",
            "index_uuid": "kKAUPpLBRCixlmQu9RySCg",
            "shard": "0",
            "index": "products"
        },
        "status": 409
    }
    ```

## 어떻게 실패를 다룰 수 있을까?

-   앱 단에서 상황을 제어하는 경우
    -   document를 다시 조회하도록 요청하기
    -   `_primary_term`과 `_seq_no`를 이용하여 update 요청을 보내기
    -   필드 값을 다시 사용하는 계산하기
