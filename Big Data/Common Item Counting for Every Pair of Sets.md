## Common Item Counting for Every Pair of Sets 알고리즘      

#### Self Join        

- Set 데이터에 있는 모든 튜플 간 중복된 item 개수 카운트     

- 장점   

  -  All pair partition 알고리즘은 모든 쌍을 다 봐야하는데, Inverted Index를 사용하면 하나의 item이라도 중복되는 쌍만 살펴보아 더 빠름         

- 알고리즘    

  - Inverted Index 만들기  

    | 레코드 id | item      |
    | --------- | --------- |
    | 1         | c d f     |
    | 2         | a b e f g |
    | 3         | a b c d e |
    | 4         | b c d e f |
    | 5         | a e g     |

    로 Inverted Index 를 만든다면,        

    | 레코드 id | item       |
    | --------- | ---------- |
    | a         | 2, 3, 5    |
    | b         | 2, 3, 4    |
    | c         | 1, 3, 4    |
    | d         | 1, 3, 4    |
    | e         | 2, 3, 4, 5 |
    | f         | 1, 2, 4    |
    | g         | 2, 5       |

    

  - 이후, Inverted Index를 스캔하며 각 item 에 대한 리스트마다 들어있는 record id 쌍에 대해 Hash table에 있는 카운트 증가    

    | 레코드 id 쌍 | 카운트 (공통 item 수) |
    | ------------ | --------------------- |
    | (2, 3)       | 3                     |
    | (3,5)        | 2                     |
    | (2, 5)       | 3                     |
    | (3, 4)       | 4                     |
    | (2, 4)       | 3                     |
    | (1, 3)       | 2                     |
    | (1, 4)       | 3                     |
    | (4, 5)       | 1                     |
    | ...          | ...                   |

    - pair가 이미 있으면 카운트 +1, 없으면 추가        



- 1번째 페이즈    

  - Set 데이터에 있는 각 튜플의 item을 key로 하는 inverted index 생성    

    => 그 후, 모든 쌍과 1 출력            

  - Map 함수   

    - Key : item  
    - Value : 레코드 id    

  - 셔플 페이즈   

    - 동일 item에 대해 레코드 id를 List로 생성 => Inverted Index 생성 완료    

  - Reduce 함수   

    - Key : inverted index 의 각 item 마다 레코드 id 쌍   
    - Value : 1    

- 2번째 페이즈     

  - 1페이즈의 Reduce 함수를 거친 출력파일이 입력으로 들어옴    
  - 모든 레코드 id 쌍에 대해 word counting 알고리즘을 사용하여 카운트    
    - Map 함수   
      - line 1줄로 들어온 데이터들을 Key - Value 쌍으로 변경          
    - 셔플 페이즈  
      - 동일 key에 대해 Value List 생성   
    - Reduce    
      - 생성된 Value List 의 합계를 Value로 출력          


## Common Item Counting for Every Pair of Sets 코딩     

- CommonItemCount.template.java 파일을 CommonItemCount.java 파일로 복사하여 코드 작성          

  - Inverted Index 를 이용하여 모든 튜플 간 중복 item 개수 카운트하는 알고리즘      

  - 셀프 조인이므로 테이블 이름은 데이터에 없음     

    +) 셀프 조인이므로 레코드 id 는 작은 것이 먼저 나와야 함 (중복 방지 겸)          

- `Driver.java` 파일에 등록 필요   

  - pgd.addClass("itemcount", CommonItemCount.class, "A map/reduce program that performs the common item count using the inverted index for a single input file");          

- 이후 Project 디렉토리에서 `ant` 명령어로 컴파일      

- `hdfs dfs -mkdir itemcount_test` 로 데이터를 위한 폴더 생성   

- `hdfs dfs -put data/setjoin-data.txt itemcount_test`  로 데이터 복사    

- `hadoop jar ssafy.jar itemcount itemcount_test itemcount_test_out1 itemcount_test_out2` 로 실행      

  - 각 페이즈마다 출력 디렉토리 따로 설정     

- `hdfs dfs -cat itemcount_test_out2/part-r-00000`     

- `hdfs dfs -cat itemcount_test_out2/part-r-00001`           


