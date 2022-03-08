## Top-K Closest Point 검색 알고리즘   

- Single Table에서 질의 포인트(쿼리 포인트)와 점들로 구성된 데이터 셋이 있을 때, 질의 포인트로부터 가장 가까운 K개 포인트 뽑기     

  - `ex`  식당 위치(Single Table) 데이터에서 현재 위치(질의)에서 가장 가까운 식당 K개를 뽑는다        

- Max-Heap 자료 구조 이용      

  - Binary tree 형태    
  - 어떤 노드에서 보든 부모는 해당 노드보다 크거나 같고, 자식은 해당 노드보다 작거나 같다.    
  - 루트 노드에 가장 큰 수가 들어감     

- Max-Heap을 이용하여 가장 작은 Top-K 찾는 알고리즘   

  - Max-Heap에는 현재까지 본 수중 가장 작은 k 개 숫자 유지   

  - k=8이라면, 먼저 8개의 숫자를 읽을 때까지는 무조건 Max-Heap 에 넣어서 Max-Heap 생성    

  - 데이터를 보다가 숫자가 Max-Heap의 루트 노드보다 작으면,    

    Max-Heap 에서 루트 노드에 들어있는 데이터 삭제하고 해당 숫자를 Max-Heap에 넣기      

    (루트 노드는 가장 큰 숫자가 들어가므로)     

    => 이후 재정렬      

  - 데이터를 보다가 숫자가 Max-Heap의 루트 노드보다 크면,        

    건너뛴다.        

  

#### 1페이즈   

  - Map 함수   

    - 각 점을 입력 받기   

    - 파티션을 m개로 나눌 때, 점의 파티션 id를 pid라고 한다면,    

      Key : pid (파티션 id)       

      Value : 입력 포인트 (`ex` p1 : (1,4) , ...)             

      로 하여 출력   

  - 셔플 페이즈      

    - 동일 key에 대하여 Value List 생성    

  - Reduce 함수   

    - 질의 포인트 (= 쿼리 포인트) 가 (2,2) 라면,    

      질의 포인트와 각 Value 의 좌표 사이의 거리를 계산하여 가장 가까운 K 개만 출력    

    - `ex` Value : p1 : (1,4) 이고, Query = (2,2) 라면,   

      Key : p1

      Value : (1,4), √5        

  - 각 파티션마다 Reduce 함수가 실행되므로, 파티션이 m개라면 총 K * m 만큼 출력됨    

    => 이 중 최소를 뽑기 위해 2페이즈 실행      


#### 2페이즈   

  - Map 함수   

    - 각 점 입력받기    

    - Key : *   (모든 Value를 한 번에 모아서 Reduce 함수를 1번만 실행하기 위한 목적)      

      Value : p1 (1,4), √5   (입력 포인트 그대로)        

      - Key는 * 가 아니어도 통일만 되면 뭐든 상관 없음     

  

  - 셔플 페이즈    

    - 모든 Value를 Value List로 합치기      

      => 모든 값의 Key 가 * 이므로 Key가 1개라서 가능       

  - Reduce 함수       

    - Value 값의 가장 마지막에 있는 거리 (√5 , √2 , ... ) 등을 비교하여 가장 작은 K개 출력   


</br>   
<hr>   

 
## Top-K Closest Point Search 코딩   

- TopKSearch.template.java 파일을 TopKSearch.java 파일로 복사하여 코드 작성          
  - Top-K Closest Point 검색 알고리즘                 

- `Driver.java` 파일에 등록 필요   
  - pgd.addClass("topksearch", TopKSearch.class, "A map/reduce program that performs the top-k search for a single input file");              
- 이후 Project 디렉토리에서 `ant` 명령어로 컴파일      
- `hdfs dfs -mkdir topksearch_test` 로 데이터를 위한 폴더 생성   
- `hdfs dfs -put data/topksearch-data.txt topksearch_test`  로 데이터 복사    
- `hadoop jar ssafy.jar topksearch 3 "1:1" 3 topksearch_test topksearch_test_out1 topksearch_test_out2` 로 실행      
  - 각 페이즈마다 출력 디렉토리 따로 설정        
  - 커맨드라인에 `파티션 수`, `질의 포인트(쿼리 포인트)` , `K(근접 이웃 수)`  추가 입력       
    - 2차원 포인트이므로 1:1 = (1,1) 이며, 만약 3차원 (1,2,3) 이라면 "1:2:3" 으로 작성    
- `hdfs dfs -cat topksearch_test_out2/part-r-00000`     
  - 마지막 Reduce 함수는 반드시 1번만 실행되어야 하므로 결과파일은 1개만 출력됨    
 
