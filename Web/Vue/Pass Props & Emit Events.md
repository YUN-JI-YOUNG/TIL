## Pass Props & Emit Events     

###### `props는 아래로, events는 위로`     

#### 컴포넌트 작성   

- Vue app은 자연스럽게 중첩된 컴포넌트 트리로 구성   

- 컴포넌트가 부모-자식 관계가 구성   

  -> 부모-자식 사이에 의사소통 필요      

- 부모는 자식에게 `데이터`를 전달(**pass props**)     

  자식은 자신에게 일어난 일을 부모에게 `메시지`를 보냄 (**Emit event**)       

  -> 자식은 데이터 전달은 불가   

- 부모와 자식이 명확하게 정의된 인터페이스를 통해 격리된 상태 유지 가능        

</br>    



#### 컴포넌트 구조      

-> `vue` 쓰고 엔터치면 기본 틀 자동완성 가능    

- 템플릿 (HTML)     

  - 평소 작성하던 body 부분     

  - 반드시 Root Element가 1개 존재해야 함!     

    -> 주로 `div` 태그 사용      

    ->  vue 3에서 변경됨     

- 스크립트(JavaScript)         

  - vue 인스턴스에 작성하던 부분    
  - 컴포넌트 정보, 데이터, 메서드 등 vue 인스턴스를 구성하는 대부분을 작성     

- 스타일 (CSS)     

  - CSS가 작성되어 컴포넌트의 스타일 담당        

</br>    



#### 컴포넌트 등록    

- 불러오기   

  - `import HelloWorld from './components/HelloWorld.vue'`     

    = `import HelloWorld from '@/components/HelloWorld.vue'`    

    (@ === src 폴더)   

    = `import HelloWorld from '@/components/HelloWorld'`     

    .vue 생략 가능    

- 등록하기     

  - ```javascript
    export default {
      name: 'App',
      components: {
        HelloWorld
      }
    }
    ```

- 보여주기     

  - 이후 템플릿에서 `<HelloWorld msg="Welcome to Your Vue.js App"/>` 처럼 사용   

    - 부모 컴포넌트에서 msg(데이터)를 내려보내는 중(props)         

    - 이후 자식 컴포넌트에서 선언 후 사용 가능        

      ```vue
      // 자식 컴포넌트에서의 선언   
      
      export default {
        name: 'HelloWorld',
        props: {
          msg: String
        }
      }
      
      // 자식 컴포넌트에서 사용   
      <h1>{{ msg }}</h1>
      ```

</br>    



#### 컴포넌트 데이터   

- 컴포넌트의 데이터는 **반드시 함수**여야함 !!   

- 기본적으로 각 인스턴스는 모두 같은 data 객체를 공유하므로 새로운 data 객체 반환 필요    

  -> 각 인스턴스가 모두 같은 data 객체를 공유하는 상황 방지 목적    

```vue
export default {
  name: 'App',
  components: {
    About,
  },
  data: function(){
    return {
      parentData:'hello!!',
    }
  }
}
```

</br>    

<hr>    

### props   

- 부모(상위) 컴포넌트의 정보를 전달하기 위한 사용자 지정 특성     

- 자식(하위) 컴포넌트는 props 옵션을 사용하여 수신하는 props를 명시적으로 선언해야 함      

  = 데이터는 props 옵션을 사용하여 자식 컴포넌트로 전달      

- 모든 컴포넌트 인스턴스는 자체 격리된 범위가 있어서 자식 컴포넌트의 템플릿에선 상위 데이터를 직접 참조 불가       

- props 이름 컨벤션   

  - 선언 시 : camelCase   

  - in template(HTML) : kebab-case    

    - `-`로 연결된 규칙      

      ```html
      <template>
        <div id="app">
          <img alt="Vue logo" src="./assets/logo.png">
          <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
          <about 
          my-message="this is prop data"
          :parent-data="parentData"
          ></about>
      
        </div>
      </template>
      ```

      - 템플릿에선 케밥 케이스 사용    

      ```vue
      export default {
        name:'About',
        props:{
          myMessage:String,
          parentData : String,
        }
      }
      ```

      - 실제 사용할 땐 카멜 케이스로 변환하여 사용         

