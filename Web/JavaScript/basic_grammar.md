## 변수와 식별자   

- 식별자는 변수를 구분할 수 있는 변수명이며, 문자, 달러($), 밑줄(_) 등으로 시작    

- 대소문자를 구분하며, 클래스명 외에는 모두 소문자로 시작   

- 예약어 사용 불가   

- 식별자 작성 스타일   

  - 카멜 케이스  

    - 변수, 객체, 함수 사용  

    - 소문자로 구성    

      두번째 단어부터 첫글자는 대문자 (`camelCase`)      

  - 파스칼 케이스  

    - 클래스, 생성자에 사용    
    - 대문자로 구성   

  - 대문자 스네이크 케이스    

    - 상수(변경될 가능성이 없는 값)에 사용    
    - `ex>` API_KEY, PI 등   



- 변수 선언 키워드   

  | const                                   | let                                     | var                                                          |
  | --------------------------------------- | --------------------------------------- | ------------------------------------------------------------ |
  | ES6에서 사용 권장                       | ES6에서 사용 권장                       | ES6 이전에서 사용                                            |
  | **재할당 불가**                         | **재할당 가능**                         | 재할당 가능                                                  |
  | let num = 10<br />num = 5 <br />-> 불가 | let num = 10 <br />num = 5<br />-> 가능 | 호이스팅 되는 특성으로 인해 <br />예기치 못한 문제 발생 가능 |
  | 재선언 불가                             | 재선언 불가                             | 재선언 가능                                                  |
  | 블록 스코프                             | 블록 스코프                             | 함수 스코프                                                  |

  - 선언 (Declaration)   

    - 변수 생성    

    - 기본적으론 `undefined` 가 할당되어 있음     

    - `const`, `let` 둘 다 기본적으론 재선언 불가하지만,  

      브라우저 개발자도구에서만 재선언 가능한 것 처럼 보일 수 있음   

      -> 한 단락에 같이 쓰면 원래처럼 에러 발생   

      ```javascript
      let num = 10
      // undefined
      let num = 5
      // undefined
      
      let numb = 10
      let numb = 4
      // Uncaught SyntaxError: Identifier 'numb' has already been declared
      ```

      

  - 할당 (Assignment)   

    - 선언된 변수에 값을 저장    

  - 초기화 (Initialization)   

    - 선언된 변수에 처음으로 값을 저장      

  - 블록 스코프   

    - if, for, 함수 등의 중괄호 내부      

    - 블록 스코프를 가지는 변수는 블록 바깥에서 접근 불가     

      ```javascript
      lex x = 1	// 선언 및 초기화 할당
      
      if ( x== 1 ){
          let x = 2		// 블록 스코프내에선 처음 선언한 것므로 재선언 아님
          console.log(x)   // 2 - 블록스코프 내의 x 값은 2
      }
      console.log(x)       // 1 - 블록스코프 바깥에서의 x 값은 1
      ```

  - 함수 스코프    

    - 함수의 중괄호 내부    
    - 함수 스코프를 가지는 변수는 함수 바깥에서 접근 불가    

  

  - 호이스팅  

    - 변수를 선언 이전에 참조 가능한 현상   

    - 변수 선언 이전에 접근 시, `undefined` 반환   

      ```javascript
      console.log(num)   // undefined
      var num = 10
      
      // let, const를 위처럼 쓰면 참조 에러 발생   
      ```

</br>     



## 데이터 타입   

