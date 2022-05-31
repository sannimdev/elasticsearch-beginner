# Join field performance considerations

## 강의 요약

-   가능한 한 조인 필드는 삼가기 (몇 가지 예외사항 제외)
-   조인 필드는 느리다.
    -   더 많은 하위 document가 유일한 부모를 가리키고 있다면, has_child 쿼리는 기본적으로 더 많은 document이다.
    -   문제가 발생하기 전까지 알아차리지 못할 가능성
-   많은 parent document는 `has_parent` 쿼리 떄문에 느려진다
-   몇 가지 경우에는 조인 필드의 성능이 나빠지지는 않는다
    -   2개의 document의 타입에서 많은 관계를 맺어야 할 때
        -   e.g. 레시피 (parent document), 재료 (child documents)

## Q&A

1. 조인 필드를 사용하는 것이 좋지 않은 방법이라면, 왜 여기에 시간을 쏟았어?
    - 몇 가지 상황에서는 조인 필드를 사용하는 것이 좋다
    - 만약에 많은 document를 가지지 않은 경우이거나 혹은 앞으로도 그럴 것 같은 경우에 성능이 괜찮을 거야
    - 얼마나 자주 사용하는지의 여부에 관계없이 조인 필드는 Elasticsearch의 한 부분이다.
    - 어쩌면 이것이 옳다고 결정을 내려야 하는 특별한 작업이 있을 수도 있다.
2. 조인 필드는 좋지 않은데 어떻게 우리는 document의 관계를 매핑했어?
    - nested data type
    - Elasticsearch는 관계형 DB와는 다르다.
        - e.g. 검색을 빠르게 하기 위하여 데이터를 최적화하여 저장하는 것
    - employees와 이에 대한 주소를 매핑하는 것을 생각해보자
        - document 되는 주소 대신 중첩된 객체나 또는 일반 필드로 만들자
