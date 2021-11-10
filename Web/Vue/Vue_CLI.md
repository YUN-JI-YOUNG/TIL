## Vue CLI        

### SFC (Single File Component)       

Vue 컴포넌트  === Vue 인스턴스  === `.vue` 파일         

- Vue 컴포넌트 기반 개발의 핵심 특징       
  - 컴포넌트    
    - 기본 HTML 엘리먼트를 확장하여 재사용 가능한 코드를 캡슐화하는데 도움    
    - CS에서는 다시 사용할 수 있는 범용성을 위해 개발된 소프트웨어 구성요소 의미    
    - 유지보수를 쉽게 만들어주며 재사용 측면에서옫 매우 강력한 기능 제공     



- 1개의 컴포넌트 === `.vue` 확장자를 가진 하나의 파일 안에서 작성되는 코드의 결과물    

- 화면의 특정 영역에 대한 HTML, CSS, JS 코드를 하나의 파일 `.vue` 에서 관리    

  = `.vue` 확장자를 가진 싱글 파일 컴포넌트를 통해 개발하는 방식       

- 단일 파일에서의 개발   

  - 처음 개발 시작할 때는 크게 신경쓸것이 없어 쉽게 개발 가능   

  - But, 코드의 양이 많아지면 변수관리가 힘들고 유지보수에 많은 비용    

    -> 그러므로 각 기능별로 파일을 나눠서 개발   

     - 처음 개발을 준비하는 단계에선 시간 소요가 증가하지만 이후 변수 관리 용이 + 기능별로 유지 보수 비용 감소      

- 한 화면 안에서도 기능 별로 각기 다른 컴포넌트 존재    

  - 각 컴포넌트는 여러개의 하위 컴포넌트 보유 가능    

- Vue 컴포넌트는 `const app = new Vue({})` 의 app을 의미 (= Vue 인스턴스 )        

  - 컴포넌트 기반의 개발이 반드시 파일 단위로 구분되어야 하는 것은 X    
  - 단일 `.html` 파일 안에서도 여러개의 컴포넌트를 만들어 개발 가능    

</br>    
<hr>   

### Vue CLI     

###### Vue Commend Line Interface    

- vue.js 개발을 위한 표존 도구   

- 프로젝트 구성을 도와주는 역할이며 Vue 개발 생태계에서  표준 tool 기준을 목표    

- 확장 플러그인, GUI, ES2015 구성요소 제공 등 다양한 tool 제공        

- 설치   

  -  `npm install -g @vue/cli`       

  - 버전 확인 -> `vue --version`    


</br>    

### node.js     

###### JavaScript Runtime Environment    

- 자바스크립트를 브라우저가 아닌 환경에서도 구동할 수 있도록 하는 JS 런타임 환경    
  - 브라우저 밖을 벗어날 수 없는 JS 언어의 태생적 한계 해결  
- 크롬 V8 엔진 제공   
  - 여러 OS 환경에서 실행 가능    
- 단순히 브라우저만 조작할 수 있던 JS를 SSR 아키텍처에서도 사용가능하도록 함    
- 2009 Ryan Dahl에 의해 발표    
- NPM (node pakage manager)    
  - 자바스크립트 언어를 위한 패키지 관리자    
    - 파이썬의 `pip` 와 동일하게 다양한 의존성 패키지 관리   
  - Node의 기본 패키지 관리자이며 Node 설치 시 함께 설치          

</br> 
<hr>   

### Vue 프로젝트 파일 구조         

- Vue CLI는 Babel, Webpack에 대한 초기 설정이 자동으로 설정       

#### Babel   

###### JavaScript compiler    

- `babel.config.js `     

- 자바스크립트의 ECMAScript 2015+코드를 이전 버전으로 번역, 변환해주는 도구   

- 과거 JS의 파편화와 표준화의 영향으로 코드의 스펙트럼이 매우 다양    

  - 그래서 최신 문법을 사용해도 이전 브라우저 or 환경에서 동작하지 않는 문제 발생    

- 원시코드(최신)를 목적 코드(구버전)으로 옮기는 번역기가 등장   

  -> 더이상 코드가 특정 브라우저에서 동작하지 않는 상황에 대해 크게 고민 X        

