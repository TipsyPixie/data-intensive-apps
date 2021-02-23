## 데이터 모델과 질의 언어

### 데이터 모델

* 관계형 모델
    * 각 레코드(혹은 튜플이나 로우) 의 모음을 테이블(또는 관계)로 표현
    * 레코드간의 관계는 테이블과 외래 키로 논리적으로만 표현
    * RDBMS 는 그 자체가 관계형 모델은 아님에 주의. 관계형 모델에서 시작했다고 봐야 할 듯
    * 객체 관계형 불일치 문제(Impedance Mismatch)
        * 객체형 데이터를 관계형으로 바꿔야만 쓸 수 있어...
    * 팁: MySQL 은 `ALTER TABLE` 시에 전체 테이블을 복사해서 느리답니다. 지금도 그래? 
    
* 문서형 모델
    * 데이터를 문서형으로 모아서 표현
    * 문서의 스키마는 고정되지 않고 속도는 관계형보다 빠름
    * 다대일, 다대다 관계 문제
        * 다대일 조인은 문서형에서 쉽지 않아...
    
* 네트워크 모델(CODASYL 모델)
    * **Co**nference on **Da**ta **Sy**stems **L**anguages
    * 계층 모델에서 발전한 형태인 듯 하다?
    * 레코드들을 directed graph 로 표현. 여러 개의 부모, 여러 개의 자식을 가질 수 있고 다대다에 적합
    * 레코드간 관계는 물리적인 reference
    * 질의와 갱신의 복잡성 문제
        * 데이터 넣을 때 관계도 신경써야 하냐...
        * a→c 가 a→b→c 로 바뀌면 코드도 바꿀 꺼냐...

* 그래프 모델
    * 정점*vertex* 와 간선*edge* 로 구성
    * 아무 정점간에나 연결 가능
    * 이거 네트워크 모델 아냐?
        * CODASYL 에는 연결할 수 있는 레코드에 제한이 있지만 그래프는 아무거나 서로 연결 가능
        * CODASYL 에서는 항상 루트부터 접근 경로를 통해야 하지만 그래프틑 임의 점점에 아이디로 접근 가능
        * CODASYL 에서 child 레코드엔 순서가 있지만 그래프에는 접점과 간선에 순서 없음
        * CODASYL 에 없는 선언형 쿼리 언어가 그래프에는 있음
    * 속성 그래프 모델(Neo4j 등에서 사용)
        * 아이디, 들어오는 간선, 나가는 간선, 키-값 모음 으로 구성
    * 트리플 저장소 모델*Triplestore Model*
        * [시맨틱 웹]("https://en.wikipedia.org/wiki/Semantic_Web") 을 위해 만들어짐
        * subject-predicate-object 세 부분으로 데이터 저장
        * SPARQL 쿼리 예시
            ```sparksql
            SELECT ?name 
                   ?email
            WHERE
              {
              ?person  a          foaf:Person .
              ?person  foaf:name  ?name .
              ?person  foaf:mbox  ?email .
              }
            ```

### 질의 언어

* *Imperative*, *Declarative*
    * SQL 처럼 원하는 결과를 선언할테냐*Declarative*, 아니면 그 과정을 지시할테냐*Imperative*?
    * MapReduce 프로그래밍 모델은 이 중간 어디쯤에 있다. Hadoop 에서 열심히 쓰는 그 방식. 작성하기가 조금 까다로운 점은 아쉽다...
    