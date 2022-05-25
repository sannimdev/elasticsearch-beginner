# Adding Documents

```json
PUT /department/_doc/1
{
  "name": "Development",
  "join_field": "department"
}

PUT /department/_doc/2
{
  "name": "Marketing",
  "join_field": "department"
}
```

```json
PUT /department/_doc/3?routing=1
{
  "name": "Bo Andersen",
  "age": 28,
  "gender": "M",
  "join_field": {
    "name": "employee",
    "parent": 1
  }
}

{
    "_index": "department",
    "_type": "_doc",
    "_id": "3",
    "_version": 1,
    "result": "created",
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "_seq_no": 1,
    "_primary_term": 1
}
```
