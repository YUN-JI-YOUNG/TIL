## 알고리즘   

### 1. 선형 탐색   
##### 배열의 인덱스를 처음부터 하나씩 증가시키면서 탐색하는 방법    
의사코드 예시 >   
for i from 0 to n-1    
　if i번째 요소 = 50   
  　　return true   
return false

</br>


### 2. 이진 탐색(=binary serach)      
##### 배열이 정렬되어있다면, 중간부터 시작하여 찾고자하는 값과 비교해가며 작은 값 혹은 큰 값이 저장되어있는 쪽으로 탐색하는 방법   
ex) 분할 정복 기법    
의사코드 예시 >   
if no items  
　return false      
if middle item = 50     
　return true   
if middle item > 50   
　search left half    
if middle item <50    
　search right half    
 
 </br>   
 
 ### 3. 빅 오 표기법(Big-O)     
 ##### 알고리즘을 수행하는 데 필요한 시간의 상한선 의미   
 ##### 코드, 알고리즘의 효율성 = 코드 속도의 대략적인 추정값을 빅오 표기법으로 표시, O(실행시간)       
 아래에 있을 수록 더 빠른 알고리즘   
 1. O(n^2)   
 2. O(n log n)   
 3. O(n) = 선형 검색     
 4. O(log n) = 이진 탐색     
 5. O(1)     

-> 크기가 어마어마하게 크다면 2로 나누는 것 정도는 티가 안날 것이므로 O(n) = O(n/2)로 취급하며, O(log n)도 밑은 신경 X      

###### ※ 실행 시간 = 프로그램이나 알고리즘이 동작하는 데 걸리는 시간, 몇 초 혹은 몇 번의 계산 과정이 필요한지를 뜻함         


