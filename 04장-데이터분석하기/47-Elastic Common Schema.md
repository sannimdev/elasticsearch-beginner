# Elastic Common Schema 소개

## 정의

-   줄여서 ECS라고 부른다
-   공통 필드의 사양 및 매핑 방법을 정의한 것
-   ECS 이전에는 필드 이름 간에 응집력(cohesion)이 없었음
-   nginx에서 로그를 수집하면 Apache와 다른 필드 이름이 제공된다
    -   apache2.access.url
    -   nginx.access.url
    -   같은 데이터의 유형(@timestamp)을 수집 (??)
-   ECS는 공통 필드의 이름이 동일함을 의미한다
    -   @timestamp
-   Use-case에 독립적
-   필드 그룹을 필드 세트라고 한다.

## ECS 사용하기

-   ECS에서 문서를 이벤트라고 한다.
    -   ECS는 이벤트가 아닌 것에 대한 필드를 제공하지 않는다
-   가장 유용한 표준 이벤트
    -   웹 서버 로그, OS 메트릭스 등
-   ECS는 자동으로 Elastic Stack 프로덕트를 제어한다.
    -   이것을 사용한다면 적극적으로 ECS를 다룰 필요는 없다.
-   필요하지 않을 수 있지만, ECS가 무엇인지 아는 것은 좋다.
