### 최단경로   

- 간선의 가중치가 있는 그래프에서 두 정점 사이의 경로들 중에 간선의 가중치의 합이 최소인 경로   
- 하나의 시작 정점에서 끝 정점까지의 최단 경로   
  - 다익스트라(dijkstra) 알고리즘   
    - 음의 가중치 허용 X
  - 벨만포드(Bellman-Ford) 알고리즘   
    - 음의 가중치 허용 O 
- 모든 정점들에 대한 최단 경로   
  - 플로이드 워샬(Floyd-Warshall) 알고리즘     



#### 다익스트라 알고리즘   

- 시작 정점(s)에서 끝정점(t)까지의 최단 경로에 정점 x 존재   

- s - x 까지의 최단경로, x-t 까지의 최단경로로 구성   

- 그리디 기법을 사용한 알고리즘으로 MST의 Prim 알고리즘과 유사   

  의사코드   

```python
!! 의사코드 !! 
def Dijkstra(s,A,D):
    U = {s};
    
    for 모든 정점 v:
        D[v] = A[s][v]
        
    while U != V:
        D[w]가 최소인 정점 w
        U = U U {w}
        
        for w에 인접한 모든 정점 v:
            D[v] = min(D[v], D[w] + A[w][v])
```

​		실제코드   

```python
!! 실제코드 !!

def dijkstra(s,V):
    U = [0]*(V+1)		# 방문표시
    U[s] = 1
    for v in range(V+1):
        D[v] = adj[s][v]
        
    # while len(U)!= V:
    for _ in range(V):	# V = 정점개수-1과 같으므로 = 남은 정점개수
        minV = INF:
        w = 0
        for i in range(V+1):
            if U[i] == 0 and minV > D[i]:
                minV = D[i]
                w = i
        U[w] = 1
        
        for v in range(V+1):
            # w에 인접이면 = adj[w][v]가 가중치가 존재하므로
            if 0 < adj[w][v] < INF:
                # 시작정점에서 v로 가는 기존 비용(D[v])과 비교 후 선택  
                D[v] = min(D[v], D[w]+adj[w][v])
                # 시작정점에서 w = D[w] , w에서 v로 가는 비용 adj[w][v]
                # = 시작정점에서 w를 거쳐 v로 가는 비용 = D[w]+adj[w][v]

INF = 10000
V,E = map(int,input().split())
adj = [[INF]*(V+1) for _ in range(V+1)]
for i in range(V+1):
    adj[i][i] = 0		# 자신한테 돌아오는건 0으로 처리 -> 없어도 ok
    
for _ in range(E):
    u,v,w = map(int,input().split())
    adj[u][v] = w		# 유향 그래프인 경우

D = [0] * (V+1)
dijkstra(0,V)
print(D)
```



