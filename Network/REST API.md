# REST API란?
**API** -> 소프트웨어가 다른 소프트웨어로부터 지정된 형식으로 요청,명령을 받을 수 있는 수단 / ***A***pplication ***P***rogramming ***I***nterface      
**REST API** -> HTTP요청을 보낼때 어떤 URI에 어떤 메소드를 사용할지 (+기타) 개발자들 사이에서 널리 지켜지는 약속 

REST 형식의 API의 가장 큰 **특징** : 각 요청의 모습만으로 어떤 동작이나 정보를 위한 것인지 추론가능하다.
-> 과거의 SOAP이란 복잡한 형식을 대체하는 것   

- 서버에 REST API로 요청을 보낼 땐 HTTP란 규약에 따라 전송한다.   
   - 그 중 GET / POST / DELETE / PUT / (PATCH) 이 4가지 혹은 5가지 메소드를 사용한다.   
1. GET : read       -데이터 조회
2. DELETE : delete  -삭제
3. POST : create    -데이터 추가할 때   / body에 정보를 실어 보낸다. 
4. PUT : update     -내용을 통째로 갈아끼울 때 / body에 변경될 정보를 실어 보낸다.
5. PATCH : update   -일부 정보를 특정 형식으로 변경할 때 / body에 변경될 정보를 실어 보낸다.   

***-> GET , DELETE < POST , PUT , PATCH (+body)   
POST, PUT, PATCH는 body에 정보를 실어 보낼 수 있기 때문에 GET, DELETE보다 더 많은 정보를 보낼 수 있다.***
   
- POST 하나만으로도 할 수 있겠지만, 만약 그렇다면 요청을 구분하기 위해선 URI 마지막에 create인지 delete인지 등을 모두 써야 할 것이다.
그러면 깔끔하지 않다. 이것을 지양하기 위한 REST의 규칙 중 하나로 URI는 동사가 아닌 명사들로 이뤄져야 한다는 것이 있다.    
즉, https:// (도메인) /classes/2/students/~~create~~    CRUD가 들어가면 안되는 것이다.
   
   
   
#### CRUD : Create Read Update Delelte = 생성 조회 수정 삭제   
   
참고 : [얄팍한 코딩사전](https://youtu.be/iOueE9AXDQQ)   

더 상세 정보 : 구글 'restful api design guidelines' 검색   
   
      
         
         
