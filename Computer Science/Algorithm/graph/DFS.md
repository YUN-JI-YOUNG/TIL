### DFS(깊이 우선 탐색)     

###### 시작 정점의 한 방향으로 갈 수 있는 경로가 있는 곳까지 깊이 탐색하다가 더이상 못 가면, 가장 마지막에 만난 갈림길로 돌아가서 다른 방향의 정점으로 탐색을 반복하여 모든 정점을 방문하는 순회 방법      

- 비선형구조인 그래프 구조는 그래프로 표현된 모든 자료를 빠짐없이 검색하는 것이 중요    

  - 비선형 구조는 트리처럼 1 : N 의 관계를 갖는 구조      
  - 탐색 방법    
    - 깊이 우선 탐색 (DFS = Depth First Search)     
    - 너비 우선 탐색 (BFS = Breadth First Search)       

- 가장 마지막에 만난 갈림길로 돌아가서 다시 반복해야하므로 후입선출 구조의 스택 사용       

  - 1 -> 2 -> 3 -> 4 에서 막히면, 3 갈림길의 다른 방향 탐색, 2 갈림길의 다른 방향 탐색      
  - 정보를 저장하는 방법 중 하나로 스택을 사용할 수 있다는 뜻!   **스택만 가능한 것은 아님**        
    - 뜬금없는 경로가 아닌 지나온 경로를 최근 순으로 꺼낼 수 있어야 하므로        

- 스택을 사용한 DFS 구현 방법   

  `정점을 스택에 push하고, 갈곳이 없으면 스택에서 pop하여 갈 수 있는 곳 있을 때까지 돌아가기`  

  `1)` 시작 정점 v를 결정하여 방문     

  `2)` 정점 v에 인접한 정점 중 (v에서 갈 수 있는 곳 중)

  - 방문하지 않은 정점 w 가 있다면,         

    정점 v를 스택에 push하고 정점 w 방문, w를 v로 하여 `2)` 반복     

  - 방문하지 않은 정점이 없다면,              

    방향을 바꾸기 위해 스택을 pop하여 받은 가장 마지막 방문 정점을 v로 하여 `2)` 반복   

  `3)` 스택이 공백이 될때까지 `2)` 반복

  **다른 방법 >**  `다른 경로 후보를 스택에 저장한 후, 막히면 스택에서 꺼내서 다시 탐색 시작`   

  ```python
  # 의사코드
  
  A - B - D  
   \   \    \
     C - E  - F - G
  # 최종 깊이 우선 탐색 경로 : A-B-D-F-E-C-G
  
  visited[], stack[] 초기화
    # 초기 상태 : 배열 visited를 False로 초기화, 공백 스택 생성   
    # visited 는 방문 여부 체크를 위한 배열    
  DFS(v)
  	v 방문;
      vistied[v] <- True
      push(A);           # 인접행렬 조사 -> 인점한 정점 B 존재 -> A를 push    
      visited[B] <- True  # v를 B로 함 > B 방문
      push(B);           # 방문 경로 저장 
      visited[D] <- True  # 인접정점 D 방문
      push(D);
      vistied[F] <- True  # 인접정점 F 방문 
      push(F);
      visited[E] <- True
      push(E);
      visited[C] <- True   # 더이상 갈 곳이 없음
      pop(stack);         # E pop -> 아직 w가 될 곳이 없음
  	pop(stack);         # F pop -> G가 아직 갈 수 있음 
      push(F);
      visited[G] <- True   # 더 갈 곳 없음    
      pop(stack);         # pop 반복하여 더이상 꺼낼 것이 없으면 출발점이므로 종료   
  ```

  - 인접행렬 : 2차원 배열로, 연결된 곳은 1, 연결 안 된 곳은 0으로 표시    

  ```python
  # 실제 코드 
  
  A - B - D  
   \   \    \
     C - E  - F - G
  
  # A,B,C,D,E,F,G = 1,2,3,4,5,6,7
  # 1부터 시작하므로 영행렬 추가
  adj = [[0,0,0,0,0,0,0,0],
         [0,0,1,1,0,0,0,0],
         [0,1,0,0,1,1,0,0],
         [0,1,0,0,0,1,0,0],
         [0,0,1,0,0,0,1,0],
         [0,0,1,1,0,0,1,0],
         [0,0,0,0,1,1,0,1],
         [0,0,0,0,0,0,1,0]]
  node = ['','1','2','3','4','5','6','7']
  dfs(1,7)
  # 답 = 1 2 4 6 5 3 7
  #    = A B D F E C G
  
  # 만약 테스트 케이스가 7 8 이런식으로 주어진다면,
  V, E = map(int, input().split())
  ad = [[0]*(V+1) for _ in range(V+1)]
  for _ in range(E):
      n1, n2 = map(int, input().split())  #-> 연결되어있는 노드 입력받기
      arr[n1][n2] = 1    # 방향이 없는 형태 1 - 2 면 1과 2 인접, 2과 1도 인접
      arr[n2][n1] = 1     # 만약, 방향이 있다면 1->2일때 arr[1][2]만 1
      
  # 스택 이용한 dfs 구현
  dfs(s, V):
      visited = [0] * (V+1)
      stack = []
      i = s    # 현재 방문 정점 i 
      visited[i] = 1
      print(node[i])
      while i != 0 :     # 기본적으로 True나 마찬가지
          for w in range(1, V+1):
              if adj[i][w] == 1 and visited[w] == 0 :
                  stack.append(i)  # 방문 경로 저장 
                  i = w            # 새 방문지 이동 
                  visited[w] = 1
                  print(node[i])
                  break            # 인접 찾는 for문 중단하여 w 초기화 후 인접 찾기
          else:
              if stack:
                  i = stack.pop()
              else:                # 스택이 공백이면
                  i = 0            # i를 0으로 -> while문 중단이나 마찬가지   
                  
  ```     
  
  ## 2. 백트래킹

