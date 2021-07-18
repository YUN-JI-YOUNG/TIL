# 예외(Syntactic error)   

## 구문 오류   
###### 잘못된 명령을 입력하여 파이썬 인터프리터가 해석하지 못할 때 발생하는 오류   

- 원인 : 오타, 문법 실수 등   

## 예외    
###### 문법적으론 문제가 없지만, 실행 중에 예기치 않게 발생하며 처리하지 않으면 프로그램이 종료되는 오류       

### 예외처리 방법   
#### 1. if ~ else문 이용  
- 정상적인 흐름을 제어할 경우에만 사용 가능 -> 예외처리 전용 메커니즘 필요할 가능성 존재      
- 예외 발생 상황을 사전에 제어할 수 있음 -> 정수 계산식 코드에선 조건식에 isdigit()함수 사용하는 등     
- if 조건식에 True를 반환하면 그대로 실행, 에외가 발생하여 False를 반환하면 else문 실행   

#### 2. try ~ except문 이용   
- 예외가 발생했을 때 처리 가능    
- try문을 실행하다 예외 발생시 except문 실행   
- except문이 실행될 경우 try문의 남은 부분은 무시   

#### 3. try ~ except ~ else문 이용   
- 예외가 발생했을 때 처리 가능   
- 예외가 발생하지 않았을 때 처리 가능   
- try문을 실행하다 예외 발생시 except문 실행, 발생하지 않으면 else문 실행   

#### 4. try ~ except ~ else ~ finally문 이용
- 예외가 발생했을 때 처리 가능   
- 예외가 발생하지 않았을 때 처리 가능   
- 예외 발생과 상관없이도 실행 가능    
- try ~ except ~ else문과 같은 단계로 실행되며, finally문은 예외 유무 상관없이 무조건 실행  

### 예외 객체   
###### 코드 실행 중 오류가 발생하면 생성되며 오류 발생 관련 정보 보유   
- ``` except Exception as ex : ``` 처럼 except문 뒤에 붙여 사용   
-> 예외 객체 Exception을 객체 ex로 except문에서 type(ex) 등으로 참조한다는 뜻          
- Excpetion 이외에도 예외 객체로 ValueError, ZeroDivisionError 등 사용 가능      
     
### 강제 예외   
- else문에 raise문을 사용하여 강제 예외 발생    
``` 
else : 
 raise ValueError("값 오류입니다.")
 ```   
 -> if문의 조건식에 대해 False라면 raise문을 이용해 강제로 ValueError 예외 상황 발생 유발   
-> 그 이후엔 `except ValueError as ve : ` 문으로 예외 객체를 ve로 참조    
-> 그 외 예외에 대해선 `except Exception as ex : `문으로 예외 객체를 ex로 참조   