- 원시타입 (Primitive type)    

  - 변수에 해당 타입의 값이 담김    

  - 다른 변수에 복사할 때 실제 값 복사  

  - Number , String, Boolean, undefined, null, Symbol ..   등 객체가 아닌 기본 타입    

    -**Number**    

      - 부동소수점 형식 - 정수, 실수 구분 X      
      - 계산 불가능한 경우 NaN(Not-A-Number) 반환    

    -**String**  

      - 16비트 유니코드 문자의 집합   

      - `' '`, `" "` 모두 가능 -> 엔터로 개행 불가   

        -> 이스케이프 문자로 개행 가능        

      - 템플릿 리터럴   

        - ES6부터 지원   
        - 따옴표 대신 backtick 표현 -> 엔터로 개행 가능   
        - ${expression} 형태로 표현식 삽입 가능       

    -**Boolean**

      - 파이썬과 달리 소문자로 시작    

      - 자동 형변환 규칙      

        | 데이터 타입 | F          | T      |
        | ----------- | ---------- | ------ |
        | undefined   | 항상 F     | X      |
        | null        | 항상 F     | X      |
        | number      | 0, -0, NaN | 그외 T |
        | string      | 빈 문자열  | 그외 T |
        | object      | X          | 항상 T |

    - **undefined**  

      - 변수의 값이 없음을 나타냄        
      - 변수 선언 이후 값을 할당하지 않으면 자동으로 undefined 할당    

    - **null**  

      - 변수의 값이 없음을 의도적으로 표현할 때 사용    

      - null 의 타입은 ECMA 명세의 원시 타입의 정의에 따라 원시타입에 속하지만, `typeof` 연산자를 찍어보면 object 가 나옴   

        -> 일종의 버그이지만 여러 이유로 수정이 불가    

    | undefined                            | null                          |
    | ------------------------------------ | ----------------------------- |
    | 개발자가 의도적으로 필요한 경우 할당 | 자바스크립트가 자동으로 할당  |
    | typeof null  // object               | typeof undefined // undefined |



- 참조타입 (Reference type)     
  - 변수에 해당 객체의 참조 값이 담김   
  - 다른 변수에 복사할 때 참조 값이 복사됨    
    - 파이썬에서의 배열 얕은 복사와 비슷    
  - Objects (Array, Function, etc..)  등 객체 타입의 자료형        




</br>      

## 연산자   

- 할당 연산자 `=`  

  - 단축 연산자 지원   `+=` `-=` `*=` `/=`      

- 비교 연산자   

  - 결과값을 boolean 반환   

  - 문자열은 유니코드 값을 사용하며 표준 사전 순서 기반으로 비교   

    =>  `소문자 > 대문자` / `'a' < 'z'`    

- 동등 비교 연산자 (`==`)  

  - 암묵적 타입 변환을 통해 타입을 일치시킨 후 비교     

    ```javascript
    const a = 1004
    const b = '1004'
    console.log(a==b)  // True
    
    a+b  // 10041004
    ```

  - 두 피연산자가 모두 객체일 경우 메모리의 같은 객체를 바라보는지 판별    

    ```javascript
    const c = 1
    const d = true
    console.log(c == d)  // True
    
    c+d // 2
    ```

  - 예상치 못한 결과가 발생할 수 있어  특별한 경우를 제외하고 사용 X   

- 일치 비교 연산자 (`===`)   

  - 엄격한 비교가 이뤄지며 암묵적 타임 변환이 발생하지 않음    
    - 엄격한 비교는 타입과 값이 모두 같은지 비교하는 방식   

- 논리 연산자   

  - and 연산 = `&&`   
  - or 연산 = `||`  
  - not 연산 = `!`    
  - 단축 평가 지원    

- 삼항 연산자    

  - 세 개의 피연산자를 사용하여 조건에 따라 값 반환   

  - 가장 왼쪽이 True 이면, `:` 앞의 값 사용   

    ​					False 이면, `:` 뒤의 값 사용   

    `console.log(true ? 1 : 2)   // 1 `   

  - 삼항 연산자의 결과는 변수에 할당 가능     

    `const result = 2 > 1 ? 'y' : 'n'   `

    `console.log(result)   // y`    

  - 한 줄에 표기하는 것을 권장   

</br>   



## 조건문    

- if 문     

  - 결과값을 Boolean 타입으로 변환 후 T/F 변환   

  - `if` `else if` `else`    

  - 조건은 소괄호 , 실행할 코드는 중괄호 안에 작성   

  - 블록 스코프 생성     

    ```javascript
    if (조건) {
        실행코드
    } else if (조건) {
        실행코드
    } else {
        실행코드
    }
    ```

    - 조건 쓸 때는 일치비교 연산자(`===`) 사용   

      ```javascript
      let username = 'admin'
      if (username === 'admin'){
        console.log('관리자님 환영합니다.')
      } else if (username === 'manager'){
        console.log('매니저님 환영합니다')
      } else {
        console.log(`${username}님 환영합니다.`)
      }
      ```

      

