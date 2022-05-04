# Search methods

```json
GET /product/default/_search
{
    "query": {
        "match": {
            "description": {
                "value": "red wine"
            }
        }
    }
}
```

```json
GET /product/default/_search
{
    "query": {
        "match": {
            "description": "red wine"
        }
    }
}
```

GET /product/default/\_search?q=name:pasta

```json
GET /product/default/_search
{
    "query": {
        "query_string": {
            "query": "name:pasta"
        }
    }
}
```
