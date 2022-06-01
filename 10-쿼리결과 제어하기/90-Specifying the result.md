# Specifying the result

## format

-   Feel free to skip this lecture if that's not something interesting to you
-   하지만, 어떻게 동작하는지 보여 줘야 한다.

실속있는 것만 추출하여 필기

### yaml 포맷으로 보여주기

-   GET /recipe/\_search?format=yaml

## size

-   GET /recipe/\_search?size=2

```json
GET /recipe/\_search
{
    "_source": false,
    "size": 2,
    "query": {
        "match": { "title": "photo" }
    }
}
```

## offset

-   GET /recipe/\_search?offset=1

```json
GET /recipe/\_search
{
    "_source": false,
    "size": 2,
    "from": 2,
    "query": {
        "match": { "title": "photo" }
    }
}
```
