# Stemming & stop words

Text 분석으로 다시 돌아가 보자
2가지 중요한 개념이 있었다.

1. Stemming
2. stop words

"I `loved` `drinking` `bottles` of wine on last `year's` vacation."

-   loved: 과거형 동사 (Past tense)
-   drinking: 동명사 (Gerund)
-   bottles: 복수형 (Plural of "bottle")
-   year's: 소유격 (Possesion)

위의 단어들은 어근(the root form) 아니라 무언가 문법적으로 가공된 형태이다.

## Stemming(형태소 분석) 소개

-   형태소 분석은 단어를 어근(the root form) 형태로 줄인다.
    -   loved -> love
    -   drinking -> drink
-   "I love drink bottl of wine on last year vacat."
    -   형태소 분석이 얼마나 공격적으로 구성되었는지에 따라 모든 단어가 유효한 단어로 파생되는 것은 아니다
    -   형태소 분석은 단지 Elasticsearch가 내부적으로 사용하는 것이며 아무도 실제로 원형으로 변환된 예문을 볼 수 없음

## stop words 소개

-   단어는 text 분석을 하는 동안 걸러진다.
    -   일반적으로 "a", "the", "at", "of", "on" 등이 있음
-   이것들은 관련성 점수에 거의 또는 전혀 가치를 제공하지 않음
-   이런 단어를 제거하는 것이 꽤 일반적이다
    -   오늘날 Elasticsearch에서는 과거보다는 흔하지는 않다.
        -   이러한 관련성 알고리즘은 상당히 개선되었음.
-   기본적으로 제거되지 않으며 일반적으로 그렇게 하지 않는 것이 좋다.

"I loved drinking wine last year's vacation."

-   of, on이 제거되었다.
