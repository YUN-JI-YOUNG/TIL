### All Pair Partition 알고리즘   

#### 1. Theta 조인 알고리즘  

- 쎄타 조인   

  - 조인조건에 기본 비교연산자인 `<, >, <=, >=, =/=, =` 사용    

    - ex> `SELECT * FROM (Table 명) WHERE (조건)` 에서 조건에 비교연산자 사용   

      이퀴조인 = `SELECT * FROM R,S WHERE R.A = S.A` 등..    

- 위와 같은 조인 알고리즘들에 대해 맵리듀스를 할 때, 모든 가능한 페어를 여러 머신에 비슷한 수로 나누고, 각각 따져서 계산하면 분산처리가 가능해짐    

  => 이 때, 페어 분할하는 방법이 All Pair Partition 알고리즘       

#### 2. All Pair Partition 알고리즘       

- 테이블 R, S 에 대해, 모든 |R|*|S| 튜플 쌍 고려           
  - R를 u개의 파티션으로, S를 v 개의 파티션으로 분할   
    - |R|은 R에 들어있는 튜플 수, |S|는 S에 들어있는 튜플 수          
  - |R|*|S| 개의 튜플 쌍을 `u*v` 개의 disjoint한 파티션으로 분할   
  - 각각의 파티션을 1개의 reduce 함수로 처리     



- `ex.` 6개의 레코드를 가진 S , 4개의 레코드를 가진 R 에 대해서 S을 3개(v), R를 2개(u) 파티션으로 분할한다면  

  == `|S| : 6`  ,  `|R| : 4`  ,  `v : 3` , `u : 2`       

  |        | S1,S2        | S3,S4        | S5,S6        |
  | ------ | ------------ | ------------ | ------------ |
  | R1, R2 | 파티션 (1,1) | 파티션 (1,2) | 파티션 (1,3) |
  | R3, R4 | 파티션 (2,1) | 파티션 (2,2) | 파티션 (2,3) |

  

  - 파티션에 소속된 튜플의 수는 꼭 동일하지 않아도 됨     
    - 위와 동일하게 |S| = 6이고, v = 3일때, 
    - `[S1]` , `[S2,S3]` , `[S4,S5,S6]` 처럼 나누기도 가능      
  - Map 함수   
    - Key : 파티션 number  (1,1),(1,2),...
    - Value : (테이블명, 레코드 id, 값) => ("R", 1, a1)      
  - Shuffle 페이즈   
    - 동일한 key에 대해 value List 생성   
    - Key (1,1)  - Value List : [("S", 1 , a1),("S", 2, a1), ("R", 1, a1), ("R", 2, a1)] 등등    
  - Reduce 함수   
    - Value List 를 읽어서 R은 R끼리, S는 S끼리 정리   
      - Key (1,1) - Value List : R:[(1, a1), (2, a1)], S: [(1, a1), (2, a1)] 등등        
    - 조인 조건에 해당하면 [`R 레코드id`, `값`, `S 레코드id`, `값`] 순으로 출력       
      - 조건이 `=` 이면, [1, a1, 1, a1], [1, a1, 2, a1], [2, a1, 1, a1], [2, a1, 2, a1]  등...       



- 장점  
  - 어떤 조인 조건이라도 처리 가능    
  - 모든 reduce 함수에 들어가는 입력 사이즈 비슷   
- 단점   
  - 모든 튜플 쌍을 모두 검사해야함    
  - 각 reduce 함수의 출력 사이즈가 다를 가능성 존재       

<hr>   




### Self-join 을 위한 All Pair Partition 알고리즘   

- Self-join 은 1개의 입력 테이블에 들어있는 레코드들 간 조인 의미    

- 입력 테이블에 있는 레코드를 m 개의 파티션으로 나누기      

  - D1,D2, D3, ... , Dm        

- Map 함수   

  - Di에 있는 각각의 튜플 p에 대해 모든 파티션과 (Key, Value) 쌍 출력    

  - Key : (1, i), (2, i), (3, i), ... (i, i), (i, i+1), ... (i, m)   

    - `파티션id`는` i`로 고정이지만, 파티션 id가 작은것부터 써야해서 순서가 바뀔 순 있음      

  - Value : ("S", 1, p)    

    - (테이블명, 레코드 id, 값)          

    - 조인할 파티션 id를 Value에 같이 보내도 되고,        

      아니면 조인할 파티션 id를 정할 때, `레코드 id % 파티션 개수` 로 정하면 된다.      

      **조인할 파티션 id = 레코드 id를 파티션 수로 나눈 나머지**   

      => 이 방법을 사용하면 Reduce 함수에서도 해당 값이 어느 파티션에서 왔는지 알 수 있어서 조인 가능.       

      ​           

