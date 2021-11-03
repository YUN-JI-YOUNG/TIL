## 객체   

- 속성의 집합    

- 중괄호 내부에 key - value 의 쌍으로 표현   

  - key는 문자열 타입만 가능   
    - 중간에 띄어쓰기 등의 구분자가 있으면 따옴표로 묶어서 표현   
    - 구분자 없으면 안써도 상관 x   
  - value는 모든 타입 가능   

- 객체 요소 접근은 점 or 대괄호로 가능   

  - key 이름에 띄어쓰기 등의 구분자가 있으면 대괄호 접근만 가능   

    `nums['num lst']`    

- ES6에 새로 도입된 문법들로 객체 생성 및 조작에 유용하게 사용 가능   

  - 속성명 축약    

    - 객체 정의할 때, key와 할당하는 변수의 이름이 같으면 축약 가능   

      ```javascript
      var bookShop = {
          books : books,
      }
      
      // -> 속성명 축약 문법 사용  
      const bookShop = {
          books,
      }
      
      console.log(bookShop.books)
      ```

      

  - 메서드명 축약     

    - 메서드 선언 시 function 키워드 생략 가능   

      ```javascript
      var obj = {
          greeting : function(){
              console.log('hello')
          }
      };
      
      // -> 메서드 축약 문법 사용  
      const obj = {
          greeting() {
              console.log('h1')
          }
      };
      
      obj.greeting()
      ```

      

  - 계산된 속성명 사용    

    - 객체 정의할 때, key의 이름을 표현식을 이용하여 동적으로 생성 가능     

      ```javascript
      const key = regions
      const value = ['서울','인천','대전']
      
      const res = {
          [key]: value,
      }
      console.log(res)  			// {regions: Araay(4)}
      console.log(res.regions)	// ["서울","인천","대전"] 
      ```

      

  - 구조 분해 할당  

    - 배열 또는 객체를 분해하여 속성을 변수에 쉽게 할당할 수 있는 문법   

      ```javascript
      const user = {
          name: 'jyj',
          userId : '334',
          phone : '010-1234-5678',
          email : 'jyj@google.com'
      }
      
      const name = user.name
      const userId = user.userId
      
      
      // -> 구조 분해 할당  
      
      const {name} = user
      const {userId} = user
      
      // 여러개도 가능 
      const {name, userId} = user
      ```



- JSON (JavaScript Object Notation)     

  - key-value 쌍의 형태로 데이터를 표기하는 언어 독립적 표준 포맷     

  - html보다 가볍고 빠르며, 데이터를 주고받을때 더 적절하기 때문에 html이 아닌 json 사용         

    - html은 사람이 쓰라고 만든 것이기때문에 데이터를 주고받는 데 사용하기엔 투머치    
    - 특히, css 등과 같이 꾸미는 것은 데이터 주고받기에 상관 X         

  - 자바스크립트의 객체와 유사하게 생겼으나 실제로는 문자열 타입   

    - 따라서 JS의 객체로 조작하기 위해서 구문 분석(파싱) 필요   

  - 자바스크립트에서 JSON을 조작하기 위한 2가지 내장 메서드 제공  

    - `JSON.parse()`    

      - JSON -> 자바스크립트 객체    

        ```javascript
        const jsondata = JSON.stringify({
            name : 'yjy',
            userid : 'yun1234'
        })
        
        const parsedata = JSON.parse(jsondata)  
        
        console.log(parsedata)	// {name:"yjy", ..}
        ```

        

    - `JSON.stringify()`    

      - 자바스크립트 객체 -> JSON      

        ```javascript
        const jsondata = JSON.stringify({
            name : 'yjy',
            userid : 'yun1234'
        })
        
        console.log(jsondata)		// "{"name":"yjy",...}" 
        ```

</br>   
<hr>   



## 함수   

- 참조 타입 중 하나로 function 타입        

- JS의 함수는 일급 객체에 해당   

  - 일급 객체는 변수에 할당가능 / 함수의 매개변수로 전달가능 / 함수 반환값으로 사용가능    

- 기본 인자   

  - 인자 작성시 `=` 문자 뒤 기본 인자 선언 가능

    `(args) 부분에 (name = 'no')`       

    ```javascript
    function makeOrder(menu, size='regular'){
    	return {menu: menu, size: size}
    }
    makeOrder('mocha')
    
    // { menu: 'mocha', size: 'regular' } 
    // 각 인자를 속성으로 갖는 객체를 반환  
    ```

    

