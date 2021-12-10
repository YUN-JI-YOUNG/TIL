# 모의 SW 역량테스트    
</br>   

## 정답 코드   

### 1949. 등산로 조성

```python
import copy

def dfs(G, v, k, cnt):
    global max_cnt
    stack.append(v)
    while stack:
        i,j = stack.pop()
        visited[i][j] = 1
        cnt += 1
        for m in range(4):
            ni = i + di[m]
            nj = j + dj[m]
            if 0<= ni < N and 0 <= nj < N:
                if visited[ni][nj] == 0 :
                    if (G[ni][nj] < G[i][j]):
                        dfs(G,[ni,nj], k, cnt)
                    elif k > 0 :
                        diff = G[ni][nj] - G[i][j]
                        if diff+1 <= k:
                            temp = G[ni][nj]
                            G[ni][nj] -= diff + 1
                            dfs(G, [ni,nj],-1, cnt)
                            G[ni][nj] = temp
        if cnt > max_cnt:
            max_cnt = cnt
        visited[i][j] = 0
                
T = int(input())
for tc in range(1,T+1):
    N,K = map(int,input().split())
    map_list = [list(map(int,input().split())) for _ in range(N)]
    max_width = 0
    idx = []

    stack = []
    for i in range(N):
        for j in range(N):
            if map_list[i][j] > max_width:
                max_width = map_list[i][j]
    for i in range(N):
        for j in range(N):
            if map_list[i][j] == max_width:
                idx.append([i,j])
    di = [0,0,-1,1]
    dj = [-1,1,0,0]
    # 좌 우 상 하
    cnt = max_cnt = 0
    for x,y in idx:
        visited = [[0] * N for _ in range(N)]
        li = copy.deepcopy(map_list)
        dfs(li,[x,y],K,0)
    print('#{} {}'.format(tc,max_cnt))
    
```

</br>   
<hr>    

### 4012. 요리사   
```python  
def perm(n,r,s,k):
    global result
    if k == r:
        combB = []
        for m in range(N):
            if m not in combA:
                combB.append(m)
        ansA = ansB = 0
        for i in range(N//2):
            for j in range(N//2):
                if i != j:
                    ansA += board[combA[i]][combA[j]]
                    ansB += board[combB[i]][combB[j]]
        res = abs(ansA-ansB)
        result = min(res, result)
        return
    else:
        for i in range(s,n-r+k+1):
            combA[k] = i
            perm(n,r,i+1,k+1)



T = int(input())
for tc in range(1,T+1):
    N = int(input())
    board = [list(map(int,input().split())) for _ in range(N)]
    peA =[]
    peB =[]
    combA =[0] * (N//2)
    result = 987654321
    perm(N,N//2,0,0)
    print('#{} {}'.format(tc,result))
```  
- 순열은 중복이 많아 시간초과가 발생하여 조합으로 해결    

</br>   
<hr>     

### 1205. 디저트 카페    
```python   
def move(x,y,l,k,stack,visited):
    for _ in range(l-1):
        x += di[k]
        y += dj[k]
        if 0<= x < N and 0 <= y < N and visited[board[x][y]] == 0:
            visited[board[x][y]] = 1
            stack.append(board[x][y])
        else:
            return -1, -1
    return x,y


def search(x,y,wid,he):
    visited = [0] * 101
    stack = []
    for i in range(4):
        if i %2 == 0:
            x,y = move(x,y,he,i,stack,visited)
        elif i % 2:
            x,y = move(x,y,wid,i,stack,visited)
        if x == -1 or y == -1:
            return -1
    return len(stack)



T = int(input())
for tc in range(1,T+1):
    N = int(input())
    board = [list(map(int,input().split())) for _ in range(N)]
    di = [1,1,-1,-1]
    dj = [1,-1,-1,1]
    res = -1
    for i in range(N - 2):
        for j in range(1,N-1):
            for m in range(2,N):
                for n in range(2,N):
                    ans = search(i,j,m,n)
                    res = max(res,ans)
    print('#{} {}'.format(tc,res))   
```    
- 처음엔 dfs 재귀로 시도   
- 하지만 오른쪽아래 > 왼쪽아래 > 왼쪽위 > 그다음에 위로 계속 올라가야하는데 왼쪽 아래로 다시 내려가는 경우도 포함되어버려서 실패    
- search 함수와 move 함수를 각각 정의하여 실행 -> 성공    

<hr>     
</br></br>    


