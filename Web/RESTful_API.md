## HTTP

- Hyper Text Transfer Protocol
- 웹 상에서 컨텐츠를 전송하기 위한 약속   
- HTML 문서와 같은 리소스들을 가져올 수 있도록 하는 규칙, 약속   
- 웹에서 이루어지는 모든 데이터 교환의 기초(요청, 응답)   
  - 요청 - 클라이언트에 의해 전송되는 메시지    
    - Method / Path / version of the protocol / Headers     
    - HTTP request methods   
      - 주어진 자원에 대해 수행하고자하는 동작을 정의      
      - 예시
        - GET / POST / PUT / DELETE    
  - 응답 - 서버에서 응답으로 전송되는 메시지 
    - version of the protocol / status code / status message / Headers   
    - HTTP response status code   
      - 특정 HTTP 요청이 성공적으로 완료되었는지 여부 나타냄   
      - 5개의 그룹  
        - Informational responses ( 1xx 번대)
        - Successful responses ( 2xx번대 )
        - Redirection messages (3xx번대)
        - Client error responses (4xx번대)
        - Server error responses (5xx번대)   
      - [참고](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)    
- 특성   
  - 상태가 없다(stateless)   
  - 연결이 없다(connectless)
  - 위의 두 특성때문에 쿠키와 세션을 이용하여 서버 상태를 요청과 연결하도록 함    
  - 
- 웹에서의 리소스 식별   
  - HTTP 요청의 대상을 리소스(자원)라고 하며, 리소스는 문서, 사진, 기타 등등이 될 수 있음   
  - 각 리소스는 리소스 식별을 위해 HTTP 전체에서 사용되는 URI로 식별됨      



### URI (Uniform Resource Identifier)      

###### URI는 크게 URL과 URN으로 나눌수 있지만, URN을 사용하는 비중은 매우 적어서 일반적으로 URL은 URI와 같은 의미처럼 사용하기도 함   

- URI (Uniform Resource Identifier)  

  - 통합 자원 식별자   
  - 인터넷의 자원을 식별하는 유일한 주소      
  - 인터넷에서 자원을 식별하거나 이름을 지정하는데 사용되는 간단한 문자열     
  - 하위 개념 : URL, URN   
    - URL(Uniform Resource Locator)   
      - 통합 자원 위치   
      - 네트워크 상에서 자원이 어디 있는지 알려주기 위한 약속  
      - 과거엔 실제 자원의 위치를 나타냈지만, 현재는 추상화된 의미론적 구성   
    - URN (Uniform Resource Name)   
      - 통합 자원 이름  
      - URL과 달리 자원의 위치에 영향받지 않는 유일한 이름 역할   
        - URL은 위치에 따라 바뀔 수 있지만, URN 은 유일       
      - ISBN(국제표준도서번호) 등이 여기에 해당         

- 구조    

  `https ://` `www.example.com` `: 80` `/ path / to / myfile.html` / `?key = value` `#quick-start ` 

  - Scheme(protocol)   `https ://`
    - 브라우저가 사용해야 하는 프로토콜    
    - http(s), data, file, ftp, malito 등   
  - Host (Domain name)   `www.example.com` 
    - 요청을 받는 웹 서버의 이름   
    - IP 주소를 직접 사용할 수도 있지만, 실 사용시 불편하므로 웹에선 그리 자주 사용되지 않음     
  - Port   `: 80`
    - 웹 서버상의 리소스에 접근하는데 사용되는 기술적인 문이며, 일반적으론 생략되어 있음      
      - Host가 주소였다면, Port는 실제로 들어가는 문    
    - HTTP 프로토콜의 표준 포트   
      - HTTP 80   
      - HTTPS 443    
  - Path   `/ path / to / myfile.html`  (예전에 사용하던 물리적 위치)    
    - 웹 서버 상의 리소스 경로    
    - 초기에는 실제 파일이 위치한 물리적 위치를 사용했지만, 오늘날은 물리적인 실제위치가 아닌 추상화 형태의 구조로 표현   
    - ex> 물리적 위치 : `article/detail.html` , 추상화 형태의 구조 : `article/1`   
  - Query (Identifier)   `?key = value`
    - Query String Parameters   
    - 웹 서버에 제공되는 추가적인 매개 변수   
    - & 로 구분되는 key-value 목록   
  - Fragment   `#quick-start `    
    - Anchor   
    - 자원 안에서의 북마크의 한 종류를 나타냄   
    - 브라우저에게 해당 문서의 특정 부분을 보여주기 위한 방법    
    - 브라우저에게 알려주는 요소이기 때문에 Fragment identifier(부분 식별자)라고 부르며, `#`  뒷 부분은 서버에 보내지는 데이터가 아님          

</br>    

## RESTful API     

###### REST 원리를 따라 설계한 API 이며, RESTful services 혹은 simply REST services 라고도 부름   

- API   
  - Application Programming Interface   
  - 프로그래밍 언어가 제공하는 기능을 수행할 수 있게 만든 인터페이스    
    - CLI 는 명령줄, GUI는 그래픽, API는 프로그래밍을 통해 특정한 기능 수행    
  - Web API   
    - 웹 어플리케이션 개발에서 다른 서비스에 요청을 보내고 응답받기 위해 정의된 명세    
    - 현재 웹 개발은 모든 것을 직접 개발하는 것보단 오픈 API를 활용하는 추세   
      - ex. `TMDB API`    
  - 응답 데이터 타입   
    - HTML, XML, JSON(대중적으로 많이 사용) 등   
  - 대표적인 API 서비스 목록  
    - 유튜브 API, 네이버 파파고 API, 카카오 맵 API 등..     



- REST   
  - REpresentational State Transfer    
  - API 서버를 개발하기 위한 일종의 소프트웨어 설계 방법론    
  - 네트워크 구조 원리의 모음   
    - 자원을 정의하고 자원에 대한 주소를 지정하는 전반적인 방법   
  - REST 원리를 따르는 시스템을 RESTful 이란 용어로 지칭    
  - 자원을 정의하는 방법에 대한 고민에서부터 시작      
    - 정의된 자원을 어디에 위치 시킬 것인가 등등    
  - REST의 자원과 주소의 지정 방법   
    - 자원  
      - URL    
    - 행위   
      - HTTP Method     
    - 표현  
      - 자원과 행위를 통해 궁극적으로 얻고자하는 응답을 JSON으로 표현된 데이터 제공  
  - REST의 핵심 규칙  
    - 정보는 URI로 표현   
    - 자원에 대한 행위는 HTTP methods로 표현 (GET / POST/ PUT / DELETE)    
    - 데이터에 대한 표현은 JSON     
  - 설계 방법론은 지키지 않았을 때 잃는 것보다 지켰을 때 얻는 것이 훨씬 많음   
    - 지키지 않더라도 동작 여부에 큰 영향을 미치지는 않음    



- JSON (JavaScript Object Notation)       
  - 자바스크립트의 표기법을 따른 단순 문자열    
  - 특징  
    - key-value 구조라서 사람이 읽거나 쓰기 쉬움    
      - 파이썬의 dictionary, 자바스크립트의 object 처럼 C 계열의 언어가 갖고 있는 자료구조로 쉽게 변화 가능       
    - 기계가 파싱(해석,분석)하고 만들어내기 쉬움   

