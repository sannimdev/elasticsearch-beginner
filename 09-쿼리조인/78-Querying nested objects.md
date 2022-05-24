```json
PUT /department
{
  "mappings": {
    "_doc": {
      "properties": {
        "name": {
          "type": "text"
        },
        "employees": {
          "type": "nested"
        }
      }
    }
  }
}
```

-   [7.x부터 인덱스에 여러개의 타입을 생성할 수 없기 때문에 위 요청에서 타입인 \_doc을 넣는 것은 무의미하기 때문에 발생하는 에러이다. 그래서 \_doc 부분을 제거한 후 스키마 매핑값을 요청한다면 정상적으로 작동하게 된다.](https://needneo.tistory.com/58)

```json
PUT /department
{
  "mappings": {
      "properties": {
        "name": {
          "type": "text"
        },
        "employees": {
          "type": "nested"
        }
    }
  }
}
```

-   아래의 값 삽입

```json
PUT /department/_doc/1
{
  "name": "Development",
  "employees": [
    {
      "name": "Eric Green",
      "age": 39,
      "gender": "M",
      "position": "Big Data Specialist"
    },
    {
      "name": "James Taylor",
      "age": 27,
      "gender": "M",
      "position": "Software Developer"
    },
    {
      "name": "Gary Jenkins",
      "age": 21,
      "gender": "M",
      "position": "Intern"
    },
    {
      "name": "Julie Powell",
      "age": 26,
      "gender": "F",
      "position": "Intern"
    },
    {
      "name": "Benjamin Smith",
      "age": 46,
      "gender": "M",
      "position": "Senior Software Engineer"
    }
  ]
}

PUT /department/_doc/2
{
  "name": "HR & Marketing",
  "employees": [
    {
      "name": "Patricia Lewis",
      "age": 42,
      "gender": "F",
      "position": "Senior Marketing Manager"
    },
    {
      "name": "Maria Anderson",
      "age": 56,
      "gender": "F",
      "position": "Head of HR"
    },
    {
      "name": "Margaret Harris",
      "age": 19,
      "gender": "F",
      "position": "Intern"
    },
    {
      "name": "Ryan Nelson",
      "age": 31,
      "gender": "M",
      "position": "Marketing Manager"
    },
    {
      "name": "Kathy Williams",
      "age": 49,
      "gender": "F",
      "position": "Senior Marketing Manager"
    },
    {
      "name": "Jacqueline Hill",
      "age": 28,
      "gender": "F",
      "position": "Junior Marketing Manager"
    },
    {
      "name": "Donald Morris",
      "age": 39,
      "gender": "M",
      "position": "SEO Specialist"
    },
    {
      "name": "Evelyn Henderson",
      "age": 24,
      "gender": "F",
      "position": "Intern"
    },
    {
      "name": "Earl Moore",
      "age": 21,
      "gender": "M",
      "position": "Junior SEO Specialist"
    },
    {
      "name": "Phillip Sanchez",
      "age": 35,
      "gender": "M",
      "position": "SEM Specialist"
    }
  ]
}
```

-   검색 쿼리 샘플 (각 개체 속성 간의 연결을 잃어버려서 실패)

```json
GET /department/_search
{
  "query": {
    "nested": {
      "path": "employees",
      "query": {
        "bool": {
          "must": [
            {
              "match": {
                "employees.position": "intern"
              }
            },
            {
              "term": {
                "employees.gender.keyword": {
                  "value": "F"
                }
              }
            }
          ]
        }
      }
    }
  }
}

```
