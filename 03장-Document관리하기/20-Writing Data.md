# How Elasticsearch writes data

-   쓰기 연산은 주 샤드에 전송된다.
-   주 샤드는 복제 샤드에 해당 연산을 전달한다.
-   Primary term(?)과 시퀀스는 실패 상황에서 복구하기 위해 사용된다
-   Global 체크포인트와 local 체크포인트는 복구하는 절차를 좀 더 빠르게 한다

## Primary Terms

-   기존 primary shards와 새로운 primary shards 사이를 구별하는 방법
-   얼마나 많이 primary shard가 변했는지를 세는 것
-   쓰기 연산에 추가됨

## Sequence Numbers

-   Primary term과 함께 쓰기 작업에 추가됨
-   기본적으로 쓰기 연산에 대해 증가하는 카운터
-   Primary shards는 sequence number를 증가시킴
-   Elasticsearch가 쓰기 연산을 수행할 수 있도록 한다

## Primary 샤드가 실패한 경우 복구하기

-   Primary terms와 시퀀스는 Elasticsearch를 회복하는 데 중요한 실마리다
    -   ex) 네트워크 오류
    -   디스크를 비교하는 방법 대신에 Primary terms와 시퀀스로 어디까지 이미 실행되었고 업데이트가 필요한지 알 수 있다.
-   만약에 거대한 인덱스가 있는 상황에서는 수백만 개의 연산을 비교하는 것이 현실적이지 않다.
    -   그래서 속도를 개선하기 위해 global과 local 체크포인트를 추가한다.

## Global and Local Checkpoints

-   기본적으로 시퀀스이다
-   각각 복제 그룹은 global 체크포인트를 가진다.
-   Global Checkpoints
    -   각각 복제 샤드는 local 체크포인트를 가진다.
        -   복제 그룹에서 활성회된 모든 시퀀스는 적어도 다음 크기까지 적용되었다.
-   Local Checkpoints
    -   마지막으로 미리 설정된 쓰기 연산의 시퀀스이다.
    -
