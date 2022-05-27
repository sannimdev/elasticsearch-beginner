# Multi-level relations

-   Employee -> Department -> Company <- Supplier

```json
PUT /company
{
  "mappings": {
    "properties": {
      "join_field": {
        "type": "join",
        "relations": {
          "company": [
            "department",
            "supplier"
          ],
          "department": "employee"
        }
      }
    }
  }
}

PUT /company/_doc/1
{
  "name": "My Company Inc.",
  "join_field": "company"
}



PUT /company/_doc/2?routing=1
{
  "name": "Development",
  "join_field": {
    "name": "department",
    "parent": 1
  }
}


PUT /company/_doc/3?routing=1
{
  "name": "Bo Andersen",
  "join_field": {
    "name": "employee",
    "parent": 2
  }
}
```
