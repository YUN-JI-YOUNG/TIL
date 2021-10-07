## 백트래킹

- 과정 

  1. 여러가지 선택지들이 존재하는 상황에서 한 가지 선택
  2. 선택이 이루어지면 새로운 선택지들의 집합 생성
     - 더이상의 선택지가 없다면 이전의 선택지로 돌아가서 다른 선택 
  3. 위의 과정을 반복하며 최종 상태에 도달
     - 올바른 선택이었다면 목표 상태에 도달    
     - 목표상태가 아닌 곳에 도달했다면 최근의 선택으로 돌아가서 다시 선택     

  - 어떤 노드의 유망성을 점검한 후에, 유망하지 않다면 부모로 돌아가 다음 자식 노드로 이동   
  - 유망하지 않은 노드가 포함되는 경로는 더이상 고려 하지 않는다 = 가지치기  
    1. 상태 공간 트리의 DFS 실시   
    2. 노드의 유망성 점검  
    3. 유망하지 않다면 가지치기 -> 부모노드로 돌아가서 검색 계속    



- 백트래킹과 DFS 차이    

  - 백트래킹은 시도의 횟수를 줄일 수 있는 가지치기가 존재    
  - DFS 는 모든 경로를 추적하는데 비해, 백트래킹은 불필요한 경로를 조기 차단   
  - n!처럼 DFS로는 경우의 수가 많아 처리 불가능한 문제는 백트래킹으로 해결 가능    
  - 백트래킹 알고리즘을 적용하면 일반적으로 경우의 수는 줄어들지만, 그래도 최악의 경우 여전히 지수함수 시간을 요하므로 처리 불가능할 수 있음    

- 문제 종류  

  - 8-Queen 문제   

    - 퀸 8개를 8*8 크기의 체스판 안에 서로를 공격할 수 없도록 배치하는 모든 경우   
    - 후보 해의 수 : 64C8 = 4,426,165,368    
    - 실제 해의 수 : 92개
    - 44억개가 넘는 후보에서 92개를 효율적으로 찾아내는 것이 관건    

    

  - 백트래킹으로 부분집합 문제   

    ```python
    # a : powerset 저장할 배열   
    # c ; 들어갈지 / 말지 체크하는 배열
    c[MAXCANDIDATES]	# 후보
    ncands 				# 후보 수
    
    c = [0] * 2
    ncands = 2
    
    def backtrack(a,k,input):
        if k == input:
            solution(a,k)
        else:
            k+=1
            make_candidates(a,k,input,c,ncands)
            for i in range(ncands):
                a[k] = c[i]
                backtrack(a,k,input)
                
    def make_candidates(a,k,n,c,nacnds):
        c[0] = True
        c[1] = False
        ncands = 2
    
    def solution(a,k):
        for i in range(1,k+1):
            if a[i] == True:
                print(i)
    ```

  ```python
  # ex2. 부분집합 원소의 합이 10인 부분집합 구하기
  
  li = [1,2,3,4,5,6,7,8,9,10]
  visited = [0] * (len(li)+1)
  result = []
  c = [0] * 2     # 들어가냐 안들어가냐 2가지 경우만 체크
  
  def backtrack(k, input):
      if k == input:
          ans = []
          for i in range(1, k + 1):
              if visited[i] == True:
                  if sum(ans) > 10:
                      return
                  else:
                      ans.append(li[i - 1])   # 현재 인덱스가 1부터 시작하므로
          if sum(ans) == 10:
              result.append(ans)
      else:
          k += 1
          make_candidates(c)
          for i in range(2):
              visited[k] = c[i]
              backtrack(k, input)
  
  def make_candidates(c):
      c[0] = True
      c[1] = False
  
  
  backtrack(0,len(li))
  print(result)
  ```

  

  - 백트래킹으로 순열

    ```python
    # 사용한 후보는 추려내는(가지치기) 방법 
    
    def bactrack(a,k,input):
        c[MAXCANDIDATES]	# 후보
        ncands 				# 후보 수
        if k == input:
            solution(a,k)
        else:
            k+=1
            make_candidates(a,k,input,c,ncands)
            for i in range(ncands):
                a[k] = c[i]
                backtrack(a,k,input)
     
    def make_candidates(a,k,n,c,nacnds):
        in_perm[NMAX] = False	  # 숫자를 인덱스로 사용여부 체크하는 카운트배열
        for i in range(1,k):
            in_perm[a[i]] = True
        ncand = 0
        for i in range(1,n+1):
            if in_perm[i] == False:
                c[nancds] = i
                ncnad +=1
                
    def solution(a,k):
        for i in range(1,k+1):
            if a[i] == True:
                print(i)
    ```

    

  