- switch 문     

  - 결과값이 어느 값(case)에 해당하는지 판별   

  - 특정 값에 따라 조건을 분기할 때 활용하며, 조건이 많아질 경우 if문보다 가독성이 더 좋을 수 있음   

  - 블록 스코프 생성    

    ```javascript
    switch(표현식) {
        case 'value': {
            실행코드
            break
        }
        case 'value': {
            실행코드
            break
        }
        default : {
            실행코드
        }
    }
    ```

    - break 문, default 문은 선택적으로 사용 가능    
    - break문을 만나거나, default 문을 실행할 때까지 다음 조건문 실행(`Fall-through`)  

    ```javascript
    let operator = '+'
    
    switch(operator){
        case '+':{
            console.log(num1+num2)
            break
        }
        case '-':{
            console.log(num1-num2)
            break
        }
        case '*':{
            console.log(num1*num2)
            break
        }
        case '/':{
            console.log(num1/num2)
            break
        }
        default:{
            console.log('유효하지 않은 연산자')
        }
    }
    ```

    - `표현식의 결과값`과 `case 문의 value 값`을 비교하여 값이 같은 case문의 코드 실행  

</br>     



## 반복문   

- while     
  - 조건문이 참인 경우 반복 시행  
  - if문처럼  조건은 소괄호 안에, 실행할 코드는 중괄호 안에 작성    
  - 블록 스코프 생성   



- for    

  - `initialization ; condition ; expression` 으로 구성   

  - initialization   

    - 최초 반복문 진입시 1회만 실행되는 부분   

  - condition  

    - 매 반복 시행 전 확인하는 부분   

  - expression   

    - 매 반복 시행 이후 시행되는 부분   

    ```javascript
    for (let i=0; i < 3; i++){
        console.log(i) 
    }
    // 0, 1, 2   
    ```

    

- for in   

  - 주로 **객체의 속성**들을 순회할 때 사용  

  - 배열도 순회 가능하지만 권장 X       

    - 인덱스 순으로 순회한다는 보장이 없으므로 `for in`이 아닌`for of` 사용       

  - 실행할 코드는 중괄호 안에 작성       

  - 블록 스코프 생성   

    ```javascript
    const nums = {
        '1key' : 1,
        '2key' : 2
    }
    
    for (let num in nums) { 
        console.log(num) 
    }
    // 키 값 출력
    // 1key, 2key
    
    for (let num in nums) { 
        console.log(nums[num]) 
    }
    // 밸류값 출력
    // 1, 2
    ```

    ```javascript
    const bestMovie = {
      title: '벤자민 버튼의 시간은 거꾸로 간다',
      releaseYear: 2008,
      actors: ['브래드 피트', '케이트 블란쳇'],
      genres: ['romance', 'fantasy'],
    }
    
    for (let movie in bestMovie){
      console.log(`${movie}: ${bestMovie[movie]}`)
    }
    
    // title: 벤자민 버튼의 시간은 거꾸로 간다
    // releaseYear: 2008
    // actors: 브래드 피트,케이트 블란쳇
    // genres: romance,fantasy
    ```

    - JS 객체의 value는 점이나 대괄호를 이용하여 key값을 통해 접근 가능   
      - `for in` 은 키값을 꺼내오므로 value들에 접근하려면 출력할 때 [] 활용      
        - `bestMovie[key]` 출력    

- for of  

  - **반복가능한(iterable)한 객체**를 순회하며 값을 꺼낼 때 사용  

    - Array, Map, Set, String 등..    

  - 실행할 코드는 중괄호 안에 작성      

  - 블록 스코프 생성   

    ```javascript
    const fruits = ['사과','딸기']
    
    for (let fruit of fruits) {
        console.log(fruit) 
    }
    // 사과, 딸기  
    
    만약 for in 으로 돈다면, 인덱스가 출력됨   
    // 0, 1
    ```

    ```javascript
    const movies = [
      {title: '어바웃 타임'},
      {title: '굿 윌 헌팅'},
      {title: '인턴'},
    ]
    
    for (let movie of movies){
      console.log(`${movie.title}`)
    }
    // 어바웃 타임, 굿 윌 헌팅, 인턴
    ```

    - JS 객체의 value는 점이나 대괄호를 이용하여 key값을 통해 접근 가능   
      - `for of`는 for문 돌면서 객체를 하나씩 꺼내므로 value에 접근하려면 출력할 때 점 표기법 활용            
        - `movie.key`          

