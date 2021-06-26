## C 기초   
### -- C 언어   
###### 아주 오래되고 전통적인 텍스트 기반의 언어 &nbsp;&nbsp;&nbsp; /스크래치만큼 사용자 친화적이진 않지만, 스크래치보다 많은 기능을 사용할 수 있다.   </br>   
- 코드 작성 환경 : [CS50 샌드박스](sandbox.cs50.io)   = 리눅스로 돌아가는 클라우드 기반의 환경   </br>   

1. 기본적인 라이브러리 = include <stdio.h>   <- io는 입출력이란 뜻, 즉, printf 함수 포함     
2. include <unistd.h> 　　-> 　　sleep 함수 사용 가능, sleep(1); = 1초 쉬라는 뜻 
3. **첫 시작코드 = int main(void) { } = 스크래치의 깃발클릭 블록**   
4. (출력형태) main (입력형태) -> int main(void) = 출력형태는 int, 입력형태는 void 즉, 입력은 없다는 뜻
5. main 함수는 보통 0을 반환한다. 프로그램에서의 0은 문제가 없다는 뜻        
6. printf = 형식화된(f) 문자를 출력(print)한다.    
7.  printf("hello, world");   = hello, world 출력       

###### ※ \0 　->　 널 문자(널 종단 문자), 1 bytes(=8 bits)가 모두 0인 상태이며 문자열의 끝 표시 　
###### printf 함수는 일종의 루프를 만들어서 첫 글자부터 널 문자 여부를 확인, 널 문자 X = 출력 / 널 문자 O = 중단     
###### 즉, hello 라는 문자열은 RAM 내에 'h' 'e' 'l' 'l' 'o' '\0' 로 총 6 bytes의 공간을 차지하는 것이다.   
</br>   

### -- 컴파일러(compiler)     
###### 소스코드를 머신코드로 번역하는 알고리즘 혹은 소프트웨어   
- source code = C, 파이썬, 자바 등의 언어로 작성한 영어와 유사한 코드   
- machine code = 컴퓨터가 실제로 이해하는 0과 1의 조합   
</br>   
   
### -- 터미널 창   
###### 프롬프트 입력 후 엔터로 코드 실행   
- $ = 프롬프트/쉘이라고 부르며 입력하라는 표시    
1. clang hello.c  　　     <소스코드 파일(hello.c)을 머신코드로 컴파일한 파일(a.out) 생성   
                  　　　　　　　　 clang = 코드를 컴파일하는 프로그램의 이름     
**1-1. clang -o (머신코드 파일명1) (소스코드 파일명2) = 파일명2를 컴파일하여 파일명1 로 생성  [-o : 출력 관련 명령행 인자**]   
2. ./a.out  　　　　       < . = 현재 디렉토리, ./(파일 이름) 현재 디렉토리에 있는 파일 실행 = 그래픽 방식에서의 더블클릭   
3. 커서 줄바꿈 = 소스코드 파일에서 printf("hello,world\n")로 수정        
4. 수정하면 다시 컴파일(clang hello.c) 후 실행 (./a.out)   

</br>     

### -- 명령행 인자    
1. **int main(void)** < 에서 void 부분 대신 명령행 인자 사용 가능   
2. **int main(argc, argv[])** < 사용자에게 직접 값을 입력받아서 개수와 배열 저장     
-> argv[0] : 프로그램의 이름, argv[1] : 입력값   
3. argc = 입력값 + 1이므로 입력값을 하나만 받는다면 불리안 표현은 [argc == 2] 이고, argv[1]을 출력해야한다.    
  
- 관습적인 표현) argc = arg count = 인자 개수, argv = arg vector = 인자 벡터      
- 활용 예시) 암호화, 독해 난이도 판별 프로그램 등등   
</br>   


