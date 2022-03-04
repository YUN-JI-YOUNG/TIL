## 1. 행렬곱

### Matrix Multiplication      

- 행렬곱 : (A 행렬의 i 행 p 열 원소) X (B 행렬의 p 행 j 열 원소) = (C 행렬의 i행 j열 원소)           

  - 즉, C(i,j) = [A(i,0) * B(0,j)] + [A(i,1) * B(1,j)] ... + [A(i,m) * B(m,j)]    

  - m은 A행렬의 열 개수임과 동시에, B 행렬의 행 개수      

    => 행렬곱은 A `x*a` , B `a*y` 처럼 A 행렬의 열 수와 B 행렬의 행 수가 같을때만 가능하므로    


#### 1-Phase Matrix Multiplication      

- A 행렬의 행 하나, B 행렬의 열 하나가 모두 들어갈 수 있는 메모리 필요    

- C(i,j)를 구하기 위해 곱해야하는 A(i,0), ... A(i,m) , B(0,j),...B(m,j) 를 메인메모리에 넣고 같은 p 값을 갖는 값끼리 곱하고 그렇게 곱한 값끼리 더해야하므로 해당 Value List를 모두 메인 메모리에 넣어야 한다.      

  => 그러므로, 각 행렬의 크기가 매우 큰데, 메인 메모리의 크기가 작다면 이 알고리즘은 돌아갈 수 없음    

  => 2-Phase Matrix Multiplication 알고리즘 필요     

- A(i,p)   

  - Key : (i,1), (i,2), (i,3), ... , (i,m)  = m은 B행렬의 행의 개수    
    - 행렬곱이 성립하려면 A행렬의 열의 개수 == B행렬의 행의 개수이므로     
    - A행렬에선 i번째 행의 값들을 계산에 사용하므로 행은 i로 고정       
  - Value : (p, A(i,p)) = 몇번째 열의 원소인지 알려주는 역할     

- B(p,j)   

  - Key : (1,j), (2,j), (3,j), ... , (m,j) = m은 A 행렬의 열의 개수    
  - Value : (p, B(p,j))     



#### 2-Phase Matrix Multiplication   

- 1페이즈만 사용하는 1-Phase Matrix Multiplication 과 달리, 페이즈가 2번으로 나뉜다.    

- 1페이즈에선, [A(i,m) * B(m,j)]에서 곱해야하는 2개의 값만 메인 메모리에 넣으면 되고,   

  2페이즈에선, 곱한 값들이 들어있는 Value List를 Sum 에 더하기만 하고 지우면 되므로 Value List를 모두 메인 메모리에 넣을 필요가 없다.           

  => 상대적으로 1-Phase Matrix Multiplication 에 비해 더 적은 메모리 소요      

  => 그러므로 Matrix 크기가 크고 메인메모리가 작더라도 이 알고리즘은 항상 돌아갈 수 있음   

  => 하지만 오버헤드가 심하므로 메인메모리가 Matrix 크기를 커버할 수 있다면, 1-Phase Matrix Multiplication 알고리즘을 사용하는 것이 훨씬 빠르다.      

- A(i,p)   

  - Key : (i, 1, p), (i, 2, p), (i, 3, p), ... (i, m, p)  = m은 B 행렬의 행의 개수        

    - 기본적으론 1-Phase 알고리즘과 동일하지만, p까지 key List에 포함되는 점이 다름   

    - 추후 A행렬과 B 행렬의 동일한 key를 가진 하나의 값만 value List에 담고, 1 페이즈에서 Reduce 함수를 통해 곱을 구한다.     

      그 후, 2페이즈에선, 1페이즈에서 구한 값을 Sum에 더하고 지운다.           

  - Value : (A(i,p))    

- B(p, j)   

  - Key : (1,j,p), (2,j,p), (3,j,p), ... , (m, j, p) = m은 A 행렬의 열의 개수       
  - Value : (B(p,j))    



#### MatrixMulti - 1phase 준비 코딩   

- MatrixMulti.template.java 파일을 MatrixMulti.java 파일로 복사하여 코드 작성        

  - Tokenizer 는 input을 String 타입으로만 쓸 수 있어서 Text를 String으로 변환하기 위한 `value.toString()` 함수 필요     

    - StringTokenizer(value.toString())   

  - 토큰으로 나눠둔 것을 하나씩 읽기위해 `nextToken()` 함수 필요     

    - 그렇게 읽은 값들을 Int로 형변환하기 위해 `parseInt` 함수 필요   

      parseInt(token.nextToken())     

  - 행렬 이름과 일치하는지 확인하기 위해 `equals()` 함수 필요    

    - equals("A")   

- `Driver.java` 파일에 등록 필요   

  - pgd.addClass("matmulti", MatrixMulti.class, "1-phase Matrix Multiplication");    

- 이후 Project 디렉토리에서 `ant` 명령어로 컴파일      

- `hdfs dfs -mkdir matmulti_test` 로 데이터를 위한 폴더 생성   

- `hdfs dfs -put data/matmulti-data-2x2.txt matmulti_test` 로 데이터 복사      

- `hadoop jar ssafy.jar matmulti A B 2 2 2 matmulti_test matmulti_test_out` 로 실행      

  - 커맨드라인으로 `행렬 이름`, `첫번째 행렬 행 수`,  `첫번째 행렬 열 수(=두 번째 행렬 행 수)`,   `두 번째 행렬 열 수` 추가 입력   

- `hdfs dfs -cat matmulti_test_out/part-r-00000` 

- `hdfs dfs -cat matmulti_test_out/part-r-00001`     

<hr>


### Result   

```java
// matmulti_test_out/part-r-00000   

0,1	1 3
0,1	0 4
0,1	1 -5
0,1	0 2
1,0	0 -1
1,0	1 7
1,0	1 6
1,0	0 1
```

```java
// matmulti_test_out/part-r-00001

0,0	1 3
0,0	1 6
0,0	0 -1
0,0	0 4
1,1	1 -5
1,1	0 2
1,1	0 1
1,1	1 7
```

