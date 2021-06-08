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


