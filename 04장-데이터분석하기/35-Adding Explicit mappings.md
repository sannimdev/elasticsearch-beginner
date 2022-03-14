# Adding Explicit Mappings

```json
PUT /reviews
{
    "mappings": {
        "properties": {
            "rating": { "type": "float" },
            "content": { "type": "text" },
            "product_id": { "type": "integer" },
            "author": {
                "properties": {
                    "first_name": { "type": "text" },
                    "last_name": { "type": "text" },
                    "email": { "type": "keyword" }
                }
            }
        }
    }
}
```

```json
{
    "acknowledged": true,
    "shards_acknowledged": true,
    "indeX": "reviews"
}
```

-   원한다면 이것보다 더 깊게 중첩된 객체를 만들 수 있다.
    그래서 중첩된 "이메일" 필드의 '키워드' 데이터 유형을 선택한 것이다.
-   text, keyword 타입을 선정했을 때는 어떻게 쿼리가 처리될 것인가가 중요하다.
    -   이메일 주소를 full-text로 처리할 일은 아마 없을 것이다

```json
PUT /reviews/_doc/1
{
    "rating": 5.0,
    "content": "Outstanding course! Bo really taught me a lot about Elasticsearch!",
    "product_id": 123,
    "author": {
        "first_name": "John",
        "last_name": "Doe",
        "email": "johndoe123@example.com"
    }
}
```

```json
{
    "_index": "reviews",
    "_type": "doc",
    "_id": "1",
    "_version": 1,
    "result": "created",
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "_seq_no": 0,
    "_primary_team": 1
}
```
