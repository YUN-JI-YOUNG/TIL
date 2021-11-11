## Vue router

- Vue.js 공식 라우터    

- router   

  - 위치에 대한 최적의 경로를 지정 -> 이 경로를 따라 데이터를 다음 장치로 전향시키는 장치    

- 라우트에 컴포넌트를 매핑한 후 어떤 주소에서 렌더링할 지 알려줌     

- Single Page Application 상에서 라우팅을 쉽게 개발할 수 있는 기능 제공      

- Vue router 시작하기       

  - 프로젝트 생성 및 이동   

    `vue create my-router-app`   > `cd my-router-app`   

  - Vue Router plugin 설치    

    `vue add router`      

    - 만약 기존 프로젝트 진행 중 추가하게되면 App.vue를 덮어쓰므로 다음 명령을 실행하기 전, 필요한 경우 파일을 백업(커밋)해야 함     

     

- Vue Router로 인한 변화   

  - App.vue 코드 변화   
  - router/index.js 생성      
    - 라우트에 관련된 정보 및 설정이 작성되는 곳   
  - views 디렉토리 생성       

</br>      

### Vue Router          

- 필요한 이유   

  - SPA 등장 이전엔, 서버가 모든 라우팅을 통제하며 요청 경로에 맞는 HTML 제공   
  - SPA 등장 이후엔, 서버는 `index.html` 하나만 제공하고 이후 모든 처리는 HTML 위에서 JS 코드 활용하여 진행 -> 요청에 대한 처리를 서버가 하지 X        

  

  - 즉, SSR은 라우팅에 대한 결정권을 서버가 가지지만,     

    CSR에선 라우팅에 대한 결정권을 클라이언트가 가짐     

    (= 클라이언트는 더이상 서버로 요청을 보내지 않고 응답받은 HTML 문서안에서 주소가 변경되면 특정 주소에 맞는 컴포넌트 렌더링 )     



- 결국 Vue Router는 라우팅의 결정권을 가진 Vue.js 에서 라우팅을 편리하게 할 수 있는 Tool을 제공해주는 라이브러리    

</br>    

#### router-link   

`<router-link to="/">Home</router-link>`   

- 사용자 네비게이션을 가능하게 하는 컴포넌트    

- 목표 경로는 `to` prop로 지정   

- HTML5 히스토리 모드에서 router-link는 클릭 이벤트를 차단하여 페이지가 다시 로드되는 것 방지     

  = a 태그지만 기본 GET 요청을 보내는 이벤트를 제거한 형태로 구성       

  - 히스토리 모드   
    - HTML History API를 사용하여 router를 구현한 것      
      - History API   
        - DOM의 Window 객체는 history 객체를 통해 브라우저의 세션 기록 접근 방법 제공    
        - history 객체는 사용자를 방문 기록 앞/뒤로 보내거나 기록의 특정 지점으로 이동하는 등 유용한 메서드와 속성 보유      
    - 브라우저의 히스토리는 남기지만 실제 페이지는 이동하지 않는 기능 지원   
    - 페이지를 다시 로드하지 않고 URL 탐색 가능    
      - SPA의 단점 중 하나인 URL이 변경되지 않는다는 것을 해결       

</br>    

#### router-view  

`<router-view/>`      

- 주어진 라우트에 대해 일치하는 컴포넌트를 렌더링하는 컴포넌트    
- 실제 컴포넌트가 DOM에 부착되어 보이는 자리 의미    
- router-link를 클릭하면 해당 경로와 연결되어있는 index.js에 정의한 컴포넌트가 위치함       
- 닫는 태그만 존재     



</br>  
<hr>    

### Named Routes     

`<router-link :to="{name:'Home'}">Home</router-link>`     

- 이름을 가지는 라우트   
- 명명된 경로로 이동하려면 객체를 vue-router 컴포넌트 요소의 prop에 전달      
  - router/index.js에 정의된 라우트들 중 일치하는 이름의 경로로 이동    

</br>     

#### 프로그래밍 방식 네비게이션       

- vue-link를 사용하여 선언적 탐색을 위한 a 태그 생성 외에도 router의 인스턴스 메서드를 사용하여 프로그래밍 방식으로 같은 작업 수행 가능    

  | 선언적 방식              | 프로그래밍 방식    |
  | ------------------------ | ------------------ |
  | `<router-link to="...">` | `router.push(...)` |

  ```vue
  <template>
  <button @click="moveToHome">Home으로 이동</button>
  </template>
  
  
  <script>
  export default {
    name:'About',
    methods:{
      moveToHome: function(){
        this.$router.push({name:'Home'})
      }
  
    }
  }
  </script>
  ```

  - push 내부 작성 예시   

    `router.push('/')` , `router.push({path: 'home'})` , `router.push({name:'user', params: {userId:'123'}})` ,  

    `router.push({path: 'register', query : {plan: 'private'}})`        

</br>    
<hr>    

#### Dynamic Route Matching   

- 동적 인자 전달     

- 주어진 패턴을 가진 라우트를 동일 컴포넌트에 매핑해야 하는 경우    

  `ex` >  모든 User에 대해 동일 레이아웃을 가지지만, User Id에 따라 따르게 렌더링 되어야 하는 User 컴포넌트 예시    

- 동적인자는 `: (콜론)` 으로 시작       

- 컴포넌트에서 `this.$route.params` 로 사용 가능    

  - 위의 프로그래밍 방식 네비게이션은 $router 고, 지금은 $route . 헷갈리지 않기!     

  | pattern                            | matched path          | $route.params                    |
  | ---------------------------------- | --------------------- | -------------------------------- |
  | /user/:userName                    | /user/john            | {username:'john'}                |
  | /user/:userName/article/:articleId | /user/john/article/12 | {username:'john', articleId: 12} |

</br>    

#### components & views   

- 기본적으로 작성된 구조에서 components 폴더와 views 폴더 내부에 각 다른 컴포넌트 존재    

- 컴포넌트를 작성할 때 정해진 구조가 있는 건 아니지만, 주로 아래와 같이 구조화하여 활용   

- **App.vue**   

  - 최상위 컴포넌트   

- **views /** 

  - router(index.js)에 매핑되는 컴포넌트 모아두는 폴더    

- **components/**   

  - router에 매핑된 컴포넌트 내부에 작성하는 컴포넌트를 모아두는 폴더         

    `ex` >   `Home` 컴포넌트 내부에 `HelloWorld` 컴포넌트 등록      



</br>    

<hr>    




### .env.local  

- 깃헙 등에 코드를 올릴 때, 공유되면 안되는 정보를 가려서 올릴 때 사용   

- 프로젝트 최상단에 `.env.local` 파일 생성   

  -> VUE_APP_{keyname} = {value} 형식으로 작성     

  ```vue
  VUE_APP_YOUTUBE_API_KEY=AIzaSy...b7d1HI
  
  const API_KEY = process.env.VUE_APP_YOUTUBE_API_KEY
  ```

  - key 이름을 어떻게 지어도 상관없지만, **반드시** VUE_APP_ 으로 시작해야함!    



