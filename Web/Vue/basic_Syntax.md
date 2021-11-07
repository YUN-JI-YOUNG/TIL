## Basic Syntax  

### Vue instance 

- 모든 Vue 앱은 Vue 함수로 새 인스턴스를 만드는 것부터 시작   
- Vue 인스턴스를 생성할 때는 Options 객체 전달   
- 여러 Options를 사용하여 원하는 동작 구현   
- Vue Instance === Vue Component      
  - 컴포넌트는 작고 독립적이며 재사용할 수 있는 것     
  - 거의 모든 유형의 애플리케이션 인터페이스를 컴포넌트 트리로 추상화 가능    

### Options/DOM    

#### el   

- Vue 인스턴스에 연결(마운트)할 기존 DOM element 필요      

- CSS 선택자 문자열 or HTML Element로 작성   

- new를 이용한 인스턴스 생성때만 사용   

  ```javascript
  const app = new Vue({
  	el : '#app',
  })
  ```

#### data  

- Vue 인스턴스의 데이터 객체   
- Vue 인스턴스의 상태 데이터 정의   
- Vue template에서 interpolation을 통해 접근 가능     
  - interpolation : 데이터 보간법  `{{ }}`   
- v-bind, v-on과 같은 directive에서도 사용 가능   
- Vue 객체 내 다른 함수에서 `this` 키워드 사용하여 접근 가능        
- app.data.message가 아닌 app.message로 접근하는 이유    
  - 인스턴스가 생성된 이후 원래 데이터 객체는 `vm.$data` 로 접근 가능   
  - Vue 인스턴스는 데이터 객체에 있는 모든 속성을 프록시(대리)하므로 `vm.a` 과 `vm.data.a` 는 동일하여 `.data`는 생략가능          
  - `_` 또는 `$` 로 시작하는 속성은 Vue 내부 속성 및 API 메소드와 충돌할 수 있으므로 Vue 인스턴스에서 프록시 되지 않음 = `vm.$data._property` 로 접근해야함        
- 화살표 함수는 사용 금지    
  - 화살표 함수가 부모 컨텍스트를 바인딩하기 때문에, 'this'는 Vue 인스턴스를 가리키지 X    

```javascript
const app = new Vue({
	el : '#app',
    data:{
        message:'Hello',
    }
})
```

#### methods  

- Vue 인스턴스에 추가할 메서드  
- Vue template에서 interpolation을 통해 접근 가능  
- v-on과 같은 directive에서도 사용 가능   
- Vue 객체 내 다른 함수에서 `this` 키워드 사용하여 접근 가능  
- data와 마찬가지로 화살표 함수 사용 금지   
  - `this`가 Vue 인스턴스가 아니게 되므로 `this.a` 는 정의되지 않음     

#### this

- Vue 함수 객체 내에서 vue 인스턴스를 가리킴   
- JS 함수에서의 this 키워드는 다른 언어와 조금 다르게 동작하는 경우 존재    
  - 파이썬의 self 등은 자기자신을 가리키지만, JS에서의 this는 자기자신을 가리키지 않는 경우가 존재     



<hr>     


</br>    

### Options/Data    

#### computed    

- 데이터를 기반으로 하는 계산된 속성   

- 함수의 형태로 정의하지만 함수가 아닌 함수의 반환 값이 바인딩       

- 종속된 데이터에 따라 미리 저장(캐싱)    

- 종속된 데이터가 변경될때만 함수 실행   

  - 어떤 데이터에도 의존하지 않는 computed 속성은 절대 업데이트 X   

- 반드시 return 값 필요    

  - watch는 return 값 없어도 ok          

- methods와 비교됨   

  - computed 속성 대신 methods 에 함수 정의 가능      

    - 최종 결과는 동일    

  - computed 속성은 종속 대상을 따라 미리 저장된다는 차이 존재      

  - computed 속성은 종속 대상이 변하지 않는 한 계산되어 있는 결과 반환    

    methods는 호출하여 렌더링을 다시 할때마다 항상 함수 실행    



#### watch   

- 데이터를 감시하며, 데이터에 변화가 일어났을 때 실행되는 함수      

- computed도 데이터에 변화가 있을 때 실행되지만, 몇가지 차이점 존재     

  - computed 

    `(특정 값이 변동하면/ 해당 값을 다시 계산해서 보여준다)`   

    - 특정 데이터를 직접적으로 사용/가공하여 다른 값으로 만들때 사용   
    - 속성은 `계산해야하는 목표 데이터를 정의`하는 방식으로 SW 공학에서 이야기하는 선언형 프로그래밍 방식       

  - watch 

    `(특정 값이 변동하면 / 다른 작업을 한다)`  `= 일종의 트리거 역할`       

    - 특정 데이터의 변화 상황에 맞춰 **다른 data 등을 바꿀때** 주로 사용     
    - 감시할 데이터를 지정하고 그 `데이터가 바뀌면 특정 함수 실행`하는 방식으로 SW 공학에서 이야기하는 명령형 프로그래밍 방식     

  - watch는 변수명을 감시하는 데이터명으로 설정      

    computed는 미리 계산해둔 값을 저장할 변수명을 따로 설정     

  - watch는 변경 전 값과 변경 후 값을 모두 활용 가능     

    computed는 해당 값 자체를 변경하여 저장       



<hr>  


</br>   

### Options/Assets  

#### filter  

- 텍스트 형식화를 적용할 수 있는 필터    

- interpolation 또는 v-bind를 이용할 때 사용 가능     

- 필터는 자바스크립트 표현식 마지막에 `|` 와 함께 추가 가능   

- chaining 가능   

  ```javascript
  //dom
  // 현재 chaining
  <div id="app">
    <p>{{ numbers | getOddNums | getUnderTenNums }}</p>
  </div>
  
  // vue
  
  const app = new Vue({
    el: '#app',
    data: {
      numbers: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15],
    },
    filters: {
   // nums 는 dom에서 파이프라인(|) 앞에 있는 numbers 의미    
      getOddNums: function (nums) {  
        const oddNums = nums.filter(num => {
          return num % 2
        })
        return oddNums
      },
  // nums는 dom에서 파이프라인 | 앞에 있는 getOddNums 에서 return된 값들 의미     
      getUnderTenNums: function (nums) {
        const underTen = nums.filter(num => {
          return num < 10
        })
        return underTen
      }
    }
  })
  ```

  

<hr>   


</br>     

