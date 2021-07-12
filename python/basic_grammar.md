## 1. 기본적인 코드 작성법   
### 1-1. 들여쓰기
- 가독성 좋은 코드를 위한 것으로, 파이썬 인터프린터에 의해 잘못된 들여쓰기가 검사되지 않으면 치명적 버그 발생   
- 4개의 공백문자로 들여쓰기하는 것이 규칙   
- 동일 코드 블록 내에선 같은 들여쓰기 사용 필수   
- 동일 코드 블록 내가 아니라면 공백문자와 탭을 혼용할 수는 있지만 권장 X   

### 1-2. 세미콜론    
- 파이썬은 기본적으로 세미콜론 생략 가능   
- 하지만, 한 줄에 여러 문장 기술할 경우 구분을 위해 세미콜론 사용 필수   

### 1-3. 파일과 모듈   
- 파이썬에서 .py 파일은 하나의 모듈   
- 모듈을 만드는 것은 재사용하는 데에 큰 도움   
- 모듈은 다른 모듈에서 불러들여 사용하는 라이브러리 성격의 기능을 가진 모듈과 프로그램의 진입점 역할을 하는 메인 모듈 존재     
- 만약 하나의 모듈이 라이브러리 기능과 메인 모듈을 모두 포함한다면, If 문으로 분기 필요     
``` 
if __name__ == "__main__":
    print("Hello,World!")
```   
> 이 모듈은 소스코드가 메인으로 실행될 때만 실행되며, 다른 모듈에서 라이브러리 용도로 불러와 사용할 땐 실행되지 x        

### 1-4. 소스코드 인코딩     
- 코드 앞에 #을 붙이면 주석으로 처리되며, #주석에 작성된 코딩 지시자는 특별한 의미로 해석     
- utf-8 소스코드 인코딩 지정 방법 > ```#coding : utf-8``` or ```# -*- coding : utf-8 -*- ```      

### 1-5. 주석   
###### 코드 앞에 ```#```을 붙여 작성된 부분. 상세설명을 추가하거나 특정코드를 실행하지 않을 목적으로 사용     
- 인터프리터의 문법 검사에 무시되고 실행 X   

</br>   

## 2. 자료형      
- 리터럴(Literal) : 소스코드 상에서 내장 자료형의 상수 값을 나타내는 용어   
  - 종류 : 정수형 리터럴 / 부동소수점 숫자형 리터럴 / 문자열 리터럴 / 부울형 리터럴 / 리스트형 리터럴   
- 리터럴의 자료형 확인 -> type 함수 사용! ```type()```       
  - 값에 의해 자료형이 결정되는 동적 타이핑 언어인 파이썬의 성격 때문에 종종 자료형을 확인해야 하는 경우 발생     
   
### 2-1. 숫자형     
###### 숫자 리터럴이 사용된 자료형   
###### 숫자 리터럴 내의 ```_```는 무시   
- 정수형     
  - 정수형의 길이는 무제한이며 메모리 허용 범위까지 가능   
  - 0o 접두어로 8진수, 0x 접두어로 16진수, 0b접두어로 2진수 사용가능          
- 부동 소수점형     
    - 5. 처럼 소수부를 생략하거나, .01 처럼 정수부 생략 가능   
    - 매우 큰 수 혹은 매우 작은 수를 표현하기 위해 지수 표기법 사용 가능 (ex. 2e+100 등)      
- 허수형
    - 파이썬에선 i 대신 j 사용      