### -- 연산자     

 (1) % :point_right: 나머지 연산자 　　　　　　　　　(2) // :point_right: 주석 　　　　　　　(3) \n :point_right: 줄바꿈 문자 　</br>   
 (4) || :point_right: 혹은, 또는 (엔터 위 키)　　　　　　(5) && :point_right: 그리고 　　　　　(6) == :point_right: equal (문자열은 비교불가)　　 </br>   　  
 (7) ; :point_right: 마침표, 실행문 끝에 사용, 조건 or 반복문 끝은 X   　　　　　　　　 (8) = :point_right: 할당연산자, 오른쪽 내용을 왼쪽으로 지정   
 
 (9) != 👉 not equal 　　　　　　　　　(10) & 👉 ~의 주소, 변수 n의 주소 표시 : ``` [printf("%p\n", &n);] ```      
 
 (11) * 👉 & 연산자와 반대 동작으로 ~의 주소로 이동, 변수 n의 값 출력 : ``` [printf("%i\n", *&n);] ```   
 </br>    

 (1) cd (폴더명) :point_right: 폴더로 이동 　　　　　　(2) pwd :point_right: 현재 경로 표시 　(3) cd :point_right: 기본 설정 디렉토리로 이동     </br>      
 (4) mkdir :point_right: 폴더 생성　　　　　　　　　(5) rmdir :point_right: 폴더 삭제　　　(6) rm (파일명) :point_right: 삭제 　</br>     
 
 (7) ls :point_right: 현재 디렉토리의 파일 목록 표시, * 붙은 파일은 머신코드 파일      
</br>   

##### 구문 설탕 = 기능 추가 X, 기존 기능을 보기 좋고 간결하게 활용   
int counter = 0;   
counter = counter + 1; 　　-> 　　counter += 1; 　　->  　　counter ++;   
counter = counter x 2; 　　-> 　　counter * = 2;

</br>

### -- 자료형 ↔ 형식지정자      

##### 1) 문자열(string) ↔ %s  　　　(CS50 라이브러리에 포함된 자료형, C 에서는 char ```*```)   
? bytes/ 프로그래머들이 단어, 문장, 구절을 부르는 말, 숫자와는 다른 종류의 데이터, 변수 지정시 " 사용 (char 1개가 아닌 여러개이므로)      
〓 문자 배열의 첫번째 바이트 주소 (널 종단 문자에 의해 끝은 알 수 있으므로 처음만 알려주면 문자의 나열과 동일)        
C 구현 -> (1) ```typedef char*string```로 string 문자열 정의    
　　　　　or (2) ```string (변수명) = ``` 대신 ```char *(변수명) = ``` 로 표현         
##### 2) bool   
1 bytes = 8 bits/ 참,거짓의 값을 지니는 불리안 표현   
##### 3) char ↔ %c  
1 bytes = 8 bits/ 1개의 문자, yes=y, no=n 등, 변수 지정시 ' 사용   
##### 4) int ↔ %i  
4 bytes = 32 bits/ 특정 크기를 가지는 정수, 약 40억 정도까지 셈 가능      
##### 5) long ↔ %li  
8 bytes = 64 bits/ int보다 더 많은 비트를 취급하는 정수, 40억 이상의 데이터를 가진 거대 기업 등에서 사용   
##### 6) float ↔ %f (%.2f = 소수점 2자리만 표현)    
4 bytes = 32 bits/ 소수점을 갖는 실수              
##### 7) double ↔ %f   
8 bytes = 64 bits/ 소수점 뒤에 더 많은 수를 가지는 실수    
##### 8) 포인터 ↔ %p    
포인터는 컴퓨터 메모리의 주소를 가리킨다. 메모리 속 해당 변수가 저장된 위치를 출력, "%p\n", &n 처럼 연산자 &와 같이 사용     
##### 9) FILE    
파일 자료형    

##### 10) 자료형 지정   
``` 
typedef struct 
{ (자료형1) (이름1); (자료형2) (이름2); } 
(새로운 자료명);
```
-> struct 는 여러 자료형을 담기 위한 그릇   
-> 그 안에 포함된 속성값은 (새로운 자료명).(이름1)로 표현할 수 있다.   
ex) 
``` 
typedef struct   
{ string abc; string cde; }   
aph;   
```      
-> 속성값 표현 : aph.abc 또는 aph.cde 등등     


###### 추가)  char a1 = 'A';     
###### 　　　printf("%i\n", (int) a1); 　　　<- 이렇게 char를 int로 바꾸는 등의 자료형 변형 가능      
</br>      

#### ※ 전역 변수    
###### 코드 상단 / 함수 바깥에서 선언하는 변수, const (자료형) (변수명-대문자) = 값;  


