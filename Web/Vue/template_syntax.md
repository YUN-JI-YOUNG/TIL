</br>     



## Template Syntax

- 렌더링 된 DOM을 기본 Vue 인스턴스의 데이터에 선언적으로 바인딩할 수 있는 HTML 기반 템플릿 구문 사용   
- Interpolation    `{{ }}`
- Directive   `v-`  

### 1. Interpolation (보간법)    

1. Text   

   `<span> 메시지 : {{msg}} </span> `  

2. Raw HTML   

   `<span v-html="rawHtml"> 해당 html 태그 </span>`

3. Attributes  

   `<div v-bind:id="dynamicId"></div>`     

4. JS 표현식   

   `{{number+1}}`  

   `{{ message.split('').reverse().join('') }}`   : 메시지 뒤집기    

   - 다만 좋은 표현식은 아니며 특수한 경우에 사용가능하다는 것   



### 2. Directive(디렉티브)  

- `v-` 접두사가 있는 특수 속성      
- 속성 값은 단일 JS 표현식  (v-for는 예외)   
- 표현식의 값이 변경될 때 반응적으로 DOM에 적응하는 역할   
- 전달인자 (Arguments)   
  - `:` 을 통해 전달인자 받기 가능   
- 수식어 (Modifiers) 
  - `.` 으로 표시되는 특수 접미사     
  - directive를 특별한 방법으로 바인딩해야함을 나타냄     

#### v-text  

- 엘리먼트의 textContent 업데이트   
  - innerText 와 유사한 기능    
- 내부적으로 interpolation 문법이 v-text로 컴파일       
  - `<p v-text="message"></p>` 와 `<p> {{message}} </p>` 는 동일   



#### v-html  

- 엘리먼트의 innerHTML 업데이트   
  - XSS 공격에 취약할 수 있음    
  - 내용은 일반 HTML로 삽입되므로 Vue 템플릿으로 컴파일 X    
- 임의로 사용자로부터 입력받은 내용은 v-html에 **절대 사용금지**        

<hr>  




### 조건부 렌더링

#### v-show  

- 조건부 렌더링 중 하나    
- 엘리먼트는 항상 렌더링되고 DOM에 남아있음   
  - false 일때엔 자리 차지하지 않음 -> true가 되면 나타남 (항상 렌더링된 상태이므로)    
- 단순히 엘리먼트에 CSS display 속성 토글     
  - `style=display:none;`       
- Expensive initial load, cheap toggle (값비싼 초기 load / 값싼 toggle)          
  - CSS display 속성을 hidden으로 만들어 토글   
  - 1번만 렌더링 되는 경우, v-if 에 비해 상대적으로 렌더링 비용 높음      
    - 실제로 렌더링되지만 눈에서 보이지않는 것이므로    
  - 자주 변경되는 요소라면 렌더링 이후엔 보여주는지에 대한 여부만 판단하면 되기 때문에 토글 비용이 적음   



#### v-if, v-else-if, v-else  

- 조건부 렌더링 중 하나   
- 조건에 따라 블록 렌더링하며, directive의 표현식이 true일때만 렌더링    
- 엘리먼트 및 포함된 directive는 토글하는 동안 삭제되고 다시 작성    
- Cheap initial load, expensive toggle (값싼 초기 load / 값비싼 toggle)            
  - 전달인자가 false인 경우 아예 렌더링 되지 않음    
    - 렌더링 자체가 되지 않는 것이므로 렌더링 비용 낮음    
  - 자주 변경되는 요소의 경우 매번 다시 렌더링 해야하므로 비용이 증가할 수 있음        

<hr>    






### 반복문 렌더링

#### v-for 

- 원본 데이터를 기반으로 엘리먼트 or 템플릿 블록을 여러번 렌더링   

- `item in items` 구문 사용   

- item 위치의 변수를 각 요소에서 사용 가능   

  - 객체의 경우는 key   

- `v-for` 사용시 반드시 key 속성을 각 요소에 작성        

  - key를 안쓴다고 에러가 나진 않지만,  객체 불변성 유지를 위해 필요 (스타일 가이드)     

  - 문자열에선 `<div v-for="(char,index) in myStr" :key="index">` 처럼 작성 가능      

    ```javascript
    <div v-for="value in myObj">
        {{value}}
    </div>
    
    // 위처럼이 아닌 스타일 가이드 대로 아래와 같이 작성 권장 
    
    <div v-for="(value,key) in myObj":key="key">
        {{value}}
    </div>
    ```

    

- `v-if` 와 함께 사용하는 경우 `v-for` 이 더 높은 우선순위 (스타일 가이드)         

  - 가능하면 함께 사용은 X        

    - 리스트 목록을 필터링하기 위한 목적  

      `v-for="user in users" v-if="user.isActive"`      

      - DOM에서 필터링 하지 말고 View 에서 애초에 필터링 할 것   

        ```javascript
        // vue
        activeUsers: function () {
                  return this.users.filter(user => {
                    return user.isActive
                  })
            
        // dom
        <ul>
            <li
            v-for="user in activeUsers"
            :key="user.id"
            >
              {{ user.name }}
        	</li>
        </ul>
        ```

        

    - 숨기기 위해 리스트의 렌더링을 피할 목적   

      `v-for="user in users" v-if="shouldShowUsers"`    

      - if 문으로 렌더링을 할지 말지 결정    

      - 이러한 경우엔 `v-if`를 `ul` 이나 `ol` 등의 컨테이너 엘리먼트로 옮기길 권장      

        ```javascript
        <ul v-if="shouldShowUsers">
            <li
              v-for="user in users"
              :key="user.id"
            >
              {{ user.name }}
            </li>
        </ul>
        ```

