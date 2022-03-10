# Understand Arrays

## Introduction to arrays

-   배열 데이터 타입은 존재하지 않는다
-   모든 필드에는 0 또는 그 이상의 값이 포함된다.
    -   구성이나 매핑이 필요하지 않음
    -   Document를 인덱싱할 때 배열을 제공하기만 하면 된다.
-   제품 인덱스의 태그 필드에 대해 이 작업을 수행했던 적이 있다.
    ```json
    POST /products/_doc
    {
        "tags": "smartphone"
    }
    ```
    ```json
    POST /products/_doc
    {
        "tags": ["Smartphone", "Electronics"]
    }
    ```
    ```json
    {
        "products": {
            "mappings": {
                "properties": {
                    "tags": {
                        "type": "text"
                    }
                }
            }
        }
    }
    ```
-   텍스트 필드의 경우에는 분석되기 이전에 문자열이 간단히 연결된다.
-   result 토큰은 inverted index에 정상적으로 저장된다.
    ```json
    POST /_analyze
    {
    "text": ["Strings are simply", "merged together"],
    "analyzer": "standard"
    }
    ```
    ```json
    {
        "tokens": [
            {
                "token": "strings",
                "start_offset": 0,
                "end_offset": 7,
                "type": "<ALPHANUM>",
                "position": 0
            },
            {
                "token": "are",
                "start_offset": 8,
                "end_offset": 11,
                "type": "<ALPHANUM>",
                "position": 1
            },
            {
                "token": "simply",
                "start_offset": 12,
                "end_offset": 18,
                "type": "<ALPHANUM>",
                "position": 2
            },
            {
                "token": "merged",
                "start_offset": 19,
                "end_offset": 25,
                "type": "<ALPHANUM>",
                "position": 3
            },
            {
                "token": "together",
                "start_offset": 26,
                "end_offset": 34,
                "type": "<ALPHANUM>",
                "position": 4
            }
        ]
    }
    ```
    -   이는 문자열이 실제로는 여러 개의 값이 아닌 하나의 문자열로서 처리된다는 것이다.

## 제약사항

-   배열은 같은 데이터 타입이어야 한다.
    -   매핑을 통한 타입 강제를 하는 한 데이터 유형을 섞을 수는 있다.
-   타입 강제는 이미 매핑된 필드에 한하여 작동한다
-   그러나 타입 강제를 사용하는 것을 추천하지는 않음 (적어도 고의는 아니더라도)

## 중첩된 배열

-   배열은 중첩된 값을 포함할 수도 있다
-   중첩된 배열은 인덱싱할 때 1차원 배열로 납작해진다.
-   [1, [2, 3]] => [1,2,3]이 된다.

## 기억할 것

-   객체를 독립적으로 쿼리해야 하는 경우 개체 배열에 중첩 데이터 유형을 사용하도록 기억하라(?)
