# Fuzzy match query (handling typos)

-   오타가 나더라도 `fuzziness` 속성으로 알아서 찾아준다.

```json
GET /products/_search
{
  "query":{
    "match":{
      "name": {
        "query": "l0bster", <-  lobster가 아니라 l0ster
        "fuzziness": "auto"
      }
    }
  }
}

#! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.15/security-minimal-setup.html to enable security.
{
  "took" : 83,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 5,
      "relation" : "eq"
    },
    "max_score" : 5.055713,
    "hits" : [
      {
        "_index" : "products",
        "_type" : "_doc",
        "_id" : "19",
        "_score" : 5.055713,
        "_ignored" : [
          "description.keyword"
        ],
        "_source" : {
          "name" : "Lobster - Live",
          "price" : 79,
          "in_stock" : 43,
          "sold" : 370,
          "tags" : [
            "Meat",
            "Seafood"
          ],
          "description" : "Integer non velit. Donec diam neque, vestibulum eget, vulputate ut, ultrices vel, augue. Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia Curae; Donec pharetra, magna vestibulum aliquet ultrices, erat tortor sollicitudin mi, sit amet lobortis sapien sapien non mi. Integer ac neque. Duis bibendum. Morbi non quam nec dui luctus rutrum.",
          "is_active" : false,
          "created" : "2007/08/10"
        }
      },
      {
        "_index" : "products",
        "_type" : "_doc",
        "_id" : "55",
        "_score" : 4.3392005,
        "_source" : {
          "name" : "Lobster - Baby Boiled",
          "price" : 134,
          "in_stock" : 41,
          "sold" : 207,
          "tags" : [
            "Meat",
            "Seafood"
          ],
          "description" : "Nulla tellus. In sagittis dui vel nisl. Duis ac nibh. Fusce lacus purus, aliquet at, feugiat non, pretium quis, lectus. Suspendisse potenti. In eleifend quam a odio.",
          "is_active" : false,
          "created" : "2016/01/19"
        }
      },
      {
        "_index" : "products",
        "_type" : "_doc",
        "_id" : "373",
        "_score" : 3.800571,
        "_ignored" : [
          "description.keyword"
        ],
        "_source" : {
          "name" : "Appetizer - Lobster Phyllo Roll",
          "price" : 153,
          "in_stock" : 32,
          "sold" : 92,
          "tags" : [
            "Meat",
            "Seafood"
          ],
          "description" : "Ut tellus. Nulla ut erat id mauris vulputate elementum. Nullam varius. Nulla facilisi. Cras non velit nec nisi vulputate nonummy. Maecenas tincidunt lacus at velit. Vivamus vel nulla eget eros elementum pellentesque. Quisque porta volutpat erat. Quisque erat eros, viverra eget, congue eget, semper rutrum, nulla. Nunc purus.",
          "is_active" : true,
          "created" : "2012/10/10"
        }
      },
      {
        "_index" : "products",
        "_type" : "_doc",
        "_id" : "471",
        "_score" : 3.800571,
        "_source" : {
          "name" : "Lobster - Tail 6 Oz",
          "price" : 197,
          "in_stock" : 9,
          "sold" : 47,
          "tags" : [
            "Meat",
            "Seafood"
          ],
          "description" : "Aenean lectus. Pellentesque eget nunc. Donec quis orci eget orci vehicula condimentum. Curabitur in libero ut massa volutpat convallis. Morbi odio odio, elementum eu, interdum eu, tincidunt in, leo.",
          "is_active" : true,
          "created" : "2014/10/01"
        }
      },
      {
        "_index" : "products",
        "_type" : "_doc",
        "_id" : "500",
        "_score" : 3.3808966,
        "_source" : {
          "name" : "Lobster - Tail 3 - 4 Oz",
          "price" : 46,
          "in_stock" : 33,
          "sold" : 188,
          "tags" : [
            "Meat",
            "Seafood"
          ],
          "description" : "Integer a nibh. In quis justo. Maecenas rhoncus aliquam lacus. Morbi quis tortor id nulla ultrices aliquet. Maecenas leo odio, condimentum id, luctus nec, molestie sed, justo. Pellentesque viverra pede ac diam.",
          "is_active" : false,
          "created" : "2015/08/26"
        }
      }
    ]
  }
}

```
