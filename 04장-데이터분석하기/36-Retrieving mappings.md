# Retrieving mappings

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

## GET /reviews/\_mapping/field/content

```json
{
    "reviews": {
        "mappings": {
            "content": {
                "full_name": "content",
                "mapping": {
                    "content": {
                        "type": "text"
                    }
                }
            }
        }
    }
}
```

## GET /reviews/\_mapping/field/author.email

```json
{
    "reviews": {
        "mappings": {
            "author.email": {
                "full_name": "author.email",
                "mapping": {
                    "email": {
                        "type": "keyword"
                    }
                }
            }
        }
    }
}
```
