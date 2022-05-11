# Working with relative dates

## GET /product/default/\_search

```json
{
    "query": {
        "range": {
            "created": {
                "gte": "2010/01/01||-1y-1d "
            }
        }
    }
}
```

-   위의 쿼리문대로 연산을 하면 2009년 제품을 확인할 수 있다.
-   `m`은 분을 나타내고 월은 `M` (대소문자 구분)
