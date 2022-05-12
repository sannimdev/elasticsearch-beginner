# Matching documents with non-null values

## GET /product/default/\_search

```json
{
    "query": {
        "exists": {
            "field": "tags"
        }
    }
}
```
