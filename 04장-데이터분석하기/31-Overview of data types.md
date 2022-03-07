# Overview of data types.

## Object Data Type

-   JSON 객체가 사용된다
-   Object는 nested 감싸진다.

    ```json
    {
        "name": "Coffee Maker",
        "price": 64.2,
        "in_stock": 10,
        "is_active": true,
        "manufacturer": {
            "name": "Nespresso",
            "country": "Switzerland"
        }
    }
    ```

    ```json
    PUT /products
    {
        "mappings": {
            "properties": {
                "name": { "type": "text" },
                "price": { "type": "double" },
                "in_stock": { "type": "short" },
                "is_active": { "type": "boolean" },
                "manufacturer": {
                    "properties": {
                        "name": { "type": "text" },
                        "country": { "type": "text" }
                    }
                }
            }
        }
    }
    ```

-   속성 매개 변수를 사용하여 매핑됨
-   객체는 Apache Lucene 안에 있는 objects에 저장되지 않는다
    -   유효한 JSON을 인덱싱할 수 있도록 객체가 변환된다.

## 객체 데이터 유형

-   모든 JSON 객체를 포함
-   Elasticsearch에 인덱싱되는 각 문서는 JSON 객체이다.
-   매핑되는 방법은 객체를 지정하는 대신 속성 키를 추가하는 방식
-   객체가 내부에 저장되는 방식이 아니라 Apache Lucene 위에 구축된다
    -   이와 같이 저장하는 이유는 문서를 색인화하기 위함임
    -   문서를 구조화할 피룡가 없어 Elasticsearch를 더 쉽게 사용할 수 있음
-   필드 이름별로 값이 그룹화되고 배열이 인덱싱

### 객체가 저장되는 형식 예시

-   입력받은 값
    ```json
    {
        "name": "Coffee Maker",
        "price": 64.2,
        "in_stock": 10,
        "is_active": true,
        "manufacturer": {
            "name": "Nespresso",
            "country": "Switzerland"
        }
    }
    ```
-   실제로 저장될 때
    ```json
    {
        "name": "Coffee Maker",
        "price": 64.2,
        "in_stock": 10,
        "is_active": true,
        "manufacturer.name": "Nespresso",
        "manufacturer.country": "Switzerland"
    }
    ```

### 배열은?

```json
{
    "name": "Coffee Maker",
    "reviews": [
        {
            "rating": 5.0,
            "author": "Average Joe",
            "description": "Haven't slept for days... Amazing"
        },
        {
            "rating": 3.5,
            "author": "John Doe",
            "description": "Could be better :)"
        }
    ]
}
```

```json
{
    "name": "Coffee Maker",
    "reviews.rating": [5.0, 3.5],
    "reviews.author": ["Average Joe", "John Doe"],
    "reviews.description": ["Haven't slept for days... Amazing!", "Could be better :)"]
}
```

## Nested data type

-   Object 데이터 타입과 유사하지만, 객체 관계를 유지한다.
    -   배열, 객체를 인덱싱할 때 유리하다
-   쿼리 객체를 독립적으로 사용할 수 있음.
    -   nested query를 사용해야만 함
        ```json
        PUT /products
        {
            "mappings": {
                "properties": {
                    "name": { "type": "text" },
                    "reviews": { "type": "nested" }
                }
            }
        }
        ```
-   nested objects는 숨겨진 documents로 저장된다.

## Keyword data type

-   정확한 값을 매칭하는 데 사용됨
-   필터링, 응집, 정렬하는 데 전형적으로 사용된다.
    -   ex) 상탯값이 `PUBLISHED`인 게시글을 검색
-   full-text 검색은 text data type 대신에 사용한다.
    -   ex) 본문 내용을 게시글에서 검색할 때
