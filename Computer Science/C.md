## C 기초   
### -- C 언어   
###### 아주 오래되고 전통적인 텍스트 기반의 언어 &nbsp;&nbsp;&nbsp; /스크래치만큼 사용자 친화적이진 않지만, 스크래치보다 많은 기능을 사용할 수 있다.   </br>   
코드 작성 환경 : [CS50 샌드박스](sandbox.cs50.io)   = 리눅스로 돌아가는 클라우드 기반의 환경   </br>   

1. 기본적인 라이브러리 = include <stdio.h>   <- io는 입출력이란 뜻, 즉, printf 함수 포함   
2. **첫 시작코드 = int main(void) { } = 스크래치의 깃발클릭 블록** 
3. printf = 형식화된(f) 문자를 출력(print)한다.    
4.  printf("hello, world");   = hello, world 출력   

</br>   

### -- 컴파일러(compiler)     
###### 소스코드를 머신코드로 번역하는 알고리즘 혹은 소프트웨어   
source code = C, 파이썬, 자바 등의 언어로 작성한 영어와 유사한 코드   
machine code = 컴퓨터가 실제로 이해하는 0과 1의 조합   
</br>   
   
### -- 터미널 창   
###### 프롬프트 입력 후 엔터로 코드 실행   
$ = 프롬프트/쉘이라고 부르며 입력하라는 표시    
1. clang hello.c  　　     <소스코드 파일(hello.c)을 머신코드로 컴파일한 파일(a.out) 생성   
                  　　　　　　　　 clang = 코드를 컴파일하는 프로그램의 이름     
**1-1. clang -o (머신코드 파일명1) (소스코드 파일명2) = 파일명2를 컴파일하여 파일명1 로 생성  [-o : 출력 관련 인자 (명령행 인자)**]   
2. ./a.out  　　　　       < . = 현재 디렉토리, ./(파일 이름) 현재 디렉토리에 있는 파일 실행 = 그래픽 방식에서의 더블클릭   
3. 커서 줄바꿈 = 소스코드 파일에서 printf("hello,world\n")로 수정        
4. 수정하면 다시 컴파일(clang hello.c) 후 실행 (./a.out)   

</br>     

### -- 연산자     

 (1) % :point_right: 나머지 연산자 　　　　　　　　　(2) // :point_right: 주석 　　　　　　　(3) \n :point_right: 줄바꿈 문자 　</br>   
 (4) || :point_right: 혹은, 또는 (엔터 위 키)　　　　　　(5) && :point_right: 그리고 　　　　　(6) == :point_right: equal 　　 </br>   　  
 (7) ; :point_right: 마침표, 실행문 끝에 사용, 조건 or 반복문 끝은 X   　　　　　　　　 (8) = :point_right: 할당연산자, 오른쪽 내용을 왼쪽으로 지정   </br>   

 (9) cd (폴더명) :point_right: 폴더로 이동 　　　　　　(10) pwd :point_right: 현재 경로 표시 　(11) cd :point_right: 기본 설정 디렉토리로 이동     </br>      
 (12) mkdir :point_right: 폴더 생성　　　　　　　　　(13) rmdir :point_right: 폴더 삭제　　　(14) rm (파일명) :point_right: 삭제 　</br>     
 
 (15) ls :point_right: 현재 디렉토리의 파일 목록 표시, * 붙은 파일은 머신코드 파일      
</br>   

### -- 자료형 ↔ 형식지정자      

##### 1) 문자열(string) ↔ %s  
프로그래머들이 단어, 문장, 구절을 부르는 말, 숫자와는 다른 종류의 데이터    
##### 2) bool   
참/거짓의 값을 지니는 불리안 표현   
##### 3) char ↔ %c  
1개의 문자, yes=y, no=n 등   
##### 4) int ↔ %i  
특정 크기를 가지는 정수, 약 40억 정도까지 셈 가능      
##### 5) long ↔ %li  
int보다 더 많은 비트를 취급하는 정수, 40억 이상의 데이터를 가진 거대 기업 등에서 사용   
##### 6) float ↔ %f (%.2f = 소수점 2자리만 표현)    
소수점을 갖는 실수   
##### 7) double ↔ %f   
소수점 뒤에 더 많은 수를 가지는 실수   
</br>    

### -- get_(자료형) 함수 (CS50 라이브러리)    
##### get_string, get_float, get_double ...etc
###### 스크래치의 ask 함수 = C의 get_string 함수   

#### get_string 함수 및 string 변수 등을 사용하기 위해선, 라이브러리 cs50.h연결 혹은 make 명령어 사용   
1. include <cs50.h>  　　　 <- cs50 라이브러리 로드   
   1-1. cs50 라이브러리 연결 : clang -o string string.c -lcs50    <- '-l'은 연결 의미   
   1-2. make 명령어 사용 : make (프로그램명) 입력   <- make string = string.c를 string으로 컴파일  
#### string answer = get_string("What's your name?\n");      
1. get_string 함수의 값을 저장할 변수 지정 -> answer = get_string("What's your name?\n");   
2. 변수(answer)가 저장하는 값의 종류가 문자열이라고 지정 -> string answer   
#### printf("hello,%s\n",answer);   
1. printf함수는 " "사이에 있는 하나의 문자열을 입력받는다   
2. printf 함수의 " " 사이에 형식지정자가 있다면 쉼표와 변수명을 같이 입력하여 어디서 값을 가져올지 지정    
3. printf("hello,%s,%s\n",answer1,answer2); -> 변수 두 개를 왼쪽부터 오른쪽으로 순서대로 지정   
</br>   

##### 구문 설탕 = 기능 추가 X, 기존 기능을 보기 좋고 간결하게 활용   
int counter = 0;   
counter = counter + 1; 　　-> 　　counter += 1; 　　->  　　counter ++;   

</br>   

### --조건문    
#### 1. 조건 2개   
##### if ( 조건 ) 
{ 실행문 ex) printf함수 등 ;　}　   　        
##### else   
{ 실행문 ex) printf함수 등 ;　}　        
#### 2. 조건 3개    
##### if ( 조건 ) 
{ 실행문 ex) printf함수 등 ;　}　   　        
##### else if ( 조건 )     
{ 실행문 ex) printf함수 등 ;　}　        
##### else if ( 조건 ) 또는 else      
{ 실행문 ex) printf함수 등 ;　}   
</br>   

### --루프    
#### 1. 무한 루프   
##### while (true)   
{ 실행문 ex) printf함수 등 ;　}     
#### 2-1. n번 반복 while 루프   
###### int i = 0;   
##### while (i < n)   
###### { 실행문 ex) printf함수 등 ;　   
##### 　i++; }     
#### 2-2. n번 반복 for 루프   
##### for (int i = 0; i < 50; i++)　　　　<- for(변수지정; 계속 물어볼 불리안 표현; 변수 수정방법)       
##### { 실행문 ex) printf함수 등 ;　}   



