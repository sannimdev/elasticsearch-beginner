# Understanding relevance scores

1. GET /product/default/\_search?q=pizza
2. 쿼리와 일치하는 documents 찾기
3. 얼마나 document가 일치할까?
    1. Pizza Hawaii
    2. Pizza Vegetariana
    3. Pizza Margherita

## 관련성

포함하는 것에만 관심이 있는 것이 아니기 때문에 관련성을 기반으로 검색할 수 있어야 한다.
일반적으로 RDBMS와 같은 데이터베이스 시스템과 대조된다.

## 작동 방식

-   기존에는 `TF IDF` 알고리즘으로 작동
-   요즘에는 `Okapi BPM` 25라는 알고리즘이 사용된다.

1. 빈도

-   검색하는 필드에 얼마나 검색어가 많이 나타나는지 확인
-   ex) sallyed라는 검색어 필드가 있다고 가정할 때 한 번만 나타난 경우가 한 번도 나타나지 않은 경우보다 관련성이 더 높다는 것을 의미

2. inverse document 빈도 (역 document 빈도)

-   인덱스에서 검색어가 나타나는 빈도
-   샐러드라는 용어가 인덱스에 몇 번이나 나타나는지 들 수 있음
-   자주 나타날수록 점수와 관련성이 낮아진다.

3. Field-length norm

-   필드 길이 Norm
-   필드가 길수록 필드 내의 단어가 관련될 가능성이 낮아진다
-   50자 제목의 샐러드라는 검색어는 5000자 내에 포함된 샐러드라는 검색어보다 중요

### BM25 알고리즘과의 비교

-   stop words를 좀 더 잘 처리한다.
-   the field-length norm 요소를 개선
-   파라미터가 설정될 수 있음
