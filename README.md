# whoareyou

### (논문)인접 조건 검사에 의한 초고속 한국어 형태소 분석
http://cs.sungshin.ac.kr/~shim/download/04_01.pdf

### Mach
http://cs.sungshin.ac.kr/~shim/demo/mach.html

### 은전한닢 프로젝트
http://eunjeon.blogspot.kr/

### mecab-ko-lucene-analyzer
https://bitbucket.org/eunjeon/mecab-ko-lucene-analyzer/src/2b345772535f081a3445da97d8738d03ed011ca0/?at=release-0.21.0

# Solr

### Solr
http://lucene.apache.org/solr/

### QuickStart
http://lucene.apache.org/solr/quickstart.html

### PDF
http://apache.tt.co.kr/lucene/solr/ref-guide/apache-solr-ref-guide-6.5.pdf

### 코어 (Collection) 생성
bin/solr create -c core_name

# Nutch

https://wiki.apache.org/nutch/NutchTutorial

# [MAC] Solr 6.5.1 - Nutch 1.13  연동 주의사항

### bash_profile 설정
export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home

### integrate Nutch with Solr
http://stackoverflow.com/questions/38525848/solr-6-and-nutch-2-3-1-integration?answertab=active#tab-top
 
### $bin/nutch index... 시, indexWriters가 올바르게 activated 되지 않을 경우
EX)SolrIndexWriters를 타지 않고 ElasticIndexWriters를 탄다.
> nutch설치경로/conf/nutch-site.xml 에서
![Alt text](https://github.com/khjoon0204/whoareyou/blob/master/nutchsite.png)
'indexer-solr'가 포함되어있는지 확인한다.
http://stackoverflow.com/questions/17649567/nutch-message-no-indexwriters-activated-while-loading-to-solr

### Indexer: java.io.IOException: Job failed!
> $bin/nutch index -Dsolr.server.url=http://localhost:8983/solr/mecabnutch crawl/crawldb/ -linkdb crawl/linkdb -dir crawl/segments/ -filter -normalize -deleteGone

### Solr Query Error: "undefined field _text_"
solr설치경로/server/solr/core이름/conf/solrconfig.xml에서

```

  <initParams path="/update/**,/query,/select,/tvrh,/elevate,/spell,/browse">
    <lst name="defaults">
      <str name="df">text</str>
    </lst>
  </initParams>
  
  ```

"_text_"를 managed-schema에 있는 ``` <defaultSearchField>text</defaultSearchField> ``` 와 맞춘다
"df" 는 default를 의미한다
http://stackoverflow.com/questions/10130163/solr-query-http-error-404-undefined-field-text/10130462#10130462http://stackoverflow.com/questions/10130163/solr-query-http-error-404-undefined-field-text/10130462#10130462

### Nutch ERROR lock file locked already exists. 
> Remove the lock files before generating a fetch list. 
> crawldb/.locked 
> crawldb/..locked.crc 

# [MAC] MeCab 설치

1. mecab-java-0.996에서 Makefile 편집.
> ...INCLUDE=/System/Library/Frameworks/JavaVM.framework/Headers...

2. 설치후,

> sudo mv /usr/local/lib/libMeCab.so /usr/local/lib/libMeCab.dylib

![Alt text](https://github.com/khjoon0204/whoareyou/blob/master/done1.png)

# 실행명령어
* bin/solr restart -noprompt -Djava.library.path=/usr/local/lib
* bin/solr stop -all
* bin/crawl -i -D solr.server.url=http://localhost:8983/solr/[core명] urls/ [crawl폴더명] 2
  > bin/crawl -i -D solr.server.url=http://localhost:8983/solr/mecabnutch urls/ news 2
* bin/nutch solrindex http://localhost:8983/solr/[core명] [crawl폴더명]/crawldb/ -linkdb [crawl폴더명]/linkdb -dir [crawl폴더명]/segments/
  > bin/nutch solrindex http://localhost:8983/solr/mecabnutch crawl/crawldb/ -linkdb crawl/linkdb -dir crawl/segments/

# Example 1

Input:

문재인 대통령은 15일(오늘) 미세먼지를 줄이기 위한 긴급 대책으로 30년 이상 노후 석탄화력발전소에 대한 ‘일시 가동 중단(셧다운)’을 지시했다.

문 대통령은 이날 오후 서울 양천구의 한 초등학교에서 열린 미세먼지 발생시 대처 방법 교육을 참관하는 자리에서 이같은 미세먼지 대책을 발표했다.

구체적으로는 우선 30년 이상 노후 석탄화력발전소를 대상으로 6월 한 달간 일시적으로 가동을 중단하고, 내년부터 3~6월 4개월간 노후 석탄화력발전소 가동 중단을 정례화하기로 했다. 

노후 발전소 10기는 임기 내 모두 폐쇄하고, 폐쇄 시기도 최대한 앞당길 방침이다.

이와 함께 미세먼지 문제를 국가적 의제로 설정하고, 조속한 시일 안에 대통령이 직접 책이는 미세먼지 대책기구를 설치할 것으로 김수현 신임 사회수석에게 지시했다. 

오늘 미세먼지 대책 발표 현장에는 이준식 교육부장관과 조경부 환경부 장관이 참석해 관련 업무 지시를 받았고, 박원순 서울시장과 조희연 서울시교육감, 임종석 대통령 비서실장 등이 배석했다.

문재인 대통령이 30년이 넘은 노후석탄화력발전소에 대해 올 6월 한달간 ‘일시적 가동중단’(셧다운)을 하라고 15일 지시했다. 대선 기간동안 공들였던 ‘미세먼지 저감 대책’에 시동이 걸린 셈이다.

미세먼지 저감대책은 대선기간 운영됐던 정책소개 사이트인 ‘문재인 1번가’의 ‘베스트 상품’이기도 했다.

문 대통령은 취임 6일째인 이날 서울 양천구 은정초등학교 ‘미세먼지 바로알기’ 교실을 방문해 고농도 미세먼지 발생시 생활 속 대처방법 교육을 참관하고 ‘노후석탄화력발전소 봄철 셧다운’ 등 미세먼지 대책을 발표했다.

문 대통령은 이날 “30년 이상 석탄화력발전소를 올 6월 일시적으로 가동 중단하고 내년부터는 봄철(3~6월) 가동중단을 정례화하겠다”고 밝혔다. 현재 노후 석탄화력발전소는 8기이며 당장은 ‘봄철 셧다운’에 돌입하지만 임기 내에는 모두 폐쇄할 계획이다. 애초 박근혜 정부는 노후석탄화력발전소를 2025년까지 폐쇄하겠다고 했으나 문 대통령은 봄철의 셧다운을 실시하면서 폐쇄 시기도 임기 내로 앞당기겠다고 한 것이다.

문 대통령은 또 김수현 사회수석에게 조속한 시일 내에 ‘대통령이 직접 챙기는 미세먼지 대책기구’를 설치할 것을 별도로 지시했다. 


연금저축보험 인터넷에 집중하삼 

청와대는 “이번 석탄화력발전소 일시 중단 업무지시는 미세먼지 문제를 국가적 의제로 설정하고 근본적 해결방안을 마련하겠다는 대통령의 강력한 의지가 담긴 것”이라고 설명했다. 

이날 문 대통령의 지시사항은 모두 후보 시절 발표한 미세먼지 정책공약의 일부다.

앞서 문 대통령은 대선 기간이었던 지난 4월 “정책역량과 외교역량을 모두 투입해 푸른 대한민국을 만들겠다”면서 에너지 정책의 패러다임 변화를 담은 정책공약을 발표한 바 있다. 

문 대통령의 미세먼지 저감을 위한 에너지 정책공약은 석탄화력발전을 대폭 감축하고 대신 천연가스(LNG) 발전으로 대체하겠다는 것이 골자였다. 지난 4월 문 대통령 측은 노후석탄화력발전소의 봄철 셧다운·조기폐쇄와 함께 봄철의 석탄화력발전 가동률 30% 축소(67%→40%), 건설 중인 화력발전소 중 공정률 10% 미만인 발전소의 원점 재검토 등을 제시했다. 그중에서 노후석탄화력발전소의 봄철 셧다운부터 이행에 들어가는 셈이다.

석탄화력발전의 축소는 에너지 패러다임 전환의 신호탄이 될 것으로 보인다.

그동안 석탄화력발전이 전체 전력비중의 40%를 차지했던 것은 석탄화력발전의 단가가 미세먼지가 덜 배출되는 LNG발전 단가보다 낮았기 때문이다. 그러나 문 대통령 측은 대선기간 비용만을 따지지 않고 국민의 환경권·안전·생명 가치를 중심으로 에너지 정책을 펼쳐가겠다고 밝혀왔다. 문 대통령의 공약집에도 “에너지 정책 패러다임을 전환하고 국민의 환경권을 지키겠다” “에너지기본계획·전력수급기본계획 등은 경제성 뿐 아니라 환경 및 안전 등을 반영 수립하겠다” “환경과 에너지를 통합 관리하겠다” 등의 약속이 명시돼 있다. 


문 대통령이 대선 기간 약속한 ‘국내 미세먼지 배출량 30% 저감’ 방안이 어떻게 현실화될지도 관심이다. ‘미세먼지 저감 공약’을 발표할 당시 문 대통령 측은 석탄화력발전 감축과 함께 2030년까지의 개인 경유차 퇴출, 사업장의 미세먼지 배출기준·부과금 강화책 등으로 국내 미세먼지 배출량의 30%는 줄일 수 있다고 설명한 바 있다.


[아시아경제 이민찬 기자] 문재인 대통령은 15일 미세먼지 감축 응급대책으로 30년 이상 된 노후 석탄화력발전소 10기의 '일시 가동 중단(셧다운)'을 지시했다고 청와대가 밝혔다. 문 대통령은 2025년까지 계획된 이들 노후 석탄화력발전소 10기의 폐쇄시기를 앞당겨 임기 내(2022년)에 모두 폐쇄한다는 방침이다. 

문 대통령은 이날 오후 2시 서울 양천구 은정초등학교 '미세먼지 바로알기 교실'을 방문해 고농도 미세먼지 발생 시 생활 속 대처방법 교육을 참관했다. 이 자리에서 학생과 학부모들로부터 생활 속 미세먼지 문제의 심각성에 대한 의견을 듣고 이 같은 내용의 미세먼지 대책을 발표했다. 

청와대 관계자는 "우선 30년 이상 노후 석탄화력발전소를 대상으로 6월 한 달간 일시적으로 가동을 중단하고, 내년부터 3~6월 4개월 간 노후 석탄화력발전소 가동 중단을 정례화할 것"이라며 "노후 발전소 10기는 임기 내 모두 폐쇄하고 폐쇄 시기도 최대한 앞당길 방침"이라고 설명했다.

현재 운영 중인 석탄화력발전소는 총 59기이며, 이 중 30년 이상 된 노후 석탄발전소는 3개 발전 공기업이 보유한 총 10기다. 산업통상자원부에 따르면 전체 석탄화력발전소에서 노후 설비의 용량 비중은 10.6% 수준이지만, 오염물질(SOx, NOx, 먼지) 배출량 비중은 19.4%로 상당량을 차지하는 것으로 조사됐다.

문 대통령은 이를 위해 조속한 시일 내에 미세먼지대책기구 설치를 사회수석에게 별도 지시했다고 청와대는 전했다. 앞서 문 대통령은 대선 후보 시절 '내 삶을 바꾸는 정권교체' 여섯 번째 공약으로 건설 중인 화력발전소 중 공정률 10% 미만 원점 재검토 등을 발표한 바 있다. 

한편 '찾아가는 대통령' 두 번째 시리즈로 진행된 이날 일정에는 이준식 사회부총리 겸 교육부 장관, 조경규 환경부 장관이 참석해 문 대통령의 업무지시를 받았다. 서울시에서는 박원순 서울시장과 조희연 서울교육감이 참석했다.  



Output(NNG only):

![Alt text](https://github.com/khjoon0204/whoareyou/blob/master/ex1_output.png)

