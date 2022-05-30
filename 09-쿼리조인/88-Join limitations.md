# Join limitations

-   Document는 같은 인덱스에 저장되어야 한다. (성능 상의 이유)
-   Parent & child document는 같은 샤드에 있어야 한다
-   인덱스별로 1개의 조인 필드만
    -   1개의 조인 필드는 원하는 만큼 많은 관계를 맺을 수 있음
    -   새로운 relations는 인덱스를 만든 이후에 추가된다
-   Child relation는 존재하는 상위 항목에 추가될 수 있다
-   1개의 상위 document만
    (그러나 1개의 상위 document가 여러 개의 child를 가지는 것은 가능)
