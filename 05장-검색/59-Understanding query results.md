# Understanding query results

```json
{
    "took": 1,
    "timed_out": false,
    "_shards": {
        "total": 5,
        "successful": 5, 성공 여부
        "skipped": 0,
        "failed": 0 실패 여부
    },
    "hits": {
       "total": 1000,
       "max_score": 1, // 연관성 점수 최대치
       // 사실 hits 속성은 hit 속성 자체를 포함한다
       "hits": [
           {
               "_index": "product",
               "_type": "default",
               "_id":  "14",
               "_score": 1,
               "_source": {
                   "name": "Nori Sea Weed - Gold Label",
                   "price": 177,
                   "in_stock": 21,
                   "sold": 411,
                   "tags": [],
                   "description": "...",
                   "is_active": true,
                   "created": "2000/02/16"
               }
           }
       ]
    }
}
```