</br>    

#### node_modules    

- node.js 환경의 여러 의존성 모듈     

- 현재 환경에 대한 패키지 목록          

  -> 가상환경은 아니지만, 파이썬의 venv와 유사   

  => git에는 올라가지 않기때문에 만약 pull을 받았다면,      

  `npm install` 또는 `npm i` 명령어를 통해 동일 환경 세팅 가능       

- node.js를 이루고 있는 환경       

</br>    

#### Webpack       

###### static module bundler    

- **node_modules 내부**에 존재    

- 모듈 간의 의존성 문제를 해결하기 위한 도구   

- 프로젝트에 필요한 모든 모듈 매핑(문제없이 하나로 통합) + 내부적으로 종속성 그래프 빌드      

- **module**    

  - 스크립트 1개 === 모듈 1개  

    -> 모듈은 파일 1개 의미     

  - 브라우저만 조작가능하던 시기의 JS는 모듈 관련 문법 없이 사용 가능했었다.          

    but, JS와 애플리케이션이 복잡해지고 크기가 커지자 전역 스코프를 공유하는 형태의 기존 개발 방식의 한계 등장    

    -> 그래서 라이브러리를 만들어 필요한 모듈을 언제든 불러오거나 코드를 모듈 단위로 작성하는 등의 다양한 시도 발생     

  - 과거 모듈 시스템 (`AMD`, `CommonJS` , `UMD` ) 들이 존재했지만, 현재는 모듈 시스템이 2015년 표준으로 등재  

    -> 현재는 대부분의 브라우저와 Node.js가 모듈 시스템 지원     

  - Module 의존성 문제    

    - 모듈의 수가 많아지고 라이브러리 or 모듈간의 의존성(연결성)이 깊어지며 특정한 곳에서 발생한 문제가 어떤 모듈 간의 문제인지 파악하기 어려워짐    

      => Webpack은 모듈간의 의존성 문제를 해결하기 위해 등장     

- **Bundler**   

  - 모듈 의존성 문제를 해결해주는 작업 === `Bundling`    
  - 번들링을 해주는 도구가 Bundler   
    - Webpack은 다양한 Bundler 중 하나   
  - 여러 모듈을 하나로 묶어주고, 묶은 파일은 하나 or 여러개로 합침         
  - 번들링된 결과물은 더이상 순서에 영향받지 않고 동작   
  - 이외에도 `snowpack` , `parcel` , `rollup.js` 등의 다양한 모듈 번들러 존재         

</br>    

#### pubilc / index.html   

- Vue 앱의 뼈대    
- 실제 제공되는 단일 html 파일   

</br>    

#### src /  

- /assets   
  - webpack에 의해 빌드 된 static 파일 (정적 파일)   
- /components    
  - 하위 컴포넌트들(`.vue`)이 위치       
- / App.vue  
  - 최상위 컴포넌트        
- / main.js    
  - webpack이 빌드 시작시에 가장 먼저 불러오는 entry point    
  - 실제 단일 파일에서 DOM과 data를 연결했던것과 동일한 작업이 이루어지는 곳    
  - Vue 전역에서 활용할 모듈을 등록할 수 있는 파일     

</br>    

#### package.json   

- 프로젝트의 종속성 목록 + 지원되는 브라우저에 대한 구성 옵션  포함      
- package-lock.json과는 달리 무언가 패키지를 설치하면 계속 갱신됨      

</br>    

#### package-lock.json   

- node_modules에 설치되는 모듈과 관련된 모든 의존성 설정 및 관리    

- 팀원 및 배포환경에서 정확히 동일한 종속성을 설치하도록 보장하는 표현     

- 사용할 패키지의 버전 고정 + 개발 과정간 의존성 패키지 충돌 방지           

  - package.json과 달리 어느 한 시점을 고정시킬 수 있음 (== requirements.txt )    

    -> 업데이트 때문에 호환성 및 안정성 문제 등이 생기는 걸 방지할 수 있음     

- `requirements.txt` 와 비슷하지만 훨씬 용량 ↑         

<hr>    

</br>      

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

