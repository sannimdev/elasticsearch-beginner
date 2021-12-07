# Queries

-   Queries는 Postman 등 다른 프로그램을 이용하여서도 HTTP Rest API를 호출할 수 있으나 이번 과정에서는 이런 것들을 이용하여 HTTP Rest API를 사용하는 것은 다루지 않을 것임.

1. GET /.kibana/\_search

```json
{
    "query": {
        "match_all": {}
    }
}
```

-   Elasticsearch Cloud 서비스는 curl 날리는 방식이 차이가 있다. (나중에)
