# Solr 내용 정리

# Solr Administration User Interface

### Solr.xml

solr에서 collection 또는 core에 적용되는 옵션 파일이다.

```xml
<solr>

  <int name="maxBooleanClauses">${solr.max.booleanClauses:1024}</int>
  <str name="sharedLib">${solr.sharedLib:}</str>

  <solrcloud>
    <str name="host">${host:}</str>
    <int name="hostPort">${jetty.port:8983}</int>
    <str name="hostContext">${hostContext:solr}</str>
    <bool name="genericCoreNodeNames">${genericCoreNodeNames:true}</bool>
    <int name="zkClientTimeout">${zkClientTimeout:30000}</int>
    <int name="distribUpdateSoTimeout">${distribUpdateSoTimeout:600000}</int>
    <int name="distribUpdateConnTimeout">${distribUpdateConnTimeout:60000}</int>
    <str name="zkCredentialsProvider">${zkCredentialsProvider:org.apache.solr.common.cloud.DefaultZkCredentialsProvider}</str>
    <str name="zkACLProvider">${zkACLProvider:org.apache.solr.common.cloud.DefaultZkACLProvider}</str>
  </solrcloud>

  <shardHandlerFactory name="shardHandlerFactory"
    class="HttpShardHandlerFactory">
    <int name="socketTimeout">${socketTimeout:600000}</int>
    <int name="connTimeout">${connTimeout:60000}</int>
    <str name="shardsWhitelist">${solr.shardsWhitelist:}</str>
  </shardHandlerFactory>

</solr>
```

- solr element
  - coreRootDirectory : core 디렉터리가 생성되는 위치 설정 옵션
    - default : $SOLR_HOME (server/solr)

- solrcloud element
  - hostPort : solr의 jetty port 설정 옵션
    - default : 8983
  - zkClientTimeout : zookeeper 서버에 대한 연결 시 timeout 시간 설정 옵션
  - zkHost : solr cloud 모드에서 연결될 zookeeper 호스트의 url 정보들

참고: https://solr.apache.org/guide/8_5/format-of-solr-xml.html
 
# Document, Field and Schema Design

solr는 특정 정보들(문서)를 입력하는 과정을 통해 나중에 질문(쿼리)으로 정보를 검색하게 도와준다.

이때 solr에 정보들을 입력하는 과정을 **색인(indexing or update)** 이라고 한다.

### Field

Field는 solr에 색인할 문서가 어떻게 색인될지 정의하는 정보이다.

- field type 정의
  - 필드 유형 이름(필수)
  - 구현 클래스 이름(필수)
  - 필드 유형이 TextField인 경우 필드 유형에 대한 분석 필드
  - 필드 유형 속성(구현 클래스에 따라 일부 속성이 필수일 수 있다)

### Field Type Definitions in schema.xml

필드 타입은 schema.xml에 정의한다.

```xml
<fieldType name="text_general" class="solr.TextField" positionIncrementGap="100"> 
  <analyzer type="index"> 
    <tokenizer class="solr.StandardTokenizerFactory"/>
    <filter class="solr.StopFilterFactory" ignoreCase="true" words="stopwords.txt" />
    <!-- in this example, we will only use synonyms at query time
    <filter class="solr.SynonymFilterFactory" synonyms="index_synonyms.txt" ignoreCase="true" expand="false"/>
    -->
    <filter class="solr.LowerCaseFilterFactory"/>
  </analyzer>
  <analyzer type="query">
    <tokenizer class="solr.StandardTokenizerFactory"/>
    <filter class="solr.StopFilterFactory" ignoreCase="true" words="stopwords.txt" />
    <filter class="solr.SynonymFilterFactory" synonyms="synonyms.txt" ignoreCase="true" expand="true"/>
    <filter class="solr.LowerCaseFilterFactory"/>
  </analyzer>
</fieldType>
```

참고: https://solr.apache.org/guide/8_1/field-type-definitions-and-properties.html
