## AJAX  

###### Asynchronous JavaScript And XML (비동기식 자바스크립트와 XML)        

- 서버와 통신하기 위해 XMLHttpRequest 객체 활용    
  - XMLHttpRequest 객체   
    - 서버와 상호작용하기 위해 사용   
    - 전체 페이지의 새로고침 없이 데이터 받아오기 가능   
    - 사용자의 작업을 방해하지 않으면서 일부 업데이트 가능    
    - 주로 AJAX 프로그래밍에 사용되며 이름과 달리 XML뿐 아닌 모든 종류의 데이터를 받아올 수 있음    
    - `XMLHttpRequest()` 로 생성   
- JSON, XML, HTML, 일반 txt 형식 등을 포함한 다양한 포맷 주고받기 가능   
  - AJAX의 X 가 XML을 의미하긴 하지만, 요즘은 더 가벼운 용량과 JS의 일부라는 장점때문에 JSON을 더 많이 사용     
- 특정 기술이 아닌 기존의 여러 기술을 잘 활용할 수 있는 방식으로 구성 및 재조합한 새로운 접근법을 설명하는 용어     
- 특징  
  - 페이지를 새로고침하지않아도 수행되는 **비동기성**     
    - 사용자의 특정 event가 있으면 전체가 아닌 일부분만 업데이트 가능   
  - 새로고침 없이 서버에 요청 + 서버로부터 데이터 받고 작업을 수행하는 작업이 가능하게 함      
- 예시  
  - G메일 전송 요청이 모두 처리되기전 다른페이지로 넘어가더라도 메일 전송    
  - 스크롤 행위 하나하나는 다 요청이지만 매번 새로고침되지는 않음       

</br>    



#### 비동기식 사용 이유   

- 사용자 경험의 증대를 위해        
  - 동기식이라면 데이터를 모두 불러올 때까진 앱이 멈춘 것 처럼 보이므로, 화면이 멈추고 응답하지 않는 것 같은 사용자 경험 제공   
  - 비동기식이라면 데이터를 요청하고 응답받는 동안, 앱 실행을 함께 진행하므로 데이터를 불러오는동안 지속적으로 응답 화면을 보여줄 수 있어 더욱 쾌적한 사용자 경험 제공 가능  



### 동기식 vs 비동기식

#### 동기식   

- 순차적, 직렬적 Task 수행   
- 요청을 보낸 후 응답을 받아야만 다음 동작 수행(blocking)   
- JavaScript는 single threaded 이기 때문에 발생   

#### 비동기식   

- 병렬적 Task 수행  
- 요청을 보낸 후 응답을 기다리지 않고 다음 동작 수행(non-blocking)    
- JavaScript는 single threaded 이기 때문에 발생       



#### Threads  

- 프로그램이 작업을 완료하기 위해 사용할 수 있는 단일 프로세스      

- 각 스레드는 한번에 하나의 작업만 수행 가능    

  - 앞의 작업이 완료되어야만 다음 작업 시작 가능    

- CPU는 여러 코어를 갖고 있으므로 한번에 여러가지 일 처리가능    

- single thread   

  - 컴퓨터가 여러개의 CPU를 갖고 있어도 main thread라 불리는 단일 스레드에서만 작업  수행 가능      

    = 즉, Single Thread는 이벤트를 처리하는 Call Stack 이 하나라는 의미    

  - 즉시 처리하지 못한 이벤트들은 다른곳(Web API)로 보내서 처리    

    -> 처리된 이벤트들은 처리순서대로 대기실(Task queue)에 줄 세워놓기     

    -> Call stack이 비면 담당자(Event Loop)가 대기줄에서 가장 앞에 있는(=오래된) 이벤트를 Call Stack으로 보냄(push)        



#### Concurrency model(동시성 모델)   

- Event loop를 기반으로 하는 동시성 모델    

- Call stack   

  - 요청이 들어올때마다 해당 요청을 순차적으로 처리하는 Stack 형태의 자료구조   

- Web API     

  - JS 엔진이 아닌 브라우저 영역에서 제공하는 API    
  - `setTimeot()` , `DOM events` 등의 AJAX로 데이터를 로드하는 시간이 소요되는 일들 처리    

- Task Queue (=Event Queue, Message Queue)      

  - 비동기 처리된 callback 함수가 대기하는 Queue 형태의 자료 구조   

  - main thread가 끝난 후 실행 

    -> 후속 JS 코드가 차단되는 것을 방지   

- Event Loop      

  - Call stack이 비어있는지 여부 확인   
  - 비어 있으면 Task Queue에 실행 대기중인 callback 함수가 있는지 여부 확인     
  - Task Queue에 대기중인 callback 함수가 있다면, 가장 앞에 있는 callback 함수를 Call Stack으로 Push     



