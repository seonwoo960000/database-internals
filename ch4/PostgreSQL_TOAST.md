# TOAST
- PostgreSQL 
  - 고정 크기 page(8KB)를 사용  
  - tuple을 저장할 때 multiple page에 걸쳐 저장되지 못하도록 구성
    - 크기가 큰 tuple을 저장할 수 없음 
    - 위와 같은 한계를 극복하기 위해 크기가 큰 tuple은 압축 이후 다수의 row로 분할됨 
- TOAST를 지원할 수 있는 data type 
  - 모든 data type이 TOAST를 지원할 필요는 없음(integer, long 등 굳이?)
  - TOAST를 지원하는 data type의 구조  
    - first 4 bytes: 해당 데이터의 길이 정보를 저장 
    - 그 이외의 구조는 자유로움   