- 함수 정의 방법   

  - 함수 선언식      

    ```javascript
    function name(args,) {
        실행 코드
    }
    // name : 함수 이름
    // args : 매개변수  
    ```

    ```javascript
    // 길이가 8 미만이면 false, 8 이상이면 true
    
    function isValid(password){
    	if (password.length < 8){
    		return false
    	} else {
    		return true
    	}
    }
    isValid('abcd')
    
    // false
    ```

    

  - 함수 표현식     

    - 함수를 표현식 내에서 정의하는 방식   
    - 이름을 생략하여 익명 함수로 정의 가능   
      - 익명함수는 따로 변수에 할당하지 않는 이상 추후 사용 불가하며, 함수 표현식에서만 가능   

    ```javascript
    const myFunction = function (args,){
        실행코드
    }
    // name : 함수이름(생략가능)
    // args : 매개변수 
    
    ```

    ```javascript
    // 특정 문자 기준으로 하나의 문자열로 합치기
    
    let myJoin = function (array,separator){
    	let res = ''
    	let cnt = array.length
    	for (let num of array){
    		res += num
    		if (cnt > 1){
    			res += separator
    			cnt -= 1
    		}
    	}
    	return res
    }
    myJoin(['010','1234','5678'], '-')
    
    // '010-1234-5678'
    ```

    ```javascript
    let myJoin = function (array,separator){
    	let res = ''
    	for (let cnt = 0; cnt < array.length; cnt++){
    		res += array[cnt]
    		if (cnt < array.length -1){
    			res += separator
    		}
    	}
    	return res
    }
    myJoin(['010','1234','5678'], '-')
    
    // '010-1234-5678'
    ```

    

|        | 함수 선언식                                               | 함수 표현식                                               |
| ------ | --------------------------------------------------------- | --------------------------------------------------------- |
| 공통점 | 데이터타입, 함수 구성요소<br />(이름, 매개변수, 실행코드) | 데이터타입, 함수 구성요소<br />(이름, 매개변수, 실행코드) |
| 차이점 | 익명함수 불가                                             | 익명함수 가능                                             |
|        | 호이스팅 O                                                | 호이스팅 X                                                |

- 보통 함수 표현식을 권장    

- 함수 선언식으로 선언한 함수는 `var` 변수처럼 호이스팅 발생    

  - 함수를 호출하고 이후에 선언해도 동작   

    ```javascript
    add(2,7)  
    function add (n1,n2){
        return n1+n2
    }
    
    // 9
    ```

  - 함수 표현식으로 선언하면 선언 이전에 호출할 경우 에러 발생      

  - 함수 표현식으로 정의된 함수는 변수로 평가되어 변수의 scope 규칙에 따름     

    -> `const` 도 선언 이전에 호출하면 참조에러 발생       

    -> 정말 만약에라도 표현식으로 선언했을 때, 호이스팅 된다고 해도 변수가 호이스팅된 것이고 함수가 호이스팅된 것은 아님     

    ```javascript
    fun(7,2)
    
    const fun = function(n1,n2){
        return n1+n2
    }
    
    // Uncaught ReferenceError: fun is not defined
    ```

  - 만약, 함수 표현식을 var 키워드로 작성할 경우 변수가 선언전 `undefined` 로 초기화되어 다른 에러 발생    

    ```javascript
    fun(7,2)
    
    var fun = function(n1,n2){
        return n1+n2
    }
    
    // Uncaught TypeError: fun is not a function  
    ```
</br>   


### 화살표 함수 (Arrow function)   

- 함수를 비교적 간결하게 정의할 수 있는 문법      

  함수 정의      

  ```javascript
  const arrow = function (name){
      return 'hello'
  }
  ```

  1. function 키워드 생략 가능     

  ```javascript
  const arrow = (name) => {return `hello!`}
  ```

  2. 매개변수가 1개라면, `()`  생략 가능     

  ```javascript
  const arrow = name => {return `hello!`}
  
  // 현재 매개변수가 name 1개이므로 생략 가능 
  ```

  3. 실행코드가 표현식 하나라면, `{ }` , `return` 생략 가능   

  ```javascript
  const arrow = name => `hello!`
  
  // 실행코드가 return 문 하나이므로 생략 가능
  ```

  

</br>   