- Shuffle 페이즈   

  - 동일한 key에 대해 value List 생성   
  - (1,1)  - [ ("S", 1, a1) ,  ("S", 2, a1) ], ....

- Reduce 함수   

  - Key  = (x,y) 이고, x,y 는 파티션 id    

    Dx 파티션과 Dy 파티션 사이의 모든 쌍에 대해 조인        

    - Default : x <= y       

    - x = y  : 파티션 id가 같은 것이므로 Dx(=Dy) 파티션에 있는 모든 쌍의 조인    
    - x =/= y : Dx 파티션과 Dy 파티션 사이의 모든 쌍의 조인      

  - Key - valueList 출력    





#### All Pair Partition 코딩   

- AllPairPartition.template.java 파일을 AllPairPartition.java 파일로 복사하여 코드 작성        
  - 현재는 조인 조건은 없고, 그냥 셔플 페이즈해서 나온 Key-Value List를 출력하는 코드       

- `Driver.java` 파일에 등록 필요   
  - pgd.addClass("allpair", AllPairPartition.class, "A map/reduce program that partitions all pairs of tuples from both tables");       
- 이후 Project 디렉토리에서 `ant` 명령어로 컴파일      
- `hdfs dfs -mkdir allpair_test` 로 데이터를 위한 폴더 생성   
- 데이터 복사      
  - `hdfs dfs -put data/equijoin-R-data.txt allpair_test`    
  - `hdfs dfs -put data/equijoin-S-data.txt allpair_test` 

- `hadoop jar ssafy.jar allpair r s 2 allpair_test allpair_test_out` 로 실행     
  - 커맨드라인에 `테이블 이름`, `파티션 수` 추가 입력   

- `hdfs dfs -cat allpair_test_out/part-r-00000` 
- `hdfs dfs -cat allpair_test_out/part-r-00001`        



#### All Pair Partition Self 코딩   

###### All Pair Partition 코딩과 유사       

- AllPairPartitionSelf.template.java 파일을 AllPairPartitionSelf.java 파일로 복사하여 코드 작성      
  - 현재는 조인 조건은 없고, 그냥 셔플 페이즈해서 나온 Key-Value List를 출력하는 코드       
- `Driver.java` 파일에 등록 필요   
  - pgd.addClass("allpairself", AllPairPartitionSelf.class, "A map/reduce program that partitions all pairs of tuples from a table");       
- 이후 Project 디렉토리에서 `ant` 명령어로 컴파일      
- `hdfs dfs -mkdir allpairself_test` 로 데이터를 위한 폴더 생성   
- `hdfs dfs -put data/equijoin-S-data.txt allpairself_test`  로 데이터 복사   
- `hadoop jar ssafy.jar allpairself s 2 allpairself_test allpairself_test_out` 로 실행     
  - 커맨드라인에 `테이블 이름`, `파티션 수` 추가 입력   
- `hdfs dfs -cat allpairself_test_out/part-r-00000` 
- `hdfs dfs -cat allpairself_test_out/part-r-00001`     

<hr>


### Result     


```java
// allpair_test_out/part-r-00000   

(0,0)	
r	6	8	16
r	4	7	14
r	3	6	13
r	2	6	12
r	1	5	11
s	6	28	16
s	5	17	15
s	2	16	12
s	1	15	11
(0,1)	
s	4	17	14
s	3	16	13
r	6	8	16
r	4	7	14
r	3	6	13
r	2	6	12
r	1	5	11
(1,0)	
r	5	7	15
s	6	28	16
s	5	17	15
s	2	16	12
s	1	15	11
(1,1)	
s	4	17	14
s	3	16	13
r	5	7	15
```



- All Pair Partition Self join     
  - 파티션 id 는 무조건 작은 숫자부터 쓰기로 했으니 (1,0) 은 존재하지 않음    

```java
// allpairself_test_out/part-r-00000    
(0,0)	
s	6	28	16
s	4	17	14
s	2	16	12
(1,1)	
s	5	17	15
s	3	16	13
s	1	15	11
```

```java
// allpairself_test_out/part-r-00001   

(0,1)	
s	6	28	16
s	5	17	15
s	4	17	14
s	3	16	13
s	2	16	12
s	1	15	11
```



