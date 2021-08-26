### DP (Dynamic Programming)

###### 동적 계획 알고리즘은 그리디 알고리즘같이 최적화 문제를 해결하는 알고리즘     

- `입력 크기가 작은` 부분 문제들을 모두 해결한 후, `해당 해를 이용`하여 보다 큰 크기의 문제를 해결하여 `최종적으로 원래 입력의 문제` 해결    
- 과정    
  - 1. 문제를 부분 문제로 분할     
    2. 가장 작은 부분 문제부터 해 도출   
    3. 해당 결과를 테이블에 저장 -> 저장된 해를 이용하여 상위문제의 해 도출        

- 피보나치 수 DP 적용 -> 부분 문제의 답에서 본 문제의 답을 얻을 수 있어, 최적 부분 구조로 구성    

  - 메모이제이션과의 차이점은 재귀가 아닌 반복 구조 사용한다는 점     

  - 메모에제이션처럼 재귀 사용하는 구조를 하향식 DP라고도 함      
  - 메모이제이션을 재귀적 구조에 사용하는 것보단 **반복 구조로 DP를 구현하는 것이 더 효율적**     
    - 재귀적 구조는 내부에 시스템 호출 스택을 사용하는 오버헤드가 발생하기 때문      

```python
# 피보나치 수 DP 적용 1
def fibo(n) :
    stack = [0,1]
    for i in range(2, n+1):
        stack.append(stack[i-1] + stack[i-2])   
    return stack[n]

n = int(input())
table = [0]*(n+1)

# 피보나치 수 DP 적용 2
def fibo(n) :
    table[0] = 0
    table[1] = 1
    for i in range(2, n+1):
        table[i] = table[i-1] + table[i-2]  
    return table[n]

n = int(input())
table = [0]*(n+1)

# 팩토리얼 DP 적용
def fact(n):
    table[0] = 1
    for i in range(1, n+1):
        table[i] = i * table[i-1]
    return table[n]

n = int(input())
table = [0] * (n+1)

```

