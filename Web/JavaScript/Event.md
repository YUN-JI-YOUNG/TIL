## Event

다양한 이벤트 유형 - 공식 문서 [참고](https://developer.mozilla.org/en-US/docs/Web/Events)   

- 네트워크 활동이나 사용자와의 상호작용 같은 사건의 발생을 알리기 위한 객체    

- 마우스 클릭 / 키보드 누르는 등 사용자 행동으로 발생 가능   

- `Element.click()` 등과 같이 특정 메서드를 호출하여 프로그래밍적으로도 만들어내기 가능  

- Event의 역할  

  - 검색, 새로고침 등의 작업을 수행하기 위해 mouse click 이벤트나 load 이벤트 등의 특정 이벤트가 발생하면 할 일을 등록    

- Event 기반 인터페이스   

  - UIEvent  
    - Event의 상속을 받으며 간단한 사용자 인터페이스 이벤트   
    - MouseEvent, KeyboardEvent, InputEvent, FocusEvent 등의 부모 객체 역할    

- Event handler

  - `addEventListener()`  

    - `EventTarget.addEventListener(type, listener[, options])`  

      - 지정한 이벤트가 대상에 전달될 때마다 호출할 함수 설정  

      - 이벤트를 지원하는 모든 객체(Element, Document, Window 등) 대상으로 지정 가능    

      - type 

        - `~ 하면`

        - 반응할 이벤트 유형(대소문자 구분 문자열)    

          ex> click 

      - listener   

        - `~한다` 
        - 지정된 타입의 이벤트가 발생했을 때 알림을 받는 객체   
        - EventListener 인터페이스 혹은 JS function 객체(콜백 함수)여야 함     

      ```python
      <script>
          
        const btn = document.querySelector("button")
      
        btn.addEventListener('click', function() {
          # console.log('버튼이 눌렸다!')
          console.log(event)	# 이벤트 객체 출력
          alert('버튼이 클릭되었습니다.')  	#  경고창 띄우기(JS 내장함수)
        })
      
      </script>
      ```

      - 다른 방법   

      ```python
        const alertMessage = function () {
          alert('메롱!!!')
        }
      
        const myButton = document.querySelector('#my-button')
        myButton.addEventListener('click',alertMessage)
      ```

      - Input 이벤트   

        ```python
          <p id="my-paragraph"></p>
        
          <form action="#">
            <label for="my-text-input">내용을 입력하세요.</label>
            <input id="my-text-input" type="text">
          </form>
        
            const myTextInput = document.querySelector('#my-text-input')
            myTextInput.addEventListener('input',function(event){
              const myPtag = document.querySelector('#my-paragraph')
              # // myPtag.innerText = event.data
              myPtag.innerText = event.target.value
            })
        ```

        - event.data 라고 하면, data는 입력값으로 갱신되므로 해당 값만 출력됨   

          `ex>` 안녕하세요를 입력한다고 하면, `안` 만 출력되고, `녕` 만 출력되는 형태   

      

      - ColorInput 이벤트   

        ```python
        <h2>Change My Color</h2>
          <label for="change-color-input">원하는 색상을 영어로 입력하세요.</label>
          <input id="change-color-input"></input>
        
        
          const colorInput = document.querySelector('#change-color-input')
          colorInput.addEventListener('input',function(event){
            const h2Tag = document.querySelector('h2')
            h2Tag.style.color = event.target.value  
          })
        
        ```

        - 함수를 따로 빼서 사용  

          ```python
            const colorInput = document.querySelector('#change-color-input')
              const ChangeColor = function(){
              const h2Tag = document.querySelector('h2')
              h2Tag.style.color = event.target.value    
            }
              
            colorInput.addEventListener('input',ChangeColor)
          ```

          

- Event 취소   

  `Event.preventDefault()` 

  - 현재 이벤트의 기본 동작 중단   

    - a 태그의 기본동작(링크 이동) / form 태그의 기본동작(form 데이터 전송)     

    - `ex>` form 칸에 입력하여 제출했을 때, 해당 페이지에 이미지를 띄우는 등의 변화를 일으키고 싶은 경우 

      -> 기본 동작을 중단하지 않으면 데이터가 전송되며 페이지가 사라짐(=주소 변경)       

  

  - 이벤트를 취소할 수 있는 경우, 이벤트의 전파를 막지 않고 해당 이벤트 취소   

    ```python
    const formTag = document.querySelector('#my-form')
    
    formTag.addEventListener('submit', function (event) {
        console.log(event)
        event.preventDefault()
        event.target.reset()
    })
    ```

    - form 태그는 데이터를 전송하므로 reset 메서드까지 필요     

  - 취소할 수 없는 이벤트도 존재   

    - 대표적으로 scroll 이벤트    
    - `event.cancelable` 속성에 대한 값으로 이벤트의 취소 가능 여부 확인 가능   

