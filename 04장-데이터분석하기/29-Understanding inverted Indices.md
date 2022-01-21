# Understanding inverted Inidices

-   필드 값은 각각 데이터 구조에 저장된다
    -   데이터 구조는 필드 데이터 타입에 의존한다
    -   효율적인 데이터 접근을 보장한다 (e.g. 검색)
-   실제적인 데이터 구조는 Apache Lucene에 의해 제어된다. (elasticsearch의 영역이 아닌 듯)
-   이번 강좌에서는 역색인에 초점을 맞춘다.
-   검색을 빠르게 할 수 있다
-   역색인은 다른 데이터도 포함할 수 있음
    -   ex) 점수를 매기는 것과 연관된 정보
    -   Elasticsearch (기술적으로는 Apache Lucnene)은 다른 데이터 구조를 사용한다
        -   Numeric values, dates, geospatial data 구조에 관해서는 BKD tree를 사용함

## Inverted Indices

-   단어(terms)와 document가 포함하는 것들 사이를 매핑한다.
-   분석기의 문맥 밖에서 용어학적 "단어"를 사용
-   단어는 가나다순 정렬
-   역색인은 단지 하나의 단어와 document ID정보보다 더 많은 것을 포함한다.
    -   예) 점수를 매기는 것과 연관된 정보
        주어진 단어가 포함된 문서를 되찾고 싶을 뿐만 아니라, 그것들이 얼마나 잘 일치하는지에 따라 순위가 매겨지기를 원한다.
-   텍스트 필드 당 하나의 반전된 인덱스
-   다른 데이터 타입은 BKD trees 등을 사용한다.
    -   다른 데이터 타입과 공존하는 필드는 다른 데이터 구조를 사용하고 있기 때문이다.
    -   numeric, date, geospatial 유형이 이에 해당한다.
    -   이러한 데이터 구조를 사용하는 것이 가장 효율적이기 때문
-   Inverted Indices는 Elasticsearch가 빌드하는 Apache Lucene에 의해 만들어진다.

예시1)

-   DOCUMENT#1
    -   "2 guys walk into a bar, but the third... DUCKS! :-)"
    -   -> ["2", "guys", "walk", "into", "a", "bar", "but", "the", "third", "ducks"]
-   DOCUMENT#2
    -   "2 guys went into a bar"
    -   -> ["2", "guys", "went", "into", "a", "bar"]
-   DOCUMENT#3
    -   "2 ducks walk around the lake"
    -   -> ["2" "ducks", "walk", "around", "the", "lake"]

```
TERM    DOCUMENT#1  DOCUMENT#2   DOCUMENT#3
2           X           X            X
a           X           X
around                               X
bar         X           X
but         X
================================================
ducks       X                        X
================================================
guys        X           X
into        X           X
the         X                        X
third       X
walk        X                        X
went                    X
```

예시2)

-   Inverted index는 실제로 text field마다 만들어지며 다음의 예시와 같다.
    -   name field
    -   description field

```json
DOCUMENT#1
{
    "name": "Coffee Maker",
    "description": "Makes coffee super fast!",
    "price": 64,
    "in_stock": 10,
    "created_at": "2009-11-08T14:21:51Z"
}
DOCUMENT#2
{
    "name": "Toaster",
    "description": "Makes delicious toasts...",
    "price": 49,
    "in_stock": 4,
    "created_at": "2007-01-29T09:44:15Z"
}
```

Name field

```
TERM        DOCUMENT#1        DOCUMENT#2
coffee          X
maker           X
toaster                            X
```

Description field

Q

> Description field inverted index mapping is mapped to wrong document
> like coffee word is present in document 1 but it is marked against document 2
> Video timing 4.44

A

> Looks like I swapped the two documents around in the diagram. I will correct that as soon as possible. Sorry about that, and thank you for letting me know!

```
TERM        DOCUMENT#1      DOCUMENT#2
coffee          X
delicious                       X
fast            X
makes           X               X
super           X
toasts                          X
```
