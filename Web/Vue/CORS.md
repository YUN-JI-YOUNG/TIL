## Server & Client   

- Server    

  - 정보제공    

  - DB와 통신하며 데이터를 CRUD    

  - 클라이언트에게 정보, 서비스를 제공하는 컴퓨터 시스템   

    `ex`  Django를 통해 응답한 template, DRF를 통해 응답한 JSON     

- Client     

  - 정보 요청 & 표현    

  - 서버에게 해당 서버가 맡은 서비스를 요청    

    -> 서비스 요청에 필요한 인자를 서버가 요구하는 방식에 맞게 제공     

    -> 서버로부터 반환된 응답을 사용자에게 적절한 방식으로 표현     

    위의 기능을 가진 시스템    



## CORS   

- vue 에서 django로 데이터를 받아오기 위해 요청을 보내기 위해선, `'Access-Control-Allow-Origin' header` 필요    
  - 요청을 보내면, 서버측에선 200 응답코드가 발생함      
  - 기본적으로 SOP 정책을 따르기 때문에 서로 다른 출처를 가진 vue와 django가 통신하기 위해선 CORS header 필요      



- **Same-origin policy(SOP)**    
  - 동일 출처 정책   
  - 특정 출처 orgin 에서 불러온 문서나 스크립트가 다른 출처에서 가져온 리소스와 상호작용하는 것을 제한하는 보안방식    
  - 잠재적으로 해로울 가능성이 있는 문서를 부리하여 공격 경로를 줄이기 위함   
  - **Origin(출처)**  
    - 두 URL의 Protocol, Port, Host 가 모두 같아야 동일 출처     
      - Protocol -> `http`, `https` ..   
      - Port -> `:81`, `:8081`, `:8000` ...   
      - Host -> `~~.~~.com` ...    
      - Host 뒷부분 `/` 은 Path      



- **Cross-Origin Resource Sharing (CORS)**   

  - 교차 출처 자원 공유   

  - 추가 HTTP header 를 사용       

    -> 특정 출처에서 실행중인 웹 애플리케이션이 다른 출처의 자원에 접근할 수 있는 권한을  부여하도록 브라우저에 알려주는 체제        

  - 리소스가 자신의 출처와 다를 때, 교차 출처 HTTP 요청 실행      

  - 보안상의 이유로 브라우저는 SOP 정책에 의해 교차출처 HTTP 요청 제한(SOP)       

    - XMLHTTPRequest 는 SOP 정책을 따름      

  - 다른 출처의 리소스를 불러오려면 그 출처에서 올바른 CORS header 를 포함한 응답 반환 필요       

  - CORS 사용 이유   

    - 브라우저 & 웹 애플리케이션 보호    

      - 악의적인 사이트의 데이터를 가져오지 않도록 사전 차단   
      - 응답으로 받는 자원에 대한 최소한의 검증  
      - 서버는 정상 응답하지만 브라우저에서 차단    

    - 서버의 자원 관리   

      - 누가 해당 리소스에 접근가능한지 관리 가능      

        -> 해당 정보를 알면 필터링 가능    

  - CORS 사용 방법   

    - CORS 표준에 의해 추가된 HTTP header 를 통해 통제    

      `Access-Control-Allow-Origin`, `Access-Control-Allow-Credentials`, `Access-Control-Allow-Headers` , `Access-Control-Allow-Methods` ..     

    - **Access-Control-Allow-Origin** 응답 헤더     

      - 이 응답이 주어진 출처로부터 요청 코드과 공유 가능한지 표시    

        -> `Access-Control-Allow-Origin : *`    

        - 브라우저 리소스에 접근하는 어떤 출처로부터의 요청을 모두 허용한다고 알리는 응답에 포함    
        - `*`  : 모든 도메인에서 접근 가능    
        - `*` 외의 특정 출처 하나 명시 가능      

    - 각 프레임워크별로 CORS를 지원하는 라이브러리들이 존재    

      -> django는 `django-cors-headers` 라이브러리로 응답 헤더 및 추가 설정 가능     

      - **django-cors-header 라이브러리 **    

        - 응답에 CORS hedaer 추가    

        - 다른 출처에서 보내는 Django 앱에 대한 브라우저 내 요청 허용     

        - Django 앱이 header 정보에 CORS를 설정한 상태로 응답 줄 수 있게 도움   

          -> 브라우저는 다른 출처에 요청을 보내는 것이 가능해짐    



- **Cross-Origin Resource Sharing Policy (CORS 정책)**    
  - 교차 출처 자원 공유 정책    
  - 다른 출처에서 온 리소스를 공유하는 것에 대한 정책    
    - SOP와 반대    
  - 교차 출처 접근 허용 방법   
    - CORS를 사용해 교차 출처 접근 허용   
    - CORS는 HTTP의 일부로, 어떤 호스트에서 자신의 컨텐츠를 불러갈 수 있는지 서버에 저장할수 있는 방법      
    - django의 django-cors-headers 라이브러리를 사용할 경우    
      - `settings.py` 에 `corsheaders` 앱 등록    
      - `'corsheaders.middleware.CorsMiddleware',` 미들웨어 등록    
      - `CORS_ALLOWED_ORIGINS = `  설정 추가   
        - 특정 출처만 등록하려면  배열로 추가    
        - 모든 출처를 허용하려면 `True` 설정     

<hr>