#### ※ 오버플로우     
###### 컴퓨터는 하드웨어인 RAM에 데이터를 저장하기 때문에 저장용량 한계 존재 (ex. Y2K 문제 : 저장용량 한계로 1920년 -> 20년으로 저장했는데 2000년이 되자 1900=2000으로 인식되는 문제 발생)          
##### 정수 오버플로우 => int는 32비트를 사용하는데, int형인 i에 2씩 계속 곱해나가면 10억자리 이후부턴 다음 자릿수를 저장할 용량/비트가 없기 때문에 0으로 표시된다. 이것이 오버플로우이다.   
##### 부동 소수점 부정확성 => 1/10=0.1이지만 n0번째 소수점까지 나타내고자 한다면, 컴퓨터는 무한한 숫자들을 모두 저장할 수 없기 때문에 1/10에 가장 가까운 값을 저장하므로 100% 정확하진 않다.    
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

### -> C에선 get_(자료형) 함수 대신 scanf("형식지정자", 변수주소); 사용    

``` 
int x;   // 변수 x 정의   
printf("x: ");  // 사용자에게 x의 값 입력받기
scanf("%i", &x:);  // scanf함수는 형식지정자를 쓰면 그형식대로 입력받으며, 입력받은 값을 저장할 변수의 주소 작성   
printf("x: %i\n", x);  // 입력받은 값 출력    
```     
###### 포인터 변수 혹은 문자열을 정의하여 입력받는 경우, 그 자체가 주소이며 문자열의 이름을 포인터처럼 다루므로 &는 안붙여도 ok,    
```
char y[7]; // 크기 7의 문자열 y 선언, 6개의 문자 입력 가능           
printf("y: ");
scanf(%s", y);
printf("y: %s\n", y);
```

</br>   
 
### -- 파일쓰기      
#### FILE ```*```변수이름 = fopen("파일 이름", "r, w, a"); // r 읽기, w 쓰기, a 덧붙이기    
#### fprintf(변수명, "형식지정자\n", 출력할 변수);   
``` 
FILE *file = fopen("파일명.csv", "a");  //fopen 함수를 이용하면 파일을 FILE 자료형으로 불러오기 가능
char *name = get_string("Name: ");   // 사용자에게 값 입력받기
char *number = get_string("Number: ");   
fprintf(file, "%s,%s\n", name, number);  // 파일에 값 출력
fclose(file);  // 파일 닫기
```
- fopen은 해당 파일을 가리키는 포인터 반환           
- scanf 함수를 사용해야하지만 간단하게 get_string 함수로 사용자에게 값 입력 받기   
- fprintf 함수는 파일용 printf로 파일에 출력 가능               
- 맨 마지막엔 fclose(변수이름); 로 파일 닫기        

</br>   

### -- 파일읽기      
#### fread(배열명, 읽을 바이트수, 읽을 횟수, 읽을 파일);   
```
int main(int argc, char *argv[])   // 명령행 인자로 사용자에게 값을 입력받아 그 값을 배열에 저장하는 것   
{ if (argc != 2)  
  { return 1; }   
  FILE *file = fopen(argv[1], "r");
  if (file == NULL)   // fopen이나 malloc 등의 함수는 오류일 때 NULL 출력하므로      
  { return 1; }   
  unsigned char bytes[3];   // 크기 3인 문자배열 만들기 
  fread(bytes, 3, 1, file);   // 파일의 첫 3바이트(24bits)를 읽어오기   
  if (bytes[0] == 0xff && bytes[1] == 0xd8 && bytes[2] == 0xff)
  { printf("Maybe\n"); }
  else
  { printf("No\n"); }
  ```
- argv[0] = 프로그램 이름, argv[1] = 사용자 입력값이므로 argc가 2가아니면 1개를 입력한 것이 아니라는 뜻     
- unsigned : -128 ~ 127 까지가 아닌 0 ~ 255까지의 범위 값 의미     
- 굳이 3바이트 읽는 이유 : jpeg 개발자들이 정한 일종의 매직 넘버가 있어 모든 jpeg 파일의 첫 3 바이트는 FF, D8, FF로 시작하므로 jpeg 파일 유무 판단 가능   

</br>   