<hr>  


#### v-bind     

- HTML 요소의 속성에 Vue의 상태 데이터를 값으로 할당   

- Object 형태로 사용하면 value가 true인 key가 class 바인딩 값으로 할당     

- `<img v-bind:src="imageSrc" alt="sample img">` 

  -> 약어 : `<img :src="imageSrc" alt="sample img"> `    

- `<h2 :class="[activeRed, myBackground]">Hello Vue!!</h2>` 처럼 배열로 class 추가도 가능    

- 약어 (Shorthand)   

  - `:`    
  - `v-bind:href` -> `:href`   
  - `v-bind:key` -> `:key`     

- 스타일 바인딩도 가능  

  ```javascript
  <ul>
      <li v-for="todo in todos" 
      :class="{ active: todo.isActive}"
      :style="{fontSize: fontSize + 'px'}"
      :key="todo.id">
        {{todo}}
      </li>
  </ul>
  ```



#### v-on  

- 엘리먼트에 이벤트 리스너 연결   
- 이벤트 유형은 전달인자로 표시    
- 특정 이벤트가 발생했을 때, 주어진 코드 실행   
- 약어(shorthand)     
  - `@`   
  - `v-on:click` -> `@click`    





 #### v-model    

- HTML form 요소의 값과 data를 양방향 바인딩      

  ```javascript
  <div id="app">
      <h2>1. Input -> Data </h2>
      <h3>{{myMessage}}</h3>
      <input v-model="myMessage" type="text">
  </div>
  
  const app = new Vue({
      el:'#app',
      data:{
        myMessage: '',
        myMessage2: '',
        isChecked: true,
      }
    })
  ```

  - 한글, 중국어, 일본어 등은 직접 input 메서드 구현 필요      

    - 안해도 동작은 하지만, 한박자씩 느림       
    - 원래부터 한글은 초성, 중성, 종성 등으로 글자 작성 완료 여부 판단이 어려워 글자가 완성된 다음에야 인식 가능하는 등의 이슈 존재       

    ```java
    <h2>2. Input -> Data</h2>
      <h3>{{myMessage2}} </h3>
      <input v-on:input="onInputChange" type="text">
          
    const app = new Vue({
        el:'#app',
        data:{
          myMessage: '',
          myMessage2: '',
          isChecked: true,
        },
        methods:{
          onInputChange: function(event) {
            this.myMessage2 = event.target.value
          }
        }
      })
    ```

    

- 수식어   

  - `.lazy`   

    - input 대신 change 이벤트 이후 동기화    

      `<input v-model.lazy="myMessage" type="text">`     

      - 바로 바뀌는 것이 아니라 엔터나 다른 곳 클릭 등으로 변화가 인식된 이후에 변경   

  - `.number`    

    - 사용자 입력이 자동으로 문자열 -> 숫자 변경      
    - 기본적으로 문자열을 반환하기때문에 해당 수식어 이용하여 숫자형으로 형변환 가능         

  - `.trim`   

    - 입력에 대한 trim 진행(양쪽 공백 제거)       



</br>   



## Lifecycle Hooks   

- 각 Vue 인스턴스는 생성될때 일련의 초기화 단계 거침   

  - 인스턴스를 DOM에 마운트 하는 경우 

    데이터가 변경되어 DOM을 업데이트 하는 경우   

    데이터 관찰 설정이 필요한 경우 등등      

- 해당 과정들에서 사용자 정의 로직을 실행할 수 있는 Lifecycle Hooks도 호출    

- 공식 문서를 통해 상세 동작 참고 가능     

  - `beforeCreate` ,  `created`,  `beforeMount` , `mounted` ... 등등    

  - created 훅은 vue 인스턴스가 생성된 후 호출      

    ```javascript
    created : function() {
          this.getImg()
        }
    // methods 생략 가능   
    ```



</br>   

## lodash library  

- 모듈성, 성능 및 추가 기능을 제공하는 JS 유틸리티 라이브러리    

- array, object 등 자료구조를 다룰 때 사용하는 유용하고 간편한 유틸리티 함수들 제공   

  `reverse`, `sortBy`, `range`, `random` 등...     

  ```javascript
  const reverseArray1 = array1.reverse()  // [4, 3, 2, 1]
  
  const sortedNums = _.sortBy(numbers2) //  [1, 3, 4, 7, 10]
  
  const num1 = _.range(4)				// [0, 1, 2, 3]
  
  const randomNums = _.random(0,5)	 // 1 - 0~5 사이에 랜덤 출력   
  
  const result = _.sampleSize([1,2,3,4,5,6,7,8],3)  // [8, 1, 3] - 3개 랜덤
  ```

  - 일반적인 JS Vanilla 에선 구현하기 어려운 range, random 등을 손쉽게 구현 가능   



- CDN 추가로 사용 가능   

  `<script src="https://cdn.jsdelivr.net/npm/lodash@4.17.21/lodash.min.js"></script>`   

- 공식문서에 더 많은 메서드 확인 가능  [공식](https://lodash.com/)       
