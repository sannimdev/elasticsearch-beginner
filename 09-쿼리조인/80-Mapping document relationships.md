# Mapping documents relationship

-   department, employee 2개의 관계 설정

```json
PUT /department
{
  "mappings": {
    "properties": {
      "join_field": {
        "type": "join",
        "relations": {
          "department": "employee"
        }
      }
    }
  }
}
```
