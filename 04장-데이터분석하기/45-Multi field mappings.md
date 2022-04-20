# Multi field Mappings

```json
PUT /multi_field_test
{
  "mappings": {
    "properties": {
      "description": {
        "type": "text"
      },
      "ingredients": {
        "type": "text",
        "fields": {
          "keyword": {
            "type": "keyword"
          }
        }
      }
    }
  }
}
```

```json
POST /multi_field_test/_doc
{
  "description": "To make this spaghetti carbonara, you first need to...",
  "ingredients": ["Spaghetti", "Bocon", "Eggs"]
}
```

```json
{
    "_index": "multi_field_test",
    "_type": "_doc",
    "_id": "WTmaQYABsm8WXt9wwb1O",
    "_version": 1,
    "result": "created",
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "_seq_no": 0,
    "_primary_term": 1
}
```

## 검색

```json

GET /multi_field_test/_search
{
  "query": {
    "match_all": {}
  }
}
```

```json
{
    "took": 2,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 1,
            "relation": "eq"
        },
        "max_score": 1.0,
        "hits": [
            {
                "_index": "multi_field_test",
                "_type": "_doc",
                "_id": "WTmaQYABsm8WXt9wwb1O",
                "_score": 1.0,
                "_source": {
                    "description": "To make this spaghetti carbonara, you first need to...",
                    "ingredients": ["Spaghetti", "Bocon", "Eggs"]
                }
            }
        ]
    }
}
```

## 레벨 검색

```json
GET /multi_field_test/_search
{
  "query":{
    "term": {
      "ingredients.keyword": "Spaghetti"
    }
  }
}
```

```json
{
    "took": 2,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 1,
            "relation": "eq"
        },
        "max_score": 0.39556286,
        "hits": [
            {
                "_index": "multi_field_test",
                "_type": "_doc",
                "_id": "WTmaQYABsm8WXt9wwb1O",
                "_score": 0.39556286,
                "_source": {
                    "description": "To make this spaghetti carbonara, you first need to...",
                    "ingredients": ["Spaghetti", "Bocon", "Eggs"]
                }
            }
        ]
    }
}
```

```
DELETE /multi_field_test
```
