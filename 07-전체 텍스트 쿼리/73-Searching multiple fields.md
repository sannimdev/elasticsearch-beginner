# Searching multiple fields

## GET /recipe/default/\_search

-   다중 언더스코어 일치

```json
{
    "query": {
        "multi_search": {
            "query": "pasta",
            "fields": ["title", "description"]
        }
    }
}
```