#### Zero delays  

```javascript
console.log('Hi')

setTimeout(function () {
    console.log('SSAFY')
}, 3000 )

console.log('BYE')
```

```javascript
console.log('Hi')

setTimeout(function () {
    console.log('SSAFY')
}, 0 )

console.log('BYE')
```

- 위 두 개의 코드는 출력형태가 동일    

- 두번째 코드가 시간설정이 0인데도 불구하고 순서대로 처리되지 않는 이유는, 

  setTimeout의 시간인자가 0이라고 실제로 0ms 후에 callback 함수가 시작된다는 의미가 아니고, 실행은 Task Queue에 대기중인 작업수에 따라 다르기 때문이다.     

  특히, 두번째 코드에서는 callback 함수의 메시지가 처리되기전에 위아래가 먼저 출력된다.    

- delay는 보장된 시간이 아닌, JS가 요청을 처리하는데 필요한 최소시간이므로 setTimeout 함수에 특정 시간제한을 설정했더라도 대기중인 메시지의 모든 코드가 완료될때까지 대기해야한다.       



#### 순차적인 비동기 처리   

- 위의 코드들은 실행순서가 불명확     

  = Web API로 들어오는 순서는 중요하지 않고, 어떤 event가 먼저 처리되느냐가 중요   

- 이를 해결하기 위해, **순차적인** 비동기 처리를 위한 작성 방식 2가지 존재       

#### 1. Async callbacks       

- 백그라운드에서 코드 실행을 시작할 함수를 호출할때 인자로 지정된 함수     

- 백그라운드 코드 실행이 끝나면 callback 함수를 호출하여 작업 완료를 알리거나, 다음 작업을 실행하게 할 수 있음    

  `ex> `    `addEventListener()` 의 두번째 인자    

- callback 함수를 다른 함수의 인수로 전달할 때, 함수의 참조를 인수로 전달할 뿐, **즉시 실행되진 않고** 함수의 body에서 'called back' 됨    

  정의된 함수는 때가 되면 callback 함수 실행 역할을 함    

  ##### - Callback function   

  - 다른 함수에 인자로 전달된 함수   

  - 외부 함수 내에서 호출되어 일종의 루틴 or 작업 완료    

  - 동기식, 비동기식 모두 사용    

    - 주로 비동기 작업이 완료된 후 코드 실행을 계속하는데 사용     

      = 이러한 콜백함수를 비동기 콜백이라고 함   

    `ex>`   JS의 `addEventListener()`  ,  python의 `map()` ,  Django의 `path()`         

  

  - 비동기 로직을 수행할때 callback 함수가 필요한 이유   
    - Django의 경우 `요청이 들어오면`, event의 경우 `특정 이벤트가 발생하면` 이라는 조건으로 함수를 호출할 수 있었던 것 처럼 callback 함수는 명시적 호출이 아닌 특정 루틴 혹은 action에 의해 호출되는 함수(= 다른 함수의 매개변수로 전달되어 해당 함수 내에서 특정 시점에 호출 가능)이므로            

  

  ##### - Callback Hell(콜백 지옥)      

  - 비동기 콜백의 한계점이며, pyramid of doom(파멸의 피라미드) 라고도 함     

  - 순차적인 연쇄 비동기 작업 처리를 위해 `callback 함수 호출 > 그다음 callback 함수호출 > 또 그 함수의 callback 함수 호출...` 의 패턴이 지속적으로 반복      

    = 여러 개의 연쇄 비동기 작업을 할때 마주하는 상황   

  - 콜백 지옥이 벌어질 경우, 디버깅이 어려워지고 코드 가독성이 떨어짐       

  

  ##### - Callback Hell 해결   

  1. Keep your code shallow   
     - 코드의 깊이를 얕게 유지    
  2. Modularize   
     - 모듈화   
  3. Handle every single error    
     - 모든 단일 오류 처리   
  4. Promise callbacks    
     - Promise 콜백 방식 사용    



#### 2. promise-style      

- Modern Web APIs 에서의 새로운 코드 스타일 

- XMLHttpRequest 객체를 사용하는 구조보다 조금더 현대적인 버전   

