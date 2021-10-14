### 서로소 집합    

- 서로소 또는 상호배타 집합들은 서로 중복 포함된 원소가 없는 집합    

- 집합에 속한 하나의 특정 멤버(대표자)를 통해 각 집합을 구분    

- 표현 방법   

  - 연결 리스트    
    - 같은 집합의 원소들은 하나의 연결리스트로 관리   
    - 연결리스트의 맨 앞 원소를 대표 원소로 설정   
    - 각 원소는 집합의 대표원소를 가리키는 링크 보유    
  - 트리     
    - 하나의 집합을 하나의 트리로 표현   
    - 자식 노드가 부모 노드를 가리키고, 루트 노드가 대표원소이며, 대표원소는 자기자신을 가리킨다.        

- 활용 연산   

  - Make-Set(x)  : x를 대표원소로 하는 집합 하나 생성   

    - `p[x] = x`  

  - Find-Set(x) : x가 포함된 집합의 대표 원소 알아내기    

    - `if x == p[x]`  

      `return x`  

  - Union(x,y) : x가 속한 집합과 y가 속한 집합 합치기 = 각각의 대표원소를 하나로 합치기      

    - `p[Find-Set(y)] = Find-Set(x) `  

- 연산의 효율을 높이는 방법  

  - Rank를 이용한 Union  

    - 각 노드는 자신을 루트로 하는 subtree의 높이를 Rank라는 이름으로 저장   

      두 집합을 합칠 때, rank가 낮은 집합을 rank가 높은 집합에 붙인다      

    - 만약 두 rank가 동일하다면, 한 집합의 rank를 높인후, 다른 집합을 거기에 붙인다.   

  - Path compression   

    - Find-Set을 행하는 과정에서 만나는 모든 노드들이 직접 root를 가리키도록 포인터를 변경   
    - 재귀로 하면 간단하게 구현 가능   
    - 대표 원소를 찾으려고 할 때, 필요없는 과정을 줄일 수 있음    



### 최소 신장 트리 (MST)     

###### Minimum Spanning Tree (최소 스패닝 트리)  

- 신장 트리  

  - n개의 정점으로 이루어진 무향 그래프에서 n개의 정점과 n-1개의 간선으로 이루어진 트리    

- 최소 신장 트리   

  - 무향 가중치 그래프에서 신장 트리를 구성하는 간선들의 가중치의 합이 최소인 신장 트리   

- Prim 알고리즘   

  - 하나의 정점에서 연결된 간선들 중에 하나씩 선택하면서 MST를 만들어가는 방식   

    1. 임의의 정점 하나 선택(=MST에 추가)

    2. 선택한 정점의 인접정점들 중 최소 비용의 간선이 존재하는 정점 선택(=MST에 추가)    

    3. 모든 정점이 선택될때까지 반복   

  - 서로소인 2개의 집합 정보 유지   

    - 트리 정점들 - 선택된 정점들   
    - 비트리 정점들 - 아직 선택되지 않은 정점들   

  ```python
  # 우선순위 큐를 사용하지 않고 구현하는 알고리즘   
  
  key = 가중치 배열
  p = 부모 배열
  adj = 인접행렬 -> bfs/dfs 처럼 0이나 1을 넣는게 아니라 가중치 넣기   
  INF = 10000      # 987654321도 가능
  
  def MST_prim(start, V):
      key = [INF] * (V+1)		# INF = 주어지는 비용의 최댓값
      key[start] = 0 			# key[i] 는 i가 MST에 연결되는 비용   
      MST = [0] * (V+1)
      pi = [0] * (V+1)
      # 모든 정점이 MST에 포함될 때 까지
      for _ in range(V):
          # MST에 포함되지 않은 정점 중 가중치가 최소인 u 찾기 
          # = key 배열 값이 최소인 u 찾기 
          # -> 왜냐하면, 처음 돌때, start에 인접인 정점에 대해 마지막 for문에서 
          # key 값을 가중치로 갱신했으므로 
    
          u = 0		# -> 없어도 되는 코드   
          minV = INF			# minV는 INF로 초기화   
          for i in range(1,V+1):		# 1번 ~ V번 노드까지 탐색   
              if MST[i] == 0:
                  if key[i] < minV:
                      u = i
                      minV = key[i]
          # 가중치가 최소인 u를 MST에 추가 = 방문여부 표시
          MST[u] = 1
          for v in range(V+1):
              # u와 인접이면서 MST에 아직 포함되지 않은 v에 대해
              if MST[v] == 0 and adj[u][v] != 0:
                  # u을 이용해 기존보다 더 작은 비용으로 연결될 수 있으면 갱신   
                  if key[v] > adj[u][v]:
                      # key 배열값을 해당 가중치로 갱신   
                      key[v] = adj[u][v]
                      # v는 u와 연결해서 MST에 포함될 예정
                      pi[v] = u
      # 가중치의 합 리턴
      return sum(key)
  
  
  ```

  

