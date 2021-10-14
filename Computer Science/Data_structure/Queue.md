## 큐(Queue)

- 스택과 마찬가지로 삽입,삭제의 위치가 제한적인 자료구조

  - 뒤에서만 삽입하고, 스택과는 반대로 앞에서만 삭제     

- 선입선출구조(FIFO : First in First out)     

  - 삽입 순서대로 저장되어 가장 먼저 저장된 원소가 가장 먼저 삭제     

- 머리(Front)와 꼬리(Rear), 2개의 인덱스 관리  <-> 스택은 top, 1개의 인덱스 관리        

  - Front는 저장된 원소 중 첫 번째 원소 (or 데이터를 꺼낸 위치)    
  - Rear는 저장된 원소 중 마지막 원소 (or 데이터를 저장할 위치)    

- 큐의 연산    

  - 삽입 - enQueue(A)  -> 줄임말 : enQ 

    ```python
    if isFull():   # 포화상태인지 확인 
    	print('Full')
    else :
    	rear += 1 # (저장할 위치 확보)
    	Q[rear] = 'A'      #enQ  (주석으로 삽입 나타내기)
    ```

  - 삭제 - deQueue  -> 줄임말 : deQ    

    ```python
    if isEmpty():   # 공백상태인지 확인
    	print('Empty')
    else:
    	front += 1 # (꺼낼 위치/ 삭제할 자리 준비)
    	return Q[front]
    
    while not isEmpty(): # 로 다 비울때까지 출력(삭제) 가능
    while front != rear:
    ```

  - 큐 생성 - createQueue()    

    ```python
    Q = [0] * 100
    front = rear = -1 로 초기화    
    ```

  - 공백 확인 - isEmpty()  : 비어있으면 True   

    ```python
    return front == rear
    ```

  - 포화 확인 - isFull()    

    ```python
    return rear == len(Q) -1
    ```

  - front에서 원소 삭제없이 반환  - Qpeek()      

    ```python
    if isEmpty():   # 공백상태인지 확인
        print('Empty')
    else :
        return Q[front+1]	# 검색해서 반환하므로 front에 직접적으로 연산하진 않음
    ```

- 선형 큐

  - 1차원 배열 이용    

  - 큐의 크기 = 배열의 크기

  - 상태 표현

    - 초기 상태 : front = rear = -1
    - 공백 상태 : front = rear
    - 포화 상태 : rear = n-1 

  - 발생 가능한 오류    

    - 잘못된 포화 상태 인식    

      - 삽입, 삭제를 계속 반복할 경우, rear = n-1 이 되고, 배열의 앞부분에 활용 가능한 공간이 있음에도 포화상태로 인식하여 더이상의 삽입을 수행하지 않게 됨     

      -> **해결 방법1**    

      	- 매 연산이 이루어질 때 마다 저장된 원소들을 앞으로 이동
      	- but, 원소 이동에 많은 시간이 소요되어 큐의 효율성 감소

      -> **해결 방법2**

       - 1차원 배열을 사용하지만, 논리적으론 배열의 처음과 끝이 연결된  원형 형태의 큐를 이룬다고 가정      

- 원형 큐    

  - 초기 공백 상태

    - front = rear = 0

  - index 순환    

    - front, rear의 위치가 n-1을 가리킨 후, 그 다음엔 논리적 순환을 이루어 배열의 처음 인덱스인 0으로 이동    

      ->나머지 연산자 **mod** , **%** 사용 

  - front 변수    

    - 공백, 포화 상태 구분을 위해 front 자리는 항상 빈자리     

  - 공백 확인

    - 선형 큐와 동일, front == rear인 경우

  - 포화 확인

    - 삽입할 rear의 다음 위치가 현재의 front

  |         | 삽입 위치               | 삭제위치                | 포화 확인               |
  | ------- | ----------------------- | ----------------------- | ----------------------- |
  | 선형 큐 | rear = rear + 1         | front = front + 1       | rear == n -1            |
  | 원형 큐 | rear = (rear + 1) mod n | front = (front+1) mod n | front == (rear+1) mod n |

  

  - 활용
    - 데이터 보존이 필요하다면, 여러 개의 선형 큐를 사용하면서 포화상태인 큐는 다른 곳으로 옮기기도 함
    - 덮어써도 된다면, 원형 큐를 사용하면서 맨 앞의 원소에 덮어씌우면서 저장하기도 함

- 연결 큐    

  - 선형 큐를 저장할 연속된 공간이 마땅치 않을 경우 등에 사용        

  - 단순 연결 리스트(Linked List)를 이용한 큐   

    - 큐의 원소 : 단순 연결 리스트의 노드
    - 큐의 원소 순서 : 노드의 연결 순서, 링크로 연결
    - front : 첫번째 노드를 가리키는 링크
    - rear : 마지막 노드를 가리키는 링크

  - 상태표현

    - 초기 상태 : front = rear = null
    - 공백 상태 : front = rear = null

  - 구현

    ```python
    class Node:
        def __init__(self, item, n=None):
            self.item = item
            self.next = n      # 다음 노드를 가리키는 링크
            
    def enQueue(item):
        global front, rear
        new = Node(item)	# 새 노드 생성
        if front == None:    # 큐 공백
            front = new		 # front에 new 삽입
        else:					# 큐가 공백이 아니리면
            rear.next = new		# 다음 노드를 가리키는 링크에 new
        rear = new
        
        
    ```

- 우선순위 큐 (Priority Queue)

  - 우선순위를 가진 항목들을 저장     

  - FIFO 순서가 아니라 우선순위 높은 순서대로 먼저 나감      

  - 적용 분야   

    - 시뮬레이션 시스템
    - 네크워크 트래픽 제어
    - 운영체제의 테스크 스케줄링     

  - 구현

    - 배열 이용

      - 원소 삽입하는 과정에서 우선순위 비교해서 적절한 위치에 삽입   

      - 가장 앞에 최고 우선순위 원소 위치     

      - 문제점

        - 배열을 사용하므로 삽입,삭제 연산을 사용할 때, 원소의 재배치가 발생하여 시간과 메모리 낭비가 심함      

          -> 추후 배열+트리 를 이용한 힙을 사용하면 속도 문제 해결 가능      

    - 리스트 이용

- 큐의 활용 - 버퍼 (Buffer)

  - 데이터를 다른 곳으로 전송하는 동안 임시로 데이터를 보관하는 메모리 영역       
  - 버퍼링 : 버퍼를 활용하는 방식 or 버퍼를 채우는 동작      
  - 자료구조 
    - 입출력 및 네트워크 관련된 기능에서 이용      
    - 순서대로 입,출력/전달되어야하므로 FIFO방식의 큐 활용     
