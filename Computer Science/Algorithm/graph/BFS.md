## 너비 우선 탐색 (Breadth First Search, BFS)

- 시작점의 인접 정점 모두 방문 -> 방문 정점을 시작점으로 하여 다시 인접 정점 방문     

- 인접 정점들에 대해 탐색한 후 차례로 다시 너비 우선 탐색해야 하므로 FIFO 형태의 큐 활용    

- 시작점이 여러 개여도 탐색 가능 (ex. 바이러스가 인접된 사람을 감염시킬 때, 초기 감염자가 여러명인 경우의 탐색 등)   

  ```python
  bfs 체크리스트
  # 큐 생성 + 초기화
  # visited 생성
  # 시작점 인큐
  # 시작점 visited
  while # 큐에 원소가 있다면
  	  # 디큐해서 t에 저장
      	# t에 대한 작업 - print든 if로 특정 도착점 찾는 것이든 등등
      for # t에 인접하고 방문하지 않은 모든 i에 대해
      	# i 인큐
          # i 방문 표시 = 큐에 들어갔으므로 중복으로 넣지 말라는 의미
  i 방문 표시를 visited[i] = visited[t]+1 로 하는 이유
  -> 시작 정점에 대한 거리를 알 수 있음  + 각 정점까지의 엣지 수 + 1과 같음
  -> 최단거리 등 거리에 대한 문제, 엣지의 총 합 같은 문제에 적용 가능 
  
  #인접 행렬
  def BFS(v,n): 				# 시작점 v, 정점 n개
      visited = [0] * (n+1)
      queue = []
      queue.append(v) 		# 시작점 큐 삽입 -> 시작점이 여러개면 모두 삽입 가능
      visited[v] = 1
      while queue:			# 큐에 원소가 있다면
          t = queue.pop(0)	# 꺼내서 t에 저장
          **print(t)**			# t에 대한 처리 / 
          # 만약 G까지의 도착 가능 여부를 찾는다면 if t == G ; return 1 
          # while문 다 돌면 못찾았다는 뜻이니까 return 0
          for i in range(1,n+1):
          	if adj[t][i] == 1 and not visited[i] :	# 인접인데 방문하지 않았다면
              	queue.append(i)					# 큐에 넣어서 대기줄 세우기
                  visited[i] = visited[t]+1		# i visited 표시
                  
                     
  V, E = map(int, input().split())
  edge = list(map(int, input().split())
  adj = [[0] * (V+1) for _ in range(V+1)]		
  
  for i in range(E):
      n1,n2 =edge[2*i], edge[2*i+1]
      adj[n1][n2] = 1
      adj[n2][n1] = 1					# 무방향 그래프
  bfs(1,V)
  
  # 인접리스트
  def BFS2(v,n): 				# 시작점 v, 정점 n개
      visited = [0] * (n+1)
      queue = []
      queue.append(v) 		# 시작점 큐 삽입 -> 시작점이 여러개면 모두 삽입 가능
      visited[v] = 1
      while queue:			# 큐에 원소가 있다면
          t = queue.pop(0)	# 꺼내서 t에 저장
          **print(t)**		# 목적에 따라 작업 변경			
          for i in adjList[t]:	# 인접리스트 순회
          	if not visited[i] :	# 방문하지 않았다면
              	queue.append(i)					# 큐에 넣어서 대기줄 세우기
                  visited[i] = visited[t]+1		# i visited 표시
              
  V, E = map(int, input().split())
  edge = list(map(int, input().split())                  
  adjList = [[] for _ in range(V+1)]		
              
  for i in range(E):
      n1,n2 =edge[2*i], edge[2*i+1]
              
      adjList[n1].append(n2)
      adjList[n2].append(n1)			# 무방향 그래프
  bfs2(1,V)
  ```

  