- 주의할 점      

  - 숫자를 전달하고자 할 땐, Static 구문이 아니라 `v-bind` 사용 필요      

    - 값이 JavaScript 표현식으로 평가되도록 하기 위함     

    ```vue
    <comp some-prop="1"></comp>
    <!-- 이렇게 static 구문이 아니라 -->
    
    <comp :some-prop="1"></comp> 
    <!-- 이렇게 v-bind 사용 필요 -->   
    ```

- 모든 props는 하위 속성과 상위 속성 사이의 **단방향** 바인딩을 형성      

- 부모의 속성이 변경되면 자식 속성에게 전달되지만, 반대 방향은 X     

  - 자식 요소가 의도치 않게 부모 요소의 상태를 변경하여 앱의 데이터 흐름을 이해하기 어렵게 만드는 일 방지     

- 부모 컴포넌트가 업데이트될때 마다 자식 요서의 모든 prop들이 최신 값으로 업데이트    

</br>   
<hr>    

### Emit event   

###### Listening to Child Components Events      

- 부모 컴포넌트는 자식 컴포넌트가 사용되는 템플릿에서 `v-on` 을 사용하여 자식 컴포넌트가 보낸 이벤트 청취 

  = v-on을 이용한 사용자 지정 이벤트    

- `$emit(eventName)`   

  - 현재 인스턴스에서 이벤트를 트리거    
  - 추가 인자는 리스너의 콜백 함수로 전달       

  ```vue
  <!-- About.vue (하위) -->
  
  <template>
    <div id="app">
      <input 
      type="text"
      @keyup.enter="childInputChange"
      v-model="childInputData"
      >
    </div>
  </template>
  ```

  ```vue
  <script>
  export default {
    name:'About',
    data: function(){
      return{
      childInputData: null,
      }
    },
    props:{ },
    methods:{
      childInputChange:function(){
        this.$emit('child-input-change', this.childInputData)
      }
    }
  }
  </script>
  ```

  - `$emit` 인스턴스 메서드를 이용하여 엔터를 누를 때마다(`@keyup.enter`)   `child-input-change` 이벤트 발생시키고, 그때, `childInputData` 데이터를 보냄       

  - 부모 컴포넌트에선 `v-on direcitve` 를 사용하여 `child-input-change` 이벤트 청취   

    ```vue
    <!-- App.vue (상위) -->
    
    <template>
      <div id="app">
        <about 
        @child-input-change="parentGetChange"
        ></about>
      </div>
    </template>
    ```

    ```vue
    <script>
    import About from './components/About.vue'
    
    export default {
      name: 'App',
      components: {
        About,
      },
      data: function(){
        return {}
      },
      methods: {
        parentGetChange: function(inputData){
        console.log(`About으로부터 ${inputData} 받았음!`)
      }}
    }
    </script>
    ```

    - 여기서 함수 인자로 받은 `inputData`는 자식 컴포넌트에서 보냈던 `childInputData`       

    - 이렇게 받은 데이터들은 `payload` 배열에서 확인 가능       



- event 이름 컨벤션    

  - 컴포넌트 및 props와 달리 이벤트는 자동 대소문자 변환 X     

  - HTML의 대소문자 구분을 위해 DOM 템플릿의 v-on 이벤트 리스너는 항상 소문자 변환되기 때문에 `v-on:myEvent` 는 자동으로 `v-on:myevent` 로 변환     

    `this.$emit('myEvent')`   -> `<my-component @my-event="..."></my-component>`   는 동작하지 않음       

    -> 그러므로 항상 **kebab-case** 사용 권장      

    특히 `this.$emit` 의 인자를 사용할땐 꼭 kebab-case 사용 권장     

