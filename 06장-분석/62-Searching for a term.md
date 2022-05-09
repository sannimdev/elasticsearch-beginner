# Searchign for a term

## GET /products/default/\_search

```json
{
    "query": {
        "terms": {
            "FIELD": {
                "value": "VALUE"
            }
        }
    }
}
```

```json
GET /products/default/\_search
{
    "query": {
        "terms": {
            "is_active": true
        }
    }
}

GET /products/default/\_search
{
    "query": {
        "terms": {
            "is_active": {
                "value": true
            }
        }
    }
}
```
