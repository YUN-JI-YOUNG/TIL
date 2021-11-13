## Vuex    

### Vuex Intro   

###### 'Statement management pattern' + Library for vue.js     

- 상태 관리 패턴 + 라이브러리      
  - 상태 관리 패턴    
    - 컴포넌트의 공유된 상태 추출 + 이를 전역에서 관리하도록 함      
      - 중앙 저장소(store)에 state를 모아놓고 관리      
    - 컴포넌트는 커다란 view가 되며, 모든 컴포넌트는 트리에 상관없이 상태에 액세스 하거나 동작 트리거 가능     
    - 상태 관리 및 특정 규칙 적용과 관련된 개념을 정의, 분리하여 코드 구조와 유지 관리 기능 향상          
    - 규모가 큰 (= 컴포넌트의 중첩이 깊은) 프로젝트에서 매우 효율적     
      - 각 컴포넌트에서는 중앙 집중 저장소의 state만 신경 쓰면 되고, 동일 state를 공유하는 다른 컴포넌트들도 동기화되므로              

- 상태(state = data)를 전역 저장소로 관리할 수 있도록 지원하는 라이브러리     
  - 상태가 예측 가능한 방식으로만 변경될 수 있도록 보장하는 규칙 설정   
  - 앱의 모든 컴포넌트에 대한 **중앙 집중식 저장소** 역할     
  - state    
    - state는 곧 data이며 해당 앱의 핵심    
    - 중앙에서 관리하는 모든 상태 정보    
- Vue 의 공식 devtools과 통합되어 기타 고급기능 제공       



##### 단방향 흐름에 의존한 상태 관리 (**pass props & emit events**)   

- 각 컴포넌트가 독립적으로 데이터 관리       

  -> 컴포넌트의 중첩이 깊어지면 (= 깊이가 깊어지면) 다른 컴포넌트로의 데이터 전달 구조가 복잡해져서 불편함                  

- 부모 자식 간 컴포넌트 관계가 단순하거나 깊이가 깊지 않은 경우엔 문제 X   

  -> 오히려 몇단계만 거치면 데이터를 이동시킬 수 있고 훨씬 직관적으로 데이터흐름 파악가능       

  -> 하지만 상태를 전달할 때, 상 > 하로만 가능하여 규모가 커지면 상태를 공유하는 컴포넌트의 상태 동기화 관리가 어려워짐        



##### vuex를 활용한 상태 관리    

- 상태의 변화에 따른 여러 흐름을 모두 관리해야 하는 불편함 해소 필요    

  -> 상태는 데이터를 주고받는 컴포넌트 간의 관계도 고려해야해서 상태 흐름관리가 매우 중요         

- 이러한 상태를 올바르게 관리하는 저장소의 필요성 발생    

  -> 상태를 한곳에 모두 모아놓고 관리하며, 상태의 변화는 오로지 Vuex가 관리하여 해당 상태를 공유하고 있는 모든 컴포넌트는 변화에 반응하도록  함    


</br>   
<hr>   
</br>    


### Vuex 핵심 컨셉    

### state    

- 중앙에서 관리하는 모든 상태 정보(data)    

- Vuex는 single state tree 사용    

  -> 해당 단일 개체는 모든 앱 상태를 포함하는 원본 소스(single source of truth)의 역할       

  = 각 앱마다 하나의 저장소만 갖게 된다는 것        

- 여러 컴포넌트 내부에 있는 특정 state를 중앙에서 관리하게 됨      

  - 이전엔 state를 찾기 위해 각 컴포넌트 확인 필요     
  - Vuex를 활용하면 Vuex store에서 각 컴포넌트에서 사용하는 state 파악 가능    

- state가 변화하면 해당 state를 공유하는 여러 컴포넌트의 DOM은 알아서 렌더링   

- 각 컴포넌트는 이제 Vuex store에서 state 정보를 가져와서 사용     

- 단, Vuex를 사용한다고 해서 Vuex Store에 모든 상태를 넣어야 하는 것은 아님!!    

  -> 다른 컴포넌트와 공유하지 않는 state는 store에 넣을 필요 없음    



### Mutations   

- 실제로 state를 변경하는 유일한 방법   

- mutations의 handler함수 는 반드시 동기적!   

  - 만약 콜백함수 같은 비동기적 로직을 사용한다면, state가 변화하는 시점이 의도와 달라질 수 있고, 콜백이 실제로 호출될 시기를 알 수 있는 방법 X (= 추적 불가)       

