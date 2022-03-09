# Introduction to type coercion

-   coerce: 강제하다
-   coercion: 강제, 위압

-   데이터 타입은 Docuemt를 인덱싱할 때 분석된다
-   유효성이 검증되며 유효하지 않은 값들은 거절된다.
-   ex) 텍스트 필드에 객체 값을 입력하는 경우
-   때때로 잘못된 데이터 타입은 통과되기도 한다.

    ```json
    PUT /coercion_test/_doc/1
    {
    "price": 7.4
    }

    PUT /coercion_test/_doc/2
    {
    "price": 7.4
    }

    PUT /coercion_test/_doc/3
    {
    "price": "7.4m"
    }

    GET /coercion_test/_doc/2


    DELETE /coercion_test
    ```

-   문자열 값으로 숫자를 전달 "7.4"했는데 Elasticsearch는 이 값을 분석하게 된다
    -   "7.4" (string) -> 7.4 (float)

## `_source` 객체 이해하기

-   index time ("7.4")를 포함한 값
    -   7.4로 인덱스되지 않음
-   검색된 쿼리는 index된 값을 사용하는 것이지 `_source`를 이용하지 않는다
    -   BKD 트리, 뒤집힌 인덱스 등
-   `_source` 는 어떻게 인덱스되는가를 반영하지 않는다
    -   만약에 \_source에 있는 값을 사용한다면 coercion을 유지한다
    -   예를들어 string값이나 소숫점이 있는 경우

## 더 많은 것

-   정수 필드에 부동 소수점을 제공하면 정수로 잘린다
-   Coercion은 동적 매핑을 사용하지 않는다
    -   "7.4"를 새로운 필드에 제공하는 것은 text 매핑을 만들 것이다
-   항상 올바른 데이터 타입을 넣어줘야 한다
    -   특히 처음 인덱스 필드를 사용할 때
-   coercion을 금지하는 것은 선호도의 문제이다.
    -   기본적으로 활성화되어있음
