## C 기초   
### -- C 언어   
###### 아주 오래되고 전통적인 텍스트 기반의 언어 &nbsp;&nbsp;&nbsp; /스크래치만큼 사용자 친화적이진 않지만, 스크래치보다 많은 기능을 사용할 수 있다.   </br>   
코드 작성 환경 : [CS50 샌드박스](sandbox.cs50.io)   = 리눅스로 돌아가는 클라우드 기반의 환경   </br>   

1. 기본적인 라이브러리 = include <stdio.h>   <- io는 입출력이란 뜻, 즉, printf 함수 포함   
2. **첫 시작코드 = int main(void) { } = 스크래치의 깃발클릭 블록** 
3. printf = 형식화된(f) 문자를 출력(print)한다.  
4. ; = 마침표  
5.  printf("hello, world");   = hello, world 출력   

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
3. 커서 줄바꿈 = 소스코드 파일에서 printf("hello,world\n")로 수정    < \n = 줄바꿈 문자    
4. 수정하면 다시 컴파일(clang hello.c) 후 실행 (./a.out)   

6. ls = list, 현재 폴더나 디렉토리에 있는 파일 목록 표시, 이름 뒤에 * 가 붙은 파일은 머신코드 파일   
7. rm (파일명) = 삭제 > 삭제할거냐고 물어보면 y혹은 yes로 대답   
8. mkdir = 폴더 생성    
9. rmdir = 폴더 삭제   

</br>   

### -- 문자열(string)   
###### 프로그래머들이 단어, 문장, 구절을 부르는 말, 숫자와는 다른 종류의 데이터   

스크래치의 ask 함수 = C의 get_string 함수   
#### get_string 함수 및 string 변수 등을 사용하기 위해선, 라이브러리 cs50.h연결 혹은 make 명령어 사용   
1. include <cs50.h>  　　　 <- cs50 라이브러리 로드   
   1-1. cs50 라이브러리 연결 : clang -o string string.c -lcs50    <- '-l'은 연결 의미   
   1-2. make 명령어 사용 : make (프로그램명) 입력   <- make string = string.c를 string으로 컴파일  
#### string answer = get_string("What's your name?\n");   
1. 할당연산자 **=** 는 ← , 오른쪽에 있는 내용을 왼쪽으로 옮긴다/지정한다   
2. get_string 함수의 값을 저장할 변수 지정 -> answer = get_string("What's your name?\n");   
3. 변수(answer)가 저장하는 값의 종류가 문자열이라고 지정 -> string answer   
#### printf("hello,%s\n",answer);   
1. printf함수는 " "사이에 있는 하나의 문자열을 입력받는다   
2. %s -> 형식 지정자   
3. printf 함수의 " " 사이에 형식지정자가 있다면 쉼표와 변수명을 같이 입력하여 어디서 값을 가져올지 지정    
4. printf("hello,%s,%s\n",answer1,answer2); -> 변수 두 개를 왼쪽부터 오른쪽으로 순서대로 지정   





