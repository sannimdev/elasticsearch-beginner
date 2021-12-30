# Indice

## (복습) Indicies

-   [Index vs Indicies](https://racoonlotty.tistory.com/entry/Elasticsearch-Index-vs-Indices)

Elasticsearch에서 사용하는 용어

-   Index: `색인`한 데이터
-   Indexing: 색인하는 과정
-   Indicies: `매핑 정보를 저장하는 논리적인 데이터 공간`

## 삭제하기

```
DELETE /pages
```

```json
{
    "acknowledged": true
}
```

```
PUT /products
{
    "settings": {
        "number_of_shards": 2,
        "number_of_replicas": 2
    }
}
```

```json
{
    "acknowledged": true,
    "shards_acknowledged": true,
    "index": "products"
}
```
