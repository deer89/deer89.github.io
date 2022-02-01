---
title: "'PLAN_TABLE' is old version"
description: "'PLAN_TABLE' is old version"
excerpt: "PLAN_TABLE' is old version"
categories: SQLexp
tag : [오라클, plantable]
author_profile : true 
toc_sticky: true
toc: true
toc_sticky: true
---

## 1. PLAN_TABLE' is old version
- EXPLAIN PLAN FOR에 대해서 후배에게 설명을 해주고 실습 문제를 내주었다. 
- 하지만, 실행계획은 나오지만 Predicate Information이 나오는 부분에 아래와 같은 오류가 나왔다. 
-  실행계획에 단계별 소요시간도 확인할 수가 없었다.
 ___PLAN_TABLE' is old version___ 

```
EXPLAIN PLAN FOR SELECT * FROM TB_6M;  

 SELECT * FROM TABLE(DBMS_XPLAN.DISPLAY(NULL, NULL, 'ADVANCED'));  
 
 
---------------------------------------------------------------------
| Id  | Operation         | Name       | Rows  | Bytes | Cost (%CPU)|
---------------------------------------------------------------------
|   0 | SELECT STATEMENT  |            |   303K|    90M|  3870   (1)|
|   1 |  TABLE ACCESS FULL| TB_SL6M    |   303K|    90M|  3870   (1)|
---------------------------------------------------------------------
Note
-----
   - 'PLAN_TABLE' is old version
``` 
## 2. 원인  
 - plan_table의 버전이 너무 오래되어서 나오는 오류라고 한다.
 - 사용자가 plan_table을 생성할 필요 없다고한다 (10g이후). 
 - SYS 스키마에 모든 유저가 사용할 수 있는 임시 테이블 PLAN_TABLE$과 PUBLIC SYNONYM PLAN_TABLE 이 존재
 - 사용자 스키마에 plan_table이 존재 할 경우 동의어보다 테이블을 우선 탐색하기 때문에 위와 같은 오류를 발생한다

## 3. 해결방법
 - 아래와 같이 본인스키마 밑에 plan table을 새로 생성해주었다.
   ```
 -- Table 생성
create table 본인스키마.PLAN_TABLE (
        statement_id       varchar2(30),
        plan_id            number,
        timestamp          date,
        remarks            varchar2(4000),
        operation          varchar2(30),
        options            varchar2(255),
        object_node        varchar2(128),
        object_owner       varchar2(30),
        object_name        varchar2(30),
        object_alias       varchar2(65),
        object_instance    numeric,
        object_type        varchar2(30),
        optimizer          varchar2(255),
        search_columns     number,
        id                 numeric,
        parent_id          numeric,
        depth              numeric,
        position           numeric,
        cost               numeric,
        cardinality        numeric,
        bytes              numeric,
        other_tag          varchar2(255),
        partition_start    varchar2(255),
        partition_stop     varchar2(255),
        partition_id       numeric,
        other              long,
        distribution       varchar2(30),
        cpu_cost           numeric,
        io_cost            numeric,
        temp_space         numeric,
        access_predicates  varchar2(4000),
        filter_predicates  varchar2(4000),
        projection         varchar2(4000),
        time               numeric,
        qblock_name        varchar2(30),
        other_xml          clob
);

--Table 삭제(본인 계정에 Plan Table이 있으며 정상적으로 Plan이 나오지 않는 경우)
drop table 본인계정.PLAN_TABLE 
;
```

## 결과
- 결과는 잘 나왔다.(수정 후 반영된것을 캡처를 못했다.)