- handler 함수 이름은 대문자(상수)로 사용할 것을 권장하며, 첫 번째 인자는 항상 state              

  - state, 즉 data를 직접적으로 조작하므로 명시적으로 표현하여 mutations 함수라는 것을 바로 알 수 있도록 함         

    -> linter 같은 tool에서 디버깅하기에 유용    

  - `CREATE_TODO : function(state, payload) { }`          

    -> 함수 내부에 `state.'state명'.'메서드'` 로 state 조작 가능   

- Actions에서 `commit()` 메서드에 의해 호출        

  - `context.commit('호출 mutations', payload)`    

    -> **payload : state가 아닌, 넘겨줄 데이터**          

- 컴포넌트에서 mutations 호출할 경우엔 `this.$store.commit('mutations 핸들러함수명')`  처럼 작성하지만, 올바른 구조는 아님 (= actions 에서 호출해야 하므로)          



### Actions   

- state를 변경하는 대신 mutations를 `commit()` 메서드로 호출하여 실행    

- mutations와 달리 state를 직접 변경하지 않으므로 비동기 작업 포함 가능  

  - 백엔드 API와 통신하여 Data Fetching 등의 작업 (= axios 등)      

- context 객체 인자를 받음   

  - context 객체를 통해 store.js 파일 내에 있는 모든 요소의 속성 접근 & 메서드 호출 가능   

    = context 내부에 commit(mutations), dispatch(actions), getters, state 모두 포함          

    -> 이를 통해 state를 조작할 수는 있지만, state는 오로지 Mutations를 통해서만 조작    

    -> 명확한 역할분담을 통해 서비스 규모가 커져도 state를 올바르게 관리하기 위함       

  - `createTodo : function(context, payload) { }`      

    -> 함수 내부에 `context.commit('mutations 함수명', payload)` 로 mutations 호출 

  

  - **구조 분해 할당**으로 축약 가능    

    `createTodo:function({commit,state},todoItem)` 

  ​	=> 첫번째 인자로 context가 들어오므로, `{commit,state} = context` 를 축약해서 쓴 것   

  ​	=> 구조 분해 할당으로 썼으면, commit 호출도 `context.commit()`이 아닌 `commit()`      



- 컴포넌트에서 `dispatch()` 메서드에 의해 호출     
  - `this.$store.dispath('actions 함수명', payload)`        



### Getters   

- state를 변경하지 않고 활용하여 계산 수행    

  - computed 속성과 유사      
  - state를 특정한 조건에 따라 구분 / 계산     

- 실제 계산된 값을 사용하는 것처럼 getters는 저장소의 state를 기준으로 계산      

  `ex>` state의 todoList에서 완료된 todo 목록만 필터링해서 출력하고자 할 경우, getters에 iscompleted의 값이 true인 요소를 필터링해서 미리 담아놓기 가능   

- computed 속성과 마찬가지로 getters의 결과는 state 종속성에 따라 캐시되고, 종속된 데이터가 변경된 경우에만 재계산       

- 첫번째 인자로 state가 들어오며, 필요 시 index.js 에 추가로 작성    

- computed 속성에서 `return this.$store.getters.'getters 함수명'`  로 호출        

  -> 아래의 Spread Syntax + Component Binding Helper인 `mapGetters` 를 활용하여 `...mapGetters([' ', ' ',])`  처럼 축약 가능       
  
  </br>   

<hr>     



`vue add vuex`  로 vue CLI에서 vuex 추가    

##### vuex 로 인한 변화  

- store 디렉토리 생성    

  -> index.js 생성    

  = Vuex 핵심 컨셉들이 작성되는 곳     

<hr>

</br>     

##### 프로젝트 진행      

- JavaScript의 Date 객체      
  - 1970년 1월 1일 UTC 자정과의 시간 차이를 **밀리초** 단위로 나타내는 정수 값              
    - UNIX 시간은 1970년 1월 1일 자정과의 시간 차이를 **초** 단위로 나타낸 것       
  - Date 객체의 중심을 구성하는 시간값은 UTC 기준     
  - getTime 메서드   
    - 1970년 1월 1일 UTC 자정으로부터의 경과시간을 밀리초 단위로 반환     
    - Date 가 기준 시간 이전을 나타낸 경우 음수 값 반환   
  - 사용 이유   
    - DB에 일반 숫자 타입 저장 가능하며, key 값으로 활용 가능        