### --조건문    
#### 1. 조건 2개   
```
if ( 조건 ) 
{ 실행문 ex) printf함수 등 ;　}　   　        
else   
{ 실행문 ex) printf함수 등 ;　}　    
```
#### 2. 조건 3개    
```
if ( 조건 ) 
{ 실행문 ex) printf함수 등 ;　}　   　        
 else if ( 조건 )     
{ 실행문 ex) printf함수 등 ;　}　        
 else if ( 조건 ) 또는 else      
{ 실행문 ex) printf함수 등 ;　}      
```
</br>   

### --루프    
#### 1. 무한 루프   
```
while (true) 혹은 while ()  
{ 실행문 ex) printf함수 등 ;　}        
```
#### 2-1. n번 반복 while 루프   
``` 
int i = 0;   
while (i < n)   
{ 실행문 ex) printf함수 등 ;　   
i++; }
``` 
#### 2-2. n번 반복 for 루프   
``` 
for (int i = 0; i < 50; i++)　　　　<- for(변수지정; 계속 물어볼 불리안 표현; 변수 수정방법)       
{ 실행문 ex) printf함수 등 ;　}     
``` 
#### 3. do-while 루프 　　　　　　　<- while, for루프와 달리 무조건 한 번은 실행한 후 불리언 표현이 참일 때 반복 결정     
``` 
int n;   
do   
{ n = 실행문 ; }   
while (불리언 표현) ;   
return n; 또는 for (int i=0; i < n; i++) + printf("\n");  등 　　　　　<- printf("\n"); 는 새로운 한 줄 출력     
```

#### 4. 중첩 루프 　　　　　　　<- 2차원 행/열 구현 가능, n x n 블록    
``` 
do-while 루프   
for (int i=0; i < n; i++)   
{ for (int j=0; j < n; j++)   
 　　{ 실행문 ; }   
 　　　printf("\n") ;   　　　}  　　　　　<- 내부 루프 끝날때마다 줄바꿈을 출력하므로 j = 가로, i = 세로     
 ``` 

#### 5. 문자열 순환 루프 (기본)      
##### 문자열 s 정의 이후     
```
for (int i = 0, n = strlen(s); i < n + 1; i++)   　　　         
 { 실행문 }        
 ``` 
##### - i < n + 1 인 이유는 널 종단 문자를 포함하기 때문, 필요없다면 n 까지만.     
##### - 실행문에 t[i] = s[i] 를 작성하면 s를 t에 복사하는 문자열 복사로, strcpy 함수를 이용하여 strcpy(t,s)로 간단히 작성할 수 있다.   

</br>   

### --사용자 정의 함수   
#### (1) void (함수명)(void) 　　　　　<- 입력 값이 없는 정의 함수의 프로토타입     
##### void = get_string 등의 함수와 달리 값을 반환하지 않는다. 즉, 값을 입력하지 않는다.         
1.  void (함수명)(void) 　　　　　<- 함수 정의/ 추상화의 개념        
   { 실행문 ex> printf 함수 등 ; }   
2.  int main(void) 　　　　　　　<- 시작 부분     
   { (함수명)(); } 　　　　　　　<- 루프문 추가 가능      
   
#### (2) void (함수명)(int n) 　　　　　<- 입력값이 있는 정의 함수의 프로토타입     
1.  void (함수명)(int n) 　　　　　<- 함수 정의/ 매개 변수화      
   { 실행문 ex> for 루프문 or printf 함수 등; }   
2.  int main(void) 　　　　　　　<- 시작 부분     
   { (함수명)(3); } 　　　　　　　<- n=3 대입의 뜻       

#### (3) float average (int length, int array[]) 　　　 
##### 반환값 = float/ 함수명 = average/ 입력값 = (배열 길이, 배열명) 인 함수/ ex) float average (n,scores) <- socres[]로 정의된 배열      
1. int sum = 0;
2. for (int i = 0; i < length; i ++)   
　　{ sum += array[1]; }   
3. return (float) sum / (float) length;   

##### main 함수가 맨 위에 있는 것이 잘 디자인 된 것이므로 함수 정의를 밑으로 내리고, 함수 프로토타입만 main 함수 위에 작성한다.   

</br>   

### 라이브러리    
<details>
   <summary><b>(1) string.h</b></summary>
   <div markdow="1">   
      
