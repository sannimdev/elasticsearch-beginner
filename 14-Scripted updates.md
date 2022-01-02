# Scripted updates

## 1. 스크립트로 재고 1 빼기

```json
POST /products/_update/100
{
  "script": {
    "source": "ctx._source.in_stock--"
  }
}
```

```json
{
    "_index": "products",
    "_type": "_doc",
    "_id": "100",
    "_version": 4,
    "result": "updated",
    "_shards": {
        "total": 3,
        "successful": 1,
        "failed": 0
    },
    "_seq_no": 3,
    "_primary_term": 3
}
```

## 2. 확인하기

```json
GET / products / _doc / 100
```

```json
{
    "_index": "products",
    "_type": "_doc",
    "_id": "100",
    "_version": 4,
    "_seq_no": 3,
    "_primary_term": 3,
    "found": true,
    "_source": {
        "name": "Toaster",
        "price": 49,
        "in_stock": 2,
        "tags": ["electronics"]
    }
}
```

### 다음과 같은 작업도 가능하다.

-   결과화면 생략

```json
POST /products/_update/100
{
  "script": {
    "source": "ctx._source.in_stock = 10"
  }
}

POST /products/_update/100
{
  "script": {
    "source": "ctx._source.in_stock -= params.quantity",
    "params": {
        "quantity": 4
    }
  }
}
```

## 3. Condition

```json
POST /products/_update/100
{
  "script": {
    "source": """
        if (ctx._source.in_stock == 0) {
            ctx.op = 'noop';
        }

        ctx._source.in_stock--;
    """
  }
}
```

```json
POST /products/_update/100
{
  "script": {
    "source": """
        if (ctx._source.in_stock > 0) {
            ctx._source.in_stock--;
        }
    """
  }
}
```

-   Document가 지워짐

    ```json
    POST /products/_update/100
    {
    "script": {
        "source": """
            if (ctx._source.in_stock <= 1) {
                ctx.op = 'delete';
            }

            ctx._source.in_stock--;
        """
    }
    }
    ```
