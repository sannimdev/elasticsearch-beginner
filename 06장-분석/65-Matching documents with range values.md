# Matching documents with range values

## GET /product/default/\_search

```json
{
    "query": {
        "range": {
            "in_stock": {
                "gte": 1,
                "lte": 5
            }
        }
    }
}

{
    "query": {
        "range": {
            "created": {
                "gte": "2010/01/01",
                "lte": "2010/12/31",
            }
        }
    }
}

{
    "query": {
        "range": {
            "created": {
                "gte": "01-01-2010",
                "lte": "31-12-2010",
                "format": "dd-MM-yyyy"
            }
        }
    }
}
```
