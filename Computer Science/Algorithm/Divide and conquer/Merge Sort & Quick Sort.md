## 분할 정복 기법

- 설계 전략

  - 분할 
    - 해결할 문제를 여러 개의 작은 부분으로 나누기
  - 정복
    - 나눈 작은 문제(부분 문제)를 각각 해결
  - 통합
    - 해결된 해답을 모아서 전체 문제의 해를 구한다  

- 문제 종류   

  - 거듭 제곱 

    - 시간 복잡도 : O(log2n)

    - C^n = C^(n/2) * C^(n/2)    (n 홀수)

      C^n = C^(n-1/2) * C^(n-1/2) * C    (n 짝수)

    ```python
    def power(x,n):
        if n == 1:
            return x
        if n % 2 == 0:
            y = power(x, n/2)
            return y*y
        else:
            y = power(x, (n-1)/2)
            return y*y*x
    ```

- 활용

  - 외부 정렬의 기본이 되는 알고리즘
  - 멀티코어 CPU나 다수의 프로세서에서 정렬 알고리즘을 병렬화하기 위해 병합 정렬 알고리즘 활용
  - 퀵 정렬은 매우 큰 입력 데이터에 대해 좋은 성능을 보이는 알고리즘   

</br>    

### 병합 정렬 (Merge Sort)

- 여러개의 정렬된 자료의 집합을 병합하여 한 개의 정렬된 집합으로 만드는 방식   

- 자료를 최소 단위의 문제까지 나눈 후에 차례대로 정렬하여 최종 결과 얻어냄    

- top - down 방식

- 시간 복잡도 : O(n logn)   

- 과정

  1. 전체에 대해 최소 크기의 부분집합이 될때까지 분할 작업    
     - 구현 과정에서, 실제로 부분집합을 만들어 저장하는 경우와, 인덱스로만 조작하는 경우로 나뉜다.
     - 인덱스로만 조작하는 쪽이 메모리 과다사용을 막아줄 수 있다.    
  2. 병합 단계 
     - 2개의 부분집합을 정렬하면서 하나의 집합으로 병합  
     - 1개로 병합될때까지 반복   

  ```python
  # 실제로 부분집합을 만들어 메모리에 저장하는 방식 -> 메모리가 많이 필요하다는 단점 존재
  
  def merge_sort(m):
      if length(m) == 1:
          return m
      left = []
      right = []
      middle = length(m)//2
      for x in m[:middle]:
          left.append(x)
      for x in m[middle:]:
          right.append(x)
      left = merge_sort(left)
      right = merge_sort(right)
      
      return merge(left,right)
      
  def merge(left, right):
      result = []
      while length(left) > 0 or length(right) > 0:
          if length(left) and length(right):
              if first(left) <= first(right):
                  result.append(popfirst(left))
              else:
                  result.append(popfirst(right))
          elif length(left):
              result.append(popfirst(left))
          elif length(right):
              result.append(popfirst(right))
      return result
                  
  ```

</br>   

### 퀵 정렬   

- 주어진 배열을 두 개로 분할하고 각각을 정렬   

  - 병합정렬은 그냥 2부분으로 나누는 반면, 퀵 정렬은 분할할때, 기준 아이템(피봇값) 중심으로 이보다 작은것은 왼쪽, 큰 것은 오른쪽에 위치시킨다.   
  - 정렬 이후 병합정렬은 병합이란 후처리 작업이 필요하나, 퀵 정렬은 필요없다.   

  ```python
  # A 배열, L 왼쪽 끝 인덱스, r 오른쪽 끝 인덱스
  def quicksort(A,L,r):
      if L < r:
          s = partition(A, L, r)
          quicksort(A,L,s-1)
          quicksort(A,s+1,r)
  
  # 피봇값을 어떻게 정하냐에 따라 알고리즘 구현은 달라짐   
  
  # Hoare-Partition
  def partition(A,L,r):
      p = A[L]	# 피봇값 = 제일왼쪽 값
      i = L
      j = r
      while i <= j:
          while i <= j and A[i] <= p :
              i +=1						# 피봇보다 큰 값 만날때까지 i 증가
          while i <= j and A[j] >= p :
              j -= 1						# 피봇보다 작은 값 만날때까지 j 감소
          if i < j:						# i,j가 모두 멈추고, i<j라면 교환 
              A[i], A[j] = A[j], A[i]
              
      A[L], A[j] = A[j], A[L]  	# i와 j가 교차한다면, j인덱스와 피봇값 교환
      return j  
  
  
  # Lomuto partition 
  def partition(A,p,r):
      x = A[r]		# 피봇값 = 제일오른쪽 값
      i = p-1
      for j in range(p,r):
          if A[j] <= x:
              i += 1
              A[i], A[j] = A[j] , A[i]
      A[i+1],A[r] = A[r],A[i+1]
      
      return i + 1
      
  ```

  - 피봇값들보다 큰 값은 오른쪽, 작은 값은 왼쪽 집합에 위치하도록 한다.
  - 피봇을 두 집합의 가운데에 위치시킨다.    

- 피봇 선택   

  - 왼쪽끝/ 오른쪽 끝/ 임의의 세개 값 중 중간 값    
  - 일반적으로 중간값 선택하는 것이 재귀깊이를 생각할 때 효율적    
    - 중간값이면 왼쪽, 오른쪽 깊이가 균일해질 수 있음       

