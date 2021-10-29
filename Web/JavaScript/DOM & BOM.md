## DOM (Document Object Model)       

- HTML , XML 과 같은 문서를 다루기 위한 문서 프로그래밍 인터페이스   

- 문서를 구조화하고 구조화된 구성 요소를 하나의 객체로 취급하여 다루는 논리적 트리 모델   

- 각 요소는 개체로 취급하며 단순한 속성 접근, 메서드 활용 등 프로그래밍 언어적 특성을 활용한 조작 가능   

- 주요 객체    

  - window   
    - DOM을 표현하는 창  
    - 가장 취상위 객체 
      - `window.document` 로 document로 접근가능하지만, window는 생략하고 `document` 만으로도 접근 가능       
  - document    
    - 페이지 컨텐츠의 Entry Point   
    - `<body>` 등 수많은 다른 요소 포함  
  - BOM - navigator, location, history, screen 등    

  

  - 상속 구조   
    - EventTarget    
      - Event Listener를 가질 수 있는 객체가 구현하는 DOM 인터페이스   
    - Node
      - 여러 DOM 타입들이 상송하는 인터페이스   
    - Element   
      - 모든 객체가 상속하는 가장 범용적인 기반 클래스    
      - 부모인 Node와 그 부모인 EventTarget의 속성 상속   
    - Document   
      - 브라우저가 불러온 웹 페이지이며 DOM 트리의 진입점 역할 수행   
    - HTMLElement   
      - 모든 HTML 요소  
      - 부모인 Element 속성 상속     

- 해석   

  - 파싱   

    - 구문 분석, 해석
    - 브라우저가 문자열을 해석하여 DOM Tree로 만드는 과정   

  - 조작   

    - 개발자도구 > Console 에서 조작 가능   

      ```python
      document 
      document.title = 'ssafy'   
      
      # 웹페이지 이름(탭 이름) 변경
      ```

      -> 내 화면만 바뀌는 것이고, 새로고침하면 원상복귀      



### BOM (Browser Object Model)    

- 자바스크립트가 브라우저와 소통하기 위한 모델    

- 브라우저 창이나 프레임을 추상화해서 프로그래밍적으로 제어할 수 있도록 제공하는 수단   

  - 버튼, URL 입력창, 타이틀 바 등 브라우저 윈도우 및 웹 페이지 일부분 제어 가능   

- window 객체는 브라우저의 창을 지칭하며 모든 브라우저로부터 지원      

- 조작  

  - 개발자도구 > Console 에서 조작 가능 

    ```python
    # 탭 창
    window.open()
    
    # 인쇄 창
    window.print()
    
    # 메세지 확인, 취소 등이 있는 대화상자 창 오픈
    window.confirm()
    
    window.document
    ```



### JavaScript Core  

- 프로그래밍 언어   

```python
const numbers = [1,2,3,4,5]
for (let i=0; i < numbers.length; i++){
    console.log(numbers[i])
}
```