- Async callbacks의 단점(=콜백 지옥) 보완     

  - callback 함수는 JS의 Event Loop가 현재 실행중인 Call Stack을 완료하기 이전에는 절대 호출되지 않지만, 

    Promise callback 함수는 Event Queue에 배치되는 엄격한 순서로 호출되는 것을 보장       

  - 비동기 작업이 성공하거나 실패한 뒤, `.then()` 메서드를 이용하여 추가한 경우에도 위와 같이 동작(=엄격한 순서로 호출되는 것을 보장)         

  - `.then` 을 여러번 사용하여 여러개의 callback 함수 추가 가능(=chaining)    

    - 각 callback은 주어진 순서대로 하나하나 실행하게 되며, chaining은 Promise의 가장 뛰어난 장점      

- Promise object      

  ```javascript
  const myPromise = axios.get(URL)  
  // console.log(myPromise) // Promise object   
  
  // chaining
  axios.get(URL)	// promise 객체   
    .then(response => {
    return response.data
  })
    .then(response => {
    return response.title
  })
    .catch(error => {
    console.log(error)
  })
    .finally(function() {
      console.log('마지막에 무조건 시행')
  })
  ```

  

  - 비동기 작업의 최종 완료 or 실패를 나타내는 객체    

    - 미래의 완료 or 실패와 그 결과값을 나타냄  
    - 미래의 어떤 상황에 대한 약속     

  - 순서와 연쇄, 가독성 보장   

    - then과 catch 등은 모두 같은 레벨에 작성   

    - 하나의 then 구문에 모두 작성할 수 있지만, 나눠서 작성함으로써 가독성을 챙기고, 모듈화까지 했다고 볼 수 있음    

      - 각각의 `then` 블록과 `catch` 블록은 서로 다른 promise를 반환하므로 `then()` 이나 `catch()` 을 여러개 사용(`chaining`) 하여 연쇄적인 작업 수행 가능        

      = 여러 비동기 작업을 차례대로 수행 가능    

  - 성공(이행)에 대한 약속 `.then()`   

    = 성공에 대한 약속 보장    

  - 실패(거절)에 대한 약속 `.catch()`     

    = 위에서 실패했을 경우 .catch() 에 작성된 콜백함수가 실행     

  ##### .then(callback)  

  - 이전 작업(promise)이 성공했을때 수행할 작업을 나타내는 콜백 함수   

  - 각 콜백 함수는 이전 작업의 성공 결과를 인자로 전달받으므로 성공했을 때의 코드를 콜백함수 안에 작성     

    = 첫번째 then 구문의 리턴값을 두번째 then 구문의 인자로 들어가는 식   

    = 그러므로 **리턴값 반드시 필요! **       

  ##### .catch(callback)    

  - `.then` 이 하나라도 실패하면 동작   
    - try - except 구문과 유사   
  - 이전 작업의 실패로 생성된 error 객체는 catch 블록 안에서 사용 가능      

  ##### .finally(callback)    

  - Promise 객체 반환      

  - 결과와 상관없이 무조건 콜백함수가 실행되므로 어떠한 인자도 전달받지 않음      

    - 성공인지 거절인지 판단할 수 없기 때문     

  - 무조건 실행되어야 하는 절에서 활용     

    = `then` 블록과 `catch` 블록에서의 코드 중복 방지      

  

  ##### Axios   

  ###### Promise based HTTP client for the browser and Node.js    

  `axios.get(URL)`        

  - 브라우저를 위한 Promise 기반 클라이언트     

    = Promise 객체 반환         

    = `.then` , `.catch` 메서드 사용 가능    

  - 원래는 `XHR(XMLHttpRequest)` 이라는 브라우저 내장 객체를 활용해 AJAX 요청을 처리하는데, 이보다 편리한 AJAX 요청이 가능하도록 도움    

    - 확장 가능한 인터페이스 + 패키지로 사용이 간편한 라이브러리 제공       

  - 사용을 위해선 CDN을 설치하거나 여러 방법 존재  [공식 깃헙](https://github.com/axios/axios#installing)

    - CDN 설치 `<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>`     

  ```javascript
  axios.get(URL)
    .then(response => response.data)
    .then(data => data.title)
    .then(title => console.log(title))
    .catch(error => console.log(error))
    .finally(function(){
    console.log('나는 무조건 시행')
  })
  ```

  

  ##### async & await   

  - then 블록이 무수히 많아지는 경우를 대비하여 등장    
    - 비동기 코드를 작성하는 새로운 방법     
    - ECMAScript 2017(ES8)에서 등장      
  - 기존 Promise 시스템 위에 구축된 syntactic sugar     
    - Promise 구조의 then chaining 제거   
    - 비동기 코드를 조금더 동기 코드처럼 표현     
    - syntactic sugar  
      - 더 쉽게 읽고 표현 가능하도록 설계된 프로그래밍 언어 내의 구문   
      - 문법적 기능은 그대로 유지하되 직관적으로 코드를 읽을 수 있게 만든 것    

