# Using dot notation in field names

## PUT /reivews

```json
{
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
            "product_id": {
                "type": "integer"
            },
            "rating": {
                "type": "float"
            }
        }
    }
}
```

## PUT /reviews_dot_notation

```json
{
    "mappings": {
        "properties": {
            "author.first_name": { "type": "text" },
            "author.last_name": { "type": "text" },
            "author.email": { "type": "text" },
            "content": {
                "type": "text"
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
```

-   `.` 마침표 표기법을 사용하여 인덱스를 생성하더라도 mapping을 조회하면 정확히 똑같다.

## GET /reviews_dot_notation/\_mapping

```json
{
    "reviews_dot_notation": {
        "mappings": {
            "properties": {
                "author": {
                    "properties": {
                        "email": {
                            "type": "text"
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
