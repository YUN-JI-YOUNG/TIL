## 알고리즘   

### 1. 검색 알고리즘   
#### 1-1. 선형 탐색(=linear search)      
##### 배열의 인덱스를 처음부터 하나씩 증가시키면서 탐색하는 방법     
###### - 효율성 : O(n), Ω(1)
의사코드 예시 >   
for i from 0 to n-1    
　if i번째 요소 = 50   
  　　return true   
return false      

실제코드 예시 >   
```
int main(void)   
{ int numbers[]={1,4,6,3,18,2,50};   
for(int i = 0; i<7; i++)
{
  if(numbers[i]==50)   
    {
      printf("Found\n");
      return 0;
    }
 }
 printf("Not Found\n");
 return 1;
}
```

</br>


#### 1-2. 이진 탐색(=binary serach)      
##### 배열이 정렬되어있다면, 중간부터 시작하여 찾고자하는 값과 비교해가며 작은 값 혹은 큰 값이 저장되어있는 쪽으로 탐색하는 방법     
###### - 효율성 : O(log n), Ω(1)
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
 
 ### 2. 정렬 알고리즘    
 #### 2-1. 버블 정렬(=bubble sort)            
 ##### 큰 숫자가 올바른 위치로 갈 때까지 움직이는 정렬 방법      
 ###### - 효율성 : O(n^2), Ω(n^2)   
 의사코드 예시 >   
 Repeat n-1    
 　For i from 0 to n-2   
   　　If i'th and i+1'th elements out of order       
 　Swap them      
 -> 바깥쪽 반복문은 최대 n-1번 실행, 안쪽 반복문도 최대 n-1번 실행(0 to n-2)이므로 (n-1) x (n-1) = n^2+2n+1 = O(n^2)   
 -> n의 값이 커질수록 n^2의 영향력이 커지므로   
 
 </br>   
 
 #### 2-2. 선택 정렬(=selection sort)      
 ##### 매번 목표를 세워 가장 작은 값을 찾아 1번째 숫자와 바꾸고, 그 다음 작은 값을 찾아 2번째의 숫자와 바꾸는 것을 반복하는 정렬 방법   
 ###### - 효율성 : O(n^2), Ω(n^2)       
 의사코드 예시 >     
 Repeat n       
 　For i from 0 to n-1   
 　　Find smallest item between i'th item and last item   
  　Swap smallest item with i'th item      
 -> 제일 적은 숫자를 찾는데 n번, 그 다음 작은 숫자 찾는데 n-1번...이므로 n+(n-1)+(n-2)+...+1 = n(n+1)/2 = (n^2)/2 + n/2 = O(n^2)      
   
 </br>   
 
 
 
 ### 알고리즘의 효율성 표기법      
 ###### ※ 실행 시간 = 프로그램이나 알고리즘이 동작하는 데 걸리는 시간, 몇 초 혹은 몇 번의 계산 과정이 필요한지를 뜻함   
 #### 2-1. 빅 오 표기법(Big-O)     
 ##### 알고리즘을 수행하는 데 필요한 시간의 상한선 의미          
 괄호 안 = 실행시간 / 아래로 갈수록 더 빠른 알고리즘   
 1. O(n^2) = 버블 정렬, 선택 정렬         
 2. O(n log n)   
 3. O(n) = 선형 검색        
 4. O(log n) = 이진 탐색     
 5. O(1)     

-> 크기가 어마어마하게 크다면 2로 나누는 것 정도는 티가 안날 것이므로 O(n) = O(n/2)로 취급하며, O(log n)도 밑은 신경 X      
     

#### 2-2. Big-Ω    
##### 알고리즘을 수행하는 데 필요한 시간의 하한선 의미   
1. Ω(n^2) = 버블 정렬, 선택 정렬        
2. Ω(n log n)   
3. Ω(n)         
4. Ω(log n)   
5. Ω(1) = 선형 검색, 이진 탐색 -> 운이 좋다면 1번에 바로 찾을 수 있으므로.       

###### ※ O(n) = Ω(n) 인 예시 = 총 갯수 세기. 최선의 방법으로 세더라도 모두 확인하는 수 밖에 없으므로.   



