# Searching - Query

## Common Query Paramters

query parser에서 지원되는 쿼리 매개변수

- defType Parameter

solr가 요청하는 query parser에 대한 타입 설정 파라미터

> ex) defType=dismax <br>
> default query parser (lucene)

- sort Parameter

검색 결과를 오름차순(asc) 또는 내림차순(desc)으로 정렬하는 설정 파라미터
(대소문자 구분 없음, asc == ASC) 

- start Parameter

쿼리 결과에 대한 offset을 지정해서 offset 부터 결과를 보여주는 설정 파라미터

결과 문서에서 offset 값을 통해 연속된 검색 페이지를 조회할 수 있다.

- rows Parameter

문서 결과의 행 개수 설정 파라미터

Solr가 한번에 반환 해주는 결과 문서의 개수를 설정할 수 있다.

- fq(Filter Query) Parameter

score에 영향을 주지 않고 반환되는 문서의 결과에 대해 제한(필터링) 설정 파라미터

## The DisMax Query Parser

DisMax query parser는 사용자가 찾으려는 문법적으로 완전하지 못한 문장(쿼리)에 대해 각 필드의 가중치(boosts) 값을 기반으로 서로 다른 가중치 값을 사용해 여러 필드에서 개별 용어(term)를 검색하게 처리한다.
<br>
solrconfig.xml 의 requestHandler, Solr Query URL에서 옵션을 재정의하여 사용할 수 있다.


- 참고
  - https://solr.apache.org/guide/8_0/the-dismax-query-parser.html#q-parameter

### DisMax Query Parser Parameters





## The Extended DisMax(eDismax) Query Parser



