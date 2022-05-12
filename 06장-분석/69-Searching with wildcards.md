# Searching with wildcards

## GET /products/\_search

```json
{
    "query": {
        "wildcard": {
            "tags.keyword": "Veg*able"
        }
    }
}
```

```json
#! Elasticsearch built-in security features are not enabled. Without authentication, your cluster could be accessible to anyone. See https://www.elastic.co/guide/en/elasticsearch/reference/7.15/security-minimal-setup.html to enable security.
{
  "took" : 6,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 69,
      "relation" : "eq"
    },
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "products",
        "_type" : "_doc",
        "_id" : "7",
        "_score" : 1.0,
        "_source" : {
          "name" : "Beets - Pickled",
          "price" : 172,
          "in_stock" : 25,
          "sold" : 290,
          "tags" : [
            "Vegetable",
            "Beets"
          ],
          "description" : "Aliquam erat volutpat. In congue. Etiam justo. Etiam pretium iaculis justo. In hac habitasse platea dictumst. Etiam faucibus cursus urna. Ut tellus. Nulla ut erat id mauris vulputate elementum.",
          "is_active" : false,
          "created" : "2008/09/20"
        }
      },
      {
        "_index" : "products",
        "_type" : "_doc",
        "_id" : "9",
        "_score" : 1.0,
        "_ignored" : [
          "description.keyword"
        ],
        "_source" : {
          "name" : "Onions - Green",
          "price" : 105,
          "in_stock" : 22,
          "sold" : 199,
          "tags" : [
            "Vegetable"
          ],
          "description" : "Ut at dolor quis odio consequat varius. Integer ac leo. Pellentesque ultrices mattis odio. Donec vitae nisi. Nam ultrices, libero non mattis pulvinar, nulla pede ullamcorper augue, a suscipit nulla elit ac nulla. Sed vel enim sit amet nunc viverra dapibus. Nulla suscipit ligula in lacus. Curabitur at ipsum ac tellus semper interdum. Mauris ullamcorper purus sit amet nulla. Quisque arcu libero, rutrum ac, lobortis vel, dapibus at, diam.",
          "is_active" : false,
          "created" : "2005/08/01"
        }
      },
      {
        "_index" : "products",
        "_type" : "_doc",
        "_id" : "13",
        "_score" : 1.0,
        "_source" : {
          "name" : "Radish",
          "price" : 91,
          "in_stock" : 13,
          "sold" : 88,
          "tags" : [
            "Vegetable"
          ],
          "description" : "Suspendisse ornare consequat lectus.",
          "is_active" : true,
          "created" : "2014/09/25"
        }
      },
      {
        "_index" : "products",
        "_type" : "_doc",
        "_id" : "29",
        "_score" : 1.0,
        "_source" : {
          "name" : "Baby Corn",
          "price" : 57,
          "in_stock" : 33,
          "sold" : 493,
          "tags" : [
            "Vegetable"
          ],
          "description" : "Duis bibendum. Morbi non quam nec dui luctus rutrum. Nulla tellus. In sagittis dui vel nisl. Duis ac nibh. Fusce lacus purus, aliquet at, feugiat non, pretium quis, lectus. Suspendisse potenti.",
          "is_active" : true,
          "created" : "2013/07/13"
        }
      },
      {
        "_index" : "products",
        "_type" : "_doc",
        "_id" : "33",
        "_score" : 1.0,
        "_source" : {
          "name" : "Beets",
          "price" : 56,
          "in_stock" : 15,
          "sold" : 234,
          "tags" : [
            "Vegetable",
            "Beets"
          ],
          "description" : "Suspendisse potenti.",
          "is_active" : true,
          "created" : "2003/08/11"
        }
      },
      {
        "_index" : "products",
        "_type" : "_doc",
        "_id" : "38",
        "_score" : 1.0,
        "_source" : {
          "name" : "Onions - Pearl",
          "price" : 86,
          "in_stock" : 34,
          "sold" : 247,
          "tags" : [
            "Vegetable"
          ],
          "description" : "Vivamus tortor. Duis mattis egestas metus. Aenean fermentum.",
          "is_active" : false,
          "created" : "2016/10/24"
        }
      },
      {
        "_index" : "products",
        "_type" : "_doc",
        "_id" : "39",
        "_score" : 1.0,
        "_source" : {
          "name" : "Mushroom - Porcini Dry",
          "price" : 28,
          "in_stock" : 33,
          "sold" : 51,
          "tags" : [
            "Vegetable"
          ],
          "description" : "Nulla nisl. Nunc nisl.",
          "is_active" : false,
          "created" : "2013/07/12"
        }
      },
      {
        "_index" : "products",
        "_type" : "_doc",
        "_id" : "41",
        "_score" : 1.0,
        "_source" : {
          "name" : "Sweet Pea Sprouts",
          "price" : 139,
          "in_stock" : 27,
          "sold" : 483,
          "tags" : [
            "Vegetable"
          ],
          "description" : "In eleifend quam a odio. In hac habitasse platea dictumst. Maecenas ut massa quis augue luctus tincidunt. Nulla mollis molestie lorem. Quisque ut erat. Curabitur gravida nisi at nibh. In hac habitasse platea dictumst.",
          "is_active" : false,
          "created" : "2011/01/19"
        }
      },
      {
        "_index" : "products",
        "_type" : "_doc",
        "_id" : "46",
        "_score" : 1.0,
        "_source" : {
          "name" : "Butterbean and white cabbage salad",
          "price" : 113,
          "in_stock" : 28,
          "sold" : 323,
          "tags" : [
            "Vegetable"
          ],
          "description" : "Butterbean and white cabbage served on a bed of lettuce",
          "is_active" : false,
          "created" : "2009/09/24"
        }
      },
      {
        "_index" : "products",
        "_type" : "_doc",
        "_id" : "54",
        "_score" : 1.0,
        "_ignored" : [
          "description.keyword"
        ],
        "_source" : {
          "name" : "Pepper - Green",
          "price" : 92,
          "in_stock" : 34,
          "sold" : 114,
          "tags" : [
            "Vegetable"
          ],
          "description" : "Maecenas tristique, est et tempus semper, est quam pharetra magna, ac consequat metus sapien ut nunc. Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia Curae; Mauris viverra diam vitae quam. Suspendisse potenti. Nullam porttitor lacus at turpis. Donec posuere metus vitae ipsum.",
          "is_active" : true,
          "created" : "2010/07/08"
        }
      }
    ]
  }
}

```
