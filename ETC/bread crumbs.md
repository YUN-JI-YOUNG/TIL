# :thought_balloon: 빵 부스러기

>개발 관련 간단한 학습 후 
해당 정보들을 모아두는 곳.
-----
## 목차
1. MVC 프레임 워크란?
2. REST API란?
   
   
   
-----
</br>

## 1. MVC 프레임 워크란?
우선, MVC = **Model View Controller** 의 줄임말이며, MVC 프레임워크는 *동적 웹* 만들 때 사용
1.  Model : 데이터베이스에 저장된 데이터들의 형식을 지정, 저장하고 불러오고 작업들에 대한 코드 작업 (=주방장)   
2.  View : html, CSS 등을 사용하여 만들며 사용자에게 보여지는 부분 관련 (=플레이팅 직원)
3.  Controller : Model의 데이터를 View에 연결하여 사용자가 GUI 화면을 통해 데이터를 볼 수 있도록 Model 파트와 View 파트를 전반적으로 관리 (=관리 매니저)

- 프레임 워크(=구조 틀) <-> 라이브러리(=부속품)
  - 프레임 워크는 만들어져있는 틀에서 원하는 대로 수정하는 것 <-> 라이브러리는 해당 기능을 갖다 쓰는 것

- MVC 프레임워크 종류 : Spring(Java) / Laravel (php) / Ruby on Rails(Ruby) / Play(함수형 언어 Scala)   

조금 다르지만,
- MTV 프레임워크 : ModelTemplateView 프레임워크 - Djanngo(python)

##### dotnet core(C#) MVC 프레임워크로 실습 -> [얄팍한 코딩사전:MVC 웹 프레임워크가 뭔가요?](https://youtu.be/AERY1ZGoYc8)   
   
   -----
</br>
   
## 2. REST API란?
API -> 소프트웨어가 다른 소프트웨어로부터 지정된 형식으로 요청,명령을 받을 수 있는 수단 / ***A***pplication ***P***rogramming ***I***nterface      
REST API -> HTTP요청을 보낼때 어떤 URI에 어떤 메소드를 사용할지 (+기타) 개발자들 사이에서 널리 지켜지는 약속 

REST 형식의 API의 가장 큰 특징 : 각 요청의 모습만으로 어떤 동작이나 정보를 위한 것인지 추론가능하다.
-> 과거의 SOAP이란 복잡한 형식을 대체하는 것   

- 서버에 REST API로 요청을 보낼 땐 HTTP란 규약에 따라 전송한다. 그 중 GET / POST / DELETE / PUT / (PATCH) 이 4가지 혹은 5가지 메소드를 사용한다.   
1. GET : read    -데이터 조회
2. DELETE : delete -삭제
3. POST : create  -데이터 추가할 때 / body에 정보를 실어 보낸다. 
4. PUT : update  -내용을 통째로 갈아끼울 때 / body에 변경될 정보를 실어 보낸다.
5. PATCH : update -일부 정보를 특정 형식으로 변경할 때 / body에 변경될 정보를 실어 보낸다.   
   
- POST 하나만으로도 할 수 있겠지만, 만약 그렇다면 요청을 구분하기 위해선 URI 마지막에 create인지 delete인지 등을 모두 써야 할 것이다.
그러면 깔끔하지 않다. 이것을 지양하기 위한 REST의 규칙 중 하나로 URI는 동사가 아닌 명사들로 이뤄져야 한다는 것이 있다.    
즉, https:// (도메인) /classes/2/students/~~create~~    CRUD가 들어가면 안되는 것이다.
   
   
   
##### CRUD : Create Read Update Delelte = 생성 조회 수정 삭제   
   
참고 : [얄팍한 코딩사전] (https://youtu.be/iOueE9AXDQQ)
더 상세 정보 : 구글 'restful api design guidelines' 검색   
   
      
         
         