- KRUSKAL 알고리즘   

  - 간선을 하나씩 선택해서 MST를 찾는 알고리즘   
    1. 모든 간선을 가중치에 따라 오름차순 정렬   
    2. 가중치가 낮은 간선부터 선택하면서 트리를 증가시킴   
       - 선택해서 사이클이 된다면, 스킵하고 다음으로 가중치가 낮은 간선 선택  
       - Union을 사용해서 대표원소를 갱신해주는데, 만약 선택한 간선의 두 정점이 대표원소가 동일하다면 스킵하는 방식    
    3. n-1개의 간선이 될때까지 반복   
  - 의사코드    

  ```python
  !! 의사코드 !!
  
  def MST-KRUSKAL(G,w):
      A = {}		# 공집합
      for v in G.v:		# G.v는 그래프 정점의 집합
          Make_Set(v)
          
      G.E에 포함된 간선들을 가중치 w에 의해 정렬 
      
      for 가중치가 낮은 간선(u,v) 선택 in G.E:	# G.E는 그래프 간선의 집합 
          # n-1개의 간선을 모두 선택한다면 break   
          # u와 v의 대표원소가 다르다면 해당 간선 선택
          if Find-Set(u) != Find_set(v):
              A = A U {(u,v)}
              Union(u,v);
     return A
  
  # 만약 가중치만 알고싶다면
  def MST-KRUSKAL(G,w):	
      A = {}		# 공집합
      for v in G.v:		
          Make_Set(v)
          
      G.E에 포함된 간선들을 가중치 w에 의해 정렬 
      
      s = 0
      for 가중치가 낮은 간선(u,v) 선택 in G.E :   # G.E는 그래프 간선의 집합  
          # n-1개의 간선을 모두 선택한다면 break   
          # u와 v의 대표원소가 다르다면 해당 간선 선택
          if Find-Set(u) != Find_set(v):
              s += adj[u][v]
              Union(u,v);
     return A
  ```

  - 실제 코드

  ```python
  !! 실제 코드 !!
  
  def find_set(x):
      while p[x]!= x:		# 대표원소가 아니면
          x= p[x]			# x가 가리키는 정점으로 이동
      return x
  
  V,E = map(int,input().split())
  edge = []
  for _ in range(E):
      u,v,w = map(int,input().split())
      edge.append((w,u,v))		# 가중치를 기준으로 정렬하기 위해 순서 바꿈  
  edge.sort()
  # 대표원소 초기화  
  p = [i for i in range(V+1)]
        
  N = V+1
  cnt = 0
  total = 0
  for w,u,v in edge:
      if find_set(u) != find_set(v): # 사이클을 형성하지 않으면=대표원소가 다르면
          # 간선선택 -> cnt +1
          cnt += 1
          # 가중치 합에 +
          total += w
          p[find_set(v)] = find_set(u)
          if cnt == N-1:		# 선택한 간선의 수가 N-1개이면 break
              break
  
  ```

  

  