## 2-2. 문자열   
###### 문자들의 집합으로 " "와 ' ' 사용, 파이썬 버전3부턴 유니코드를 기본 문자열로 사용       
- 자료형으로서의 문자형은 제공하지 않음     
- " " 안에 ' ' 중복 사용이나 ' ' 안에 " " 중복 사용은 가능 (다른 따옴표끼리 중복 가능)       
- but, ' ' 안에 ' ' 사용이나 " " 안에 " " 사용은 불가능 (같은 따옴표끼리 그냥 중복 불가능)               
-> 작은/큰따음표 자체를 의미하는 이스케이프 시퀀스 작은/큰 따옴표(```\'```, ```\"```)문자 조합 사용    
- """나 ''' 처럼 따옴표 3개를 문자열 앞 뒤에 사용하면, 줄바꿈 문자를 사용하지 않고도 다중행으로 표현된 문자열 생성 가능   
```
print('''
안녕하세요.
누구누구
입니다.
''')
                    
print('\n안녕하세요.\n누구누구\n입니다.\n')
```  
※ 이스케이프 시퀀스
- 소스 코드 내에서 사용할 수 있도록 ```\``` 기호와 조합해서 사용하는 사전에 정의해둔 문자 조합
- 문자열의 출력 결과 제어 목적         
- ```\```와 ```"```와 ```'```는 문자열 출력시 단독으로 사용하면 각각의 용도(이스케이프 시퀀스 조합, 문자열 리터럴 생성)로 사용되기 때문에, 그 자체를 출력하기 위해선 이스케이프 시퀀스 필요   
- ```\n``` 라인 피드(LF) - 줄바꿈
- ```\t``` 수평 탭          

### 2-2-1. 문자열 포맷팅      
###### 문자열 내에 사용된 문자열 표시 유형(문자열 포맷 코드)을 특정 값으로 변경하는 기법     
- %-포맷팅      
    - ```'s'``` 문자열 포맷, ```'c'``` 문자 포맷,정수->유니코드 문자 변환, ```'d'``` 10진 정수, ```'o'``` 8진수, ```'x'``` 16진수, ```'f'``` 부동소수점 숫자, 소수점 이하 6자리 기본값, ```'%'``` %문자 자체 출력    
    - 자료구조 튜플 -> ```"이름 : %s\n나이 : %s 세" % ("김아무개", 26)``` 이면 각 원소를 순서대로 각각의 %s에 전달         
    - 자료구조 사전 -> ```"이름 : %(name)s\n나이 : %(age)s 세" % {"name" : "김아무개", "age" : 26 }``` 이면 %와 s 사이에 사전에 적은 키 값 반환    
        - 자료구조 튜플/사전은 여러 포맷이 섞여도 순서에 맞춰 모두 적용 가능      
    - 폭과 정렬방향          
        - ```"%-10s" % "좌측정렬"``` 이면 문자열 폭 10이고, 좌측정렬. ```'좌측정렬_ _ _ _ _ _'```      
        - ```"%10s" % "우측정렬"``` 이면 문자열 폭 10이고, 우측정렬. ```'_ _ _ _ _ _우측정렬'```          
        - ```"%0.3f" % 1.2345678``` 이면 소수점 아래 3자리까지만 표시. ```'1.234'```       
        - ```"%10.3f" % 1.2345678``` 이면 전체 폭이 10이고, 소수점 아래 3자리까지 표현. ```'_ _ _ _ _1.234'```     
        - ```"%010.3f" % 1.2345678``` 이면 나머진 위와 같고, 앞의 남은 자리를 0으로 표현.  ```'000001.234'```        

- str.format()함수 이용 
    - 0부터 시작하는 위치 인덱스 or 값의 위치 이용     
 ```"이름:{0}, 나이:{1}세".format("김아무개",26)``` = ```"이름:{}, 나이:{}세".format("김아무개",26)```    
        - 순서가 바뀌어도 위치 인덱스를 사용하면 인덱스대로 출력   ```"나이:{1}세, 이름:{0}".format("김아무개",26)```          
        - 위치 인덱스내에서 ```:```뒤에 문자열 표시 유형을 사용하면 포맷 지정 가능 ```"{0:d}".format(26) // 26을 10진수로 출력```         
    - 이름 이용   
        - 이름=값 형식으로 인자 구성 ```"이름:{name}, 나이:{age}세".format(name="김아무개", age=26) // {name}은 이름이 name인 값 반환 등```      
    - 폭과 정렬방향   
        - ```"{0:<10}".format("좌측정렬")``` 이면 폭이 10이고 < 이므로 좌측정렬. ```'좌측정렬 _ _ _ _ _ _'```   
        - ```"{0:>10}".format("우측정렬")``` 이면 폭이 10이고 > 이므로 우측정렬. ```'_ _ _ _ _ _우측정렬'```     
        - ```"{0:^10}".format("중앙정렬")``` 이면 폭이 10이고 ^ 이므로 중앙정렬. ```'_ _ _중앙정렬_ _ _'```      
        - ```"{0:*^10}".format("중앙정렬")``` 이면 나머지는 위와 같고, ```*```은 공백을 채울 문자. ```'***중앙정렬***'```      
        - ```"{0:0.2f}".format(1.2345678)``` 이면 소수점 아래 2자리까지 표현. ```'1.23'```    
        - ```{0:10.2f}```는 폭 10, 소수점2자리, ```{0:010.2f}```는 폭10, 소수점2자리, 공백 0 표기   
        - ```"{{ {0:.2f} }}".format(12.34)```이면 소수점2자리까지, 대괄호 표현 ```'{12.34}'```   
        
   
※ ord("문자열")함수는 문자열을 정수로 반환   