- C 언어의 표준 라이브러리이며 메모리 블록이나 문자열을 다룰 수 있는 함수 포함     
      
1. memcpy(k,const n,size_t j) = n의 j 바이트만큼을 k에 복사한다.   
2. memmove(k,const n,size_t j) = n의 j 바이트만큼을 k로 옮긴다.   
3. char strcpy(char k, const char n) = n을 k에 복사한다.   
4. char strncpy(char k, const char n, size_t j) = n의 처음 j개 문자를 k로 복사한다.      

5. strcpy (k, const n) = n을 k뒤에 붙인다.   
6. strncat (k,n,size_t j) = n의 j개 문자를 k뒤에 붙인다.   

7. memcmp (k,n,size_t j) = k의 j바이트 데이터와 n의 j바이트 데이터를 비교한다.     
8. strcmp (k,n) = k와 n을 비교한다.   
9. strcoll (k,n) = LC_COLLATE에 정의된 방식에 따라 해석된후 비교한다.   
10. strncmp (k,n,size_t j) = k의 j개 문자와 n의 j개 문자를 비교한다.   
11. strxfrm (k,const n,size_t j) = 현재 지역 정보에 따라 n의 문자열을 변환하고 j개 문자를 k에 복사한다.    

12. memchr(const k, n, size_t j) = k의 메모리의 처음 j바이트 중 처음으로 n과 일치하는 문자의 주소를 반환한다.   
13. strchr(const k, n) = k에서 처음으로 n과 일치하는 문자의 주소를 반환한다.
14. strcspn(const k, const n) = k에서 n의 문자들을 찾아 처음으로 일치하는 문자의 주소를 반환한다.
15. strpbrk(const k, const n) = k에서 n의 문자를 찾아 n에서 k와 처음으로 일치하는 문자의 주소를 반환한다.
16. strrchr(const k, n) = k에서 마지막으로 n과 일치하는 문자의 주소를 반환한다.
17. strspn(const k, const n) = k에서 n의 문자가 아닌 문자가 처음으로 나오는 주소를 반환한다.
18. strstr(const k, const n) = k에서 n을 검색하여 처음 나타난 주소를 반환한다.
19. strtok(k, const n) = k를 n 문자로 분리한다, n에 해당하는 문자가 NULL로 변경 (ex. k = 12&345, const n = & 일때, 출력하면 12 345)     
      
19-1. strtok_r(k, const n, j) 👉 strtok()과 동일한 기능, 다른점은 j로 찾는 위치를 관리하므로 중복 사용 가능. 즉, thread-safe      
      
20. strsep(k, const n) = k를 n문자로 분리한다. n 문자가 처음이나 마지막에 위치하거나 연속될 경우, strtok()는 무시, strsep()는 공백으로 분리     
　　　　　　　(ex. k = &12&&345&789&, const n = & 일때, strsep() = (공백) 12 (공백) 345 789 (공백) )   

21. memset(k, n, size_t j) = k의 메모리의 처음 j바이트를 n으로 채운다.   
22. strerror(k) = k을 해석한 후 그에 해당하는 에러 문자열의 포인터를 반환한다.   
23. strlen(const k) = k의 길이를 반환한다.   
   </div>
   </details>
   
   </br>   
<details>   
   <summary><b>(2) ctype.h</b></summary>   
   <div markdown="2">   
      
- C 언어의 표준 라이브러리이며, 문자들을 조건에 맞는지 검사하고 변환하는 함수 포함   
      
1. int is_____(int c) = c가 _______이면 0이 아닌 값 반환
      - alnum = 알파벳 or 숫자    
      - alpha = 알파벳   
      - cntrl = 제어 문자   
      - digit = 숫자    
      - graph = 그래픽 문자   
      - lower = 소문자   
      - print = 출력할 수 있는 문자   
      - punct = 구두점 문자   
      - space = 공백 문자   
      - upper = 대문자   
      - xdigit = 16진 숫자   
 
2. int to_____(int c) = c를 _____로 변환   
      - lower = 소문자   
      - upper = 대문자   
   int __toascii(int c) = c를 아스키 코드로 변환     
      </div>   
   </details>   
   
   </br>   
   

      
      
      
      
 