- Vuex Store의 state에 접근   
  - 템플릿 에선 -> `$store.state`        
  - 인스턴스 에선 -> `this.$store.state`    

- 작업 흐름    
  1. 컴포넌트에서 `this.$store.dispatch()` 로 actions 호출   

  2. actions에서 동기+비동기 작업 이후 `context.commit()` /`commit()`로 mutations 호출   

  3. mutations에서 state 조작    

  4. state가 변경되면 해당 state를 공유하는 컴포넌트들에 변경사항 적용        

</br>    

<hr> 




### JavaScript Spread Syntax    

###### 전개구문   

- 배열,문자열처럼 반복가능한(iterable) 문자를 요소(배열 리터럴의 경우)로 확장하여, 0개 이상의 key-value 쌍으로 된 객체로 확장 가능    

- `...` 을 붙여 요소 or 키가 0개 이상의 iterable object를 하나의 object로 간단하게     

  표현하는 법   

- ECMAScript 2015에서 추가되었으며, Spread Syntax의 대상은 반드시 **iterable** 객체     

- 함수, 배열, 객체 에서의 사용법이 모두 다름    

- 객체에서는 **객체 복사(shallow copy)**로 사용   

  - `{...obj1}` ㅣ 그대로 복사   

  - `{...obj1, ...obj2}` | 두 요소를 비교하여, 공통부분은 그대로 쓰고, 동일 키에 대해 다른 부분은 덮어쓰고, 동일 키가 아니라면 추가        

  - `ex`   

    ```vue
    <!-- 기존 mutations 에서의 todo update --> 
    
    UPDATE_TODO_STATUS:function(state, todoItem){
          ...
              return{
                title : todoItem.title,
                isCompleted :!todo.isCompleted,
    			date = new Date().getTime()
              }
            ...
    ```

    ```vue
    <!-- Spread Syntax 적용 -->
    
    UPDATE_TODO_STATUS:function(state, todoItem){
          ...
              return{
                ...todo,
                isCompleted :!todo.isCompleted,
              }
            ...
    ```

    - 반복을 피할 수 있음     



</br>     

<hr> 




### Component Binding Helper  

- JS Array Helper Method 를 통해 배열 조작을 편하게 하는 것과 유사   

  - 논리적인 코드 자체가 변하는 것이 아니라 쉽게 사용할수 있도록 되어 있는 것!    

- 종류   

  - mapState     

    - Computed와 State 매핑      

    - 해당 컴포넌트 내에서 매핑하고자 하는 이름이 index.js 에 정의해놓은 이름과 동일하면, 배열의 형태로 해당 이름만 문자열로 추가    

    - state를 객체 전개 연산자 (Object Spread Operator) 로 계산하여 추가    

      - mapState는 객체를 반환하고 그 객체를 `...` 연산자로 풀어서 새 object에 매핑    

        기존에 state에 직접 접근하여 값을 가져오던 것을   

        ```vue
          computed:{
            todos : function(){
              return this.$store.state.todos
            }
          }
        ```

        mapState 를 사용하여 가져올 수 있음      

        ```vue
        computed : {
          ...mapState([
            'todos'
          ])
        }
        
        <!--아래와 같이 쓸수도 있지만, state와 매핑되지 않은 값은 계산 불가-->
        computed : mapState([
          'todos',
        ])
        ```

  

  - mapGetters      

    - 위와 동일하게 Computed와 Getters 매핑    

  - mapActions     

    - 위와 동일하게 methods와 Actions 매핑    

    - 다만, 전해줘야하는 payload는 props 데이터로 넘겨줘야 함    

      즉, `@click="updateTodoStatus(todo)"`  처럼 인자 필요     

  - mapMutations      

    - 위와 동일하게 진행        

</br>        

<hr>   




### Local Storage      

#### vuex-persistedstate 

- Vuex state를 자동으로 브라우저의 Local Storage 에 저장해주는 라이브러리 중 하나    

  1. 설치    

     `npm i vuex-persistedstate`    

  2. index.js 에서 라이브러리 import   

     `import createPersistedState from 'vuex-persistedstate'`     

  3. 플러그인 작성   

     `plugins:[createPersistedState()],`     


  