###### 해를 찾는 도중 막히면, 되돌아가서 다시 해를 찾아 가는 기법

- 백트래킹 기법은 최적화 문제와 결정 문제 해결 가능     

  - 최적화 문제 - 최단거리 등 
  - 결정 문제 - 해가 존재하는지 여부를 yes / no 로 답하는 문제
    - ex. 경로 문제 / 미로 찾기 / n-Queen / map coloring / 부분집합의 합 문제 등
    - 미로 찾기 -> 입구, 출구가 있는 미로에서 입구에서 출구까지의 경로를 찾는 문제

- DFS 을 재귀 등으로 먼저 구현하고, 거기에 조건을 건 형태가 백트래킹이라고 할 수 있음

- 백트래킹과 DFS(깊이 우선탐색)은 방식이 비슷하지만, 차이가 존재

  - 백트래킹은 어떤 노드에서 출발하는 경로가 해결책으로 이어질 것 같지 않으면 조기에 차단하여 시도의 횟수를 줄임 (Prunning 가지치기) <-> 깊이우선탐색은 모든 경로 탐색
  - 깊이 우선 탐색을 하기엔 경우의 수가 너무 많으면, 즉, N! 가지의 경우의 수를 가진 문제에 대해선 깊이우선탐색으론 불가능하므로 백트래킹 사용
  - 하지만, 백트래킹 역시 최악의 경우엔 여전히 지수함수 시간을 요하므로 처리 불가    

- 어떤 노드의 유망성을 점검한 후 유망(promising)하지 않다고 결정되면 부모로 돌아가 (backtracking) 다음 다식 노드로 감      

  - 어떤 노드를 방문했을 때, 해당 노드를 포함한 경로가 해결책이 될 수 없으면 유망하지 않다고 함 / 해답의 가능성이 있으면 유망하다고 함   
    - ex. 최단거리를 찾는 문제에서, 이미 갔던 경로보다 더 길어진다면, 유망하지 않으므로 다시 돌아감    
  - 가지치기(pruning) : 유망하지 않는 노드가 포함된 경로는 더이상 고려하지 X

- N-Queen 문제

  - 백트래킹 (27 노드) vs 순수한 깊이 우선 탐색 (155 노드)     

  - 순수한 DFS는 퀸을 (1,1),(2,1),(3,1),(4,1)에 우선 다 놓아보고, 가능한지 여부를 판단한다면,

    백트래킹은 퀸을 (1,1)에 두고, (2,1)에 둬서 가능한지 판단해서 유망하지 않으면(불가하면) 돌아가서 다음 자식 노드에 두는 형식     

