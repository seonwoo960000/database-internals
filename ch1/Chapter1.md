# Chapter 1: Introduction and Overview 
## Introduction
- 데이터베이스 선정 시 고려할 사항
  - 사용하고자하는 쿼리를 지원하는가
  - 저장하고자하는 데이터의 양을 DB가 감당할 수 있는가
  - 단일 DB node에서 처리할 수 있는 읽기/쓰기 처리량
  - 시스템에서 필요한 DB node의 수
  - DB 확장을 어떻게 하는가
  - 유지보수 프로세스
- YCSB(Yahoo! Cloud Serving Benchmark)
  - 시중에 다양한 데이터베이스가 존재
    - 비슷한 유형의 데이터베이스 사이에서 어떤걸 선택해야하는지 ....
- YCSB project is to develop a framework and common set of workloads for evaluating the performance of different "key-value" and "cloud" serving stores
- 구성
  - YCSB client: Workload generator
  - Core workloads: Generator에 의해 실행될 workload scenario

## 데이터베이스 분류 
### OLTP vs OLAP 
- OLTP
  - 짧은 트랜잭션 대상 
- OLAP 
  - 복잡한 분석 작업 
  - 정형화된 쿼리가 아닌 경우 다수 존재

## 데이터베이스 아키텍처 
- Transport 
  - cluster communication 
  - client communication 
- Query processor 
  - query parser 
  - query optimizer 
- Execution engine 
  - remote execution 
  - local execution 
- Storage engine 
  - transaction manager: tx 관리 
  - lock manager: tx를 위해 database object locking 
  - access methods: disk의 데이터 조회 
  - buffer manager: memory에 page 캐싱 
  - recovery manager: failure에 대비한 recovery 관련

## Memory vs Disk Based DBMS 
- 구조와 접근 속도로 인해 효율적으로 사용하는 방법 자체가 다름 
  - memory based  
    - disk에 비해 빠름 
    - 메모리를 자유롭게 할당받아 사용 가능 
  - disk based 
    - disk에 위치한 데이터를 식별하고 참조할 수 있어야 함 
    - serialization / deserialization 고려 
    - fragmentation 관리 

### Disk Based DBMS Durability 
- Sequential log를 활용 
  - WAL (Write Ahead Log)
  - Back up copy: 비동기 + 배치 형태로 적용됨 
- WAL를 활용해서 데이터베이스 복구에 사용
  - checkpointing: 디스크에 성공적을 반영이 완료된 부분을 표시 
- 각 DBMS에서 WAL를 사용하는 방법에 대한 조사

## Column VS Row Oriented DBMS 
- Row oriented 
  - block 단위로 데이터를 조회
    - query에서 조회하고자 하는 데이터가 block 내부에 존재하는 경우 유용
    - 특정 컬럼만 조회하는 경우 효율적이지 않음
- Column oriented 
  - 동일 컬럼을 조회하는 쿼리에 유용 
    - 컬럼별 효율적인 압축 알고리즘을 사용할 수 있다는 장점 
    - CPU 효율도 상승함(vectorized instruction)

### Wide column store 
- Column oriented database와 다른 개념 
  - BigTable, HBase 
- Data는 다차원 map의 형태로 표시됨 

## Data Files & Index Files 
- Data file
  - Data를 저장 
  - 구현 방식 
    - IOT(Index organized table)
      - 데이터를 인덱스와 함께 저장하는 방식 
    - Heap organized table
      - 쓰기 순서대로 데이터가 저장됨 
      - 데이터가 저장된 위치를 가리키는 별도의 자료구조가 필요 
    - Hash organized table 
      - key의 해시값을 기준으로 value가 담길 bucket을 결정  
- Index file 
  - secondary kek 
    - primary key와 함꼐 저장되거나(MySQL)
    - heap file/IOT에서 해당되는 데이터의 offset을 가리킬 수 있음(PostgreSQL)
- * MySQL과 PostgreSQL의 index 구성 차이
