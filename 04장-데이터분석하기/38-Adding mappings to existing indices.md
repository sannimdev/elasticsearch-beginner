# Adding mappings to existing indicies

## PUT /reviews/\_mapping

-   기존에 인덱스를 만들 때 `mapping`이라는 키워드를 사용하였지만, 인덱스에 속성을 추가하는 경우에는 `mapping`키워드는 입력할 필요가 없다.

```json
{
    "properties": {
        "created_at": {
            "type": "date"
        }
    }
}
```

## GET /reviews/\_mapping

```json
{
    "reviews": {
        "mappings": {
            "properties": {
                "author": {
                    "properties": {
                        "email": {
                            "type": "keyword"
                        },
                        "first_name": {
                            "type": "text"
                        },
                        "last_name": {
                            "type": "text"
                        }
                    }
                },
                "content": {
                    "type": "text"
                },
                "created_at": {
                    "type": "date"
                },
                "product_id": {
                    "type": "integer"
                },
                "rating": {
                    "type": "float"
                }
            }
        }
    }
}
```
