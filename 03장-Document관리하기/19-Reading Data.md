# How Elasticsearch reads data

Elasticsearch가 주 샤드에서 직접 문서를 검색했을 때 모든 검색어가 동일한 샤드에 있게 되는데 이러면 확장성이 좋지 않다.
대신 복제 그룹에서 샤드를 선택하고 Elasticsearch는 이를 위해 ARS(Adaptive Replica Selection)라는 기술을 아용한다.

(여긴 다시 들어야겠는데..)

-   Read 요청은 노드가 서로 협조함으로써 제어된다 (?)
-   라우팅은 Document의 복제 그룹을 풀어내는 데 사용된다
-   ARS는 쿼리를 사용가능한 샤드로 보내는 가장 좋은 방법이다
    -   ARS는 Adaptive Replica Selection의 줄임말
    -   ARS는 쿼리 반응 시간을 줄여줌
    -   ARS는 기본적으로 똑똑한 로드밸런서이다.
-   Coordinating 노드는 반응들을 모으고 클라이언트로 전송해 준다.