- 부분집합 합 구하기

  - powerset을 구하는 백트래킹 알고리즘(dfs 기반)

  ```python
  # 슈더코드
  def backtrack(a, k, input):      # input : 원소 개수
      global MAXCANDIDATES
      c = [0] * MAXCANDIDATES
      
      if k == input:		# 모든 결정이 끝났다면(마지막 원소까지)
          process_solution(a,k) # 원하는 작업 수행
  # 가지치기 필요(현재까지의 합이 타겟보다 크다면, 남은 원소들을 다 더해도 타겟보다 작다면, 더이상 볼 필요 없음) -> 항상 효과적인건 아님
      else:
          k += 1
          ncandidates = construct_candidates(a, k, input, c)v # 유망 후보 추천
          for i in range(ncandidates):	# 추천받은 후보 순회
              a[k] = c[i]
              backtrack(a,k,input)
  def construct_candidates(a, k, input, c):  # 부분집합에서 후보는 0,1 2개뿐
      c[0] = True 
      c[1] = False
      return 2
  MAXCANDIDATES = 100
  NMAX = 100
  a = [0] * NMAX
  backtrack(a, 0, 3)
  
  # 가지치기 코드 (양자택일이 아닌, 양갈래로 갈라지면 재귀호출이 2번 발생)
  f(i,N,s,t)
  		if s == t:		# i-1 원소까지의 합이 찾는 값인 경우
          elif i == N :	# 모든 원소에 대한 고려가 끝난 경우
          elif s > t :	# 남은 원소를 고려할 필요 X (이미 합이 타겟보다 커짐)
          else:			# 남은 원소가 있고, s < t인 경우 -> 지속
              subset[i] = 1		#bit배열과 비슷
              f(i+1,N,s+A[i],t)	# i 원소 포함
              subset[i] = 0
              f(i+1,N,s,t)		# i 원소 미포함
              
  # 추가 고려사항 
  # 커질때뿐만 아니라, 현재까지의 합(s)에 남은 원소들의 합(rs)을 더해도 t 미만이면 중단
  # rs = 총 합
  # f(i+1,N,s+A[i],t,rs-A[i])  i를 포함하든, 안하든 어쨌든 A[i]는 넘어간 것이므로
  # f(i+1,N,s,t,rs-A[i])
  ```

  

- 순열 생성 (dfs 기반 백트래킹 이용)

  - 앞에서 사용한 원소는 사용하지 않는다.

  ```python
  # 현실적인 순열 만들기
  P = [1,2,3]
  f(i, N):  # i = 인덱스, N = 배열의 크기
      if i == N : 	# 순열 완성
          
      else:
          for j : i -> N-1  	#가능한 모든 원소에 대해 P[i] 결정
              #처음은 자기자신과 바꾸기 = 그대로 두기
             	P[i] <-> P[j]	# j번째 원소와 교환(즉, 자기 오른쪽 원소와 교환) 
          	f(i+1,N)		# 다음 결정 
              P[i] <-> P[j]	# 되돌아오면 원상복구
  # 실제 코드
  def f(i,N):
      if i == N :		# 순열 완성
          print(P)
  	else:			# i번 원소값 결정
          for j in range(1, N):  # 자신부터 오른쪽 원소와 교환
              P[i], P[j] = P[i], P[j]
              f(i+1,N)
              P[i], P[j] = P[i], P[j]
  f(0, len(P))     
  
  # N개 배열중 r개짜리 순열 만들기
  def f(i,N,r):
      if i == r :		# 순열 완성
          print(P[0:r])
  	else:			# i번 원소값 결정
          for j in range(1, N):  # 자신부터 오른쪽 원소와 교환
              P[i], P[j] = P[i], P[j]
              f(i+1,N, r)
              P[i], P[j] = P[i], P[j]
  f(0, len(P), r)   
      
  
  
  # loop 사용
  for i1 in range(1,4):
      for i2 in range(1,4):
          if i2 != i1:
              for i3 in range(1,4):
                  if i3!= i1 and i3 != i2:
                      print(i1,i2,i3)
  
  # 백트래킹 사용 - 이후 응용파트에서 자세히 다룰 것
  def backtrack(a, k, input):      # input : 원소 개수
      global MAXCANDIDATES
      c = [0] * MAXCANDIDATES
      
      if k == input:		# 모든 결정이 끝났다면(마지막 원소까지)
          for i in range(1,k+1):
              print(a[i], end = " ")
          print()
      else:
          k += 1
          ncandidates = construct_candidates(a, k, input, c)v # 유망 후보 추천
          for i in range(ncandidates):	# 추천받은 후보 순회
              a[k] = c[i]
              backtrack(a,k,input)
  def construct_candidates(a, k, input, c):  # 부분집합에서 후보는 0,1 2개뿐
      in_perm = [False] * NMAX
      
      for i in range(1,k):
          in_perm[a[i]] = True
          
      ncandidates = 0
      for i in range(1, input+1):
          if in_perm[i] == False:
              c[ncandidates] = i
              ncandidates += 1
      return ncandidates
  ```

  
