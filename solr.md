# Solr 내용 정리


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

