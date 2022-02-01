# :thought_balloon: 빵 부스러기

>개발 관련 간단한 학습 후 
해당 정보들을 모아두는 곳.
-----
## 목차
1. MVC 프레임 워크란?
2. REST API란? 
3. Java 설치 에러 해결        
4. Docker 이용해보기    
5. SpringBoot > JPA    
6. 데이터 크롤링     
   
   
   
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
   
      
<hr>    
</br></br>     
         
## 3. Java 설치 에러 해결           
- Java 설치하여 환경변수 설정까지 모두 끝냈는데 아래와 같은 에러 발생    
```java    
java : 'java' 용어가 cmdlet, 함수, 스크립트 파일 또는 실행할 수 있는 프로그램 이름으로 인식되지 않습니다. 이름이 정확한
지 확인하고 경로가 포함된 경우 경로가 올바른지 검증한 다음 다시 시도하십시오.
위치 줄:1 문자:1
+ java -version
+ ~~~~
    + CategoryInfo          : ObjectNotFound: (java:String) [], CommandNotFoundException
    + FullyQualifiedErrorId : CommandNotFoundException
```     
- 알고보니, Java.exe가 설치된 폴더 경로를 사용자 변수가 아닌, 환경변수에 추가했어야 했고, Windows 10에선 추가한 다음엔 해당 환경변수를 맨 위로 올렸어야 했다.       
- 그리고 터미널을 재시작하여 버전을 확인해보니 아래와 같이 정상적으로 나오는 것을 확인할 수 있었다.    
```java    
openjdk version "17.0.1" 2021-10-19
OpenJDK Runtime Environment (build 17.0.1+12-39)
OpenJDK 64-Bit Server VM (build 17.0.1+12-39, mixed mode, sharing)
```    
         
<hr>    
</br></br>     


## 4. Docker 
- 설치 에러   
   - 알고보니, 리눅스 커널도 안깔려있고, 가상화 설정도 안되어있어서 생긴 오류 였었던 걸로 추정된다.     
   -> bios 진입하여 advanced 모드에서 cpu configurations 설정을 선택하고, svm 모드를 disabled 에서 enabled로 변경하여 종료 하면 가상화 설정은 완료   
   -> 그리고 Docker 를 실행해보면, 리눅스 설치 에러메시지가 출력되므로 해당 링크를 타고 가서 다운로드 받아 실행시키고 Docker 를 재실행하면 정상 작동하는 것을 확인  
- 실행   
   - Window powershell 에서`docker run hello-world` 명령어로 테스트용 도커 컨테이너를 실행해보면, 처음엔 없다고 나오다가 자동으로 설치되는 것을 확인할 수 있다. 

- 명령어    
   - `docker run [컨테이너]` : 컨테이너 실행   
   - `docker run --name=[컨테이너 이름]` : 자동으로 만들어진 이름 대신 지정 이름으로 컨테이너 생성 가능      
   - `docker run --name myjenkins -d -p 9080:8080 jenkins/jenkins`  : Jenkins를 도커 컨테이너로 실행   
      - `-d`  : 백그라운드 데몬으로 실행    
      - `-p`  : 가상 머신(게스트 pc)에서 동작하는 서비스에 docker proxy 서비스가 NAT 기능을 수행해 포트 포워딩을 해주어 실제 머신(호스트 pc) IP로 서비스에 접속 가능하도록 함     
      - 보통, 컨테이너가 삭제되면 생성된 데이터가 같이 삭제되어, 호스트 oc의 디렉토리에 데이터가 생성되도록 볼륨 마운트 옵션을 추가하여 실행    

      ` -v /your/home:/var/jenkins_home`      
   - `docker ps` : 컨테이너 조회    
   - `docker ps -a` 로 정지된 컨테이너까지 조회 가능   
   - `docker rm [컨테이너 ID or NAME]` : 컨테이너 삭제  
      - 만약 컨테이너 ID가 abe7a2c9083b 라면, `docker rm abe7a2c` 까지로 일부만 입력해도 정상 삭제 가능    
         -> 컨테이너 삭제한다고 도커 이미지까지 삭제되는 것은 X   
         -> 이미지까지 삭제하기 위해선 `docker rmi [이미지 ID or 이미지명:TAG명]` 명령어 필요    

    - TAG 중 최신이라는 뜻인 latest 태그명은 생략 가능      

  - `docker rm -f [컨테이너 ID or NAME]` : 실행중이던 컨테이너도 강제 삭제    

<hr>    
</br></br>    

## 5. SpringBoot > JPA     

## JPA 

- 표준 ORM   

  - ORM = 객체 관계 매핑  
  - 객체 저장 / 관계 맺기 등의 작업 가능   

- 내부적으론 SpringBoot가 hibvernate 프레임워크를 채택하여 사용  

  - Entity (객체) + Repository (메소드)  = Query       

  - Django 에서 models.py 작성하고 migration 하는 것과 비슷    



### 필수요소

#### @Entity   

- DB의 테이블명         

- DB  설계한 것을 토대로 Entity 구현     

- Entity 구현 전 사전 작업 필요       

  - DB 테이블 등이 설계되는 DDL 작업  

    ```java
    spring.jpa.generate-ddl = true
    spring.jpa.hibernate.ddl-auto = update
    ```

    

  - Query가 수행되는 것을 log로 보는 옵션 설정    

    - resources > application.properties 파일     

    ```java
    // 수행되는 쿼리를 로그로 볼 수 있음  
    spring.jpa.show-sql = true
    spring.jpa.properties.hibernate.format_sql = true
        
    // 바인딩 되는 값을 로그로 볼 수 있음 
    logging.level.org.hibernate.type.descriptor.sql.BasicBinder = trace
    ```

    - 디버깅 편리   

- Entity 구현 Tip   

  - `@Column(columnDefinition = "INT UNSIGNED")` 처럼 타입을 unsigned 로 설정하면 

    용량은 줄어들지만 데이터는 2배로 넣을 수 있음   

- Relationship 관계 정의할 때 고려할 사항  

  - 다중성   
    - 1:N  `@OneToMany` 
    - N:1 `@ManyToOne`  
    - 1:1   `@OneToOne`
    - N:M  `@ManyToMany`  
  - 방향성  
    - 단방향 
    - 양방향   
      - 양방향은 JPA에서는 지양   
  - 양방향일 경우, 연관관계의 주인     
    - `@OneToMany (mappedBy= "boardFk")`  
      - `@JoinColumn(name="boardFk")`  
    - 어디서 컨트롤할 수 있는가     
    - FK 키 관리 주인 설정   
    - `Many` 쪽이 주인이며, `@ManyToOne` 은 항상 주인이므로 `mappedBy` 속성 X      



##### DB 설계  

- Model 작성이 어려울땐 육하원칙을 따라서!     

| 종류   | PK   | FK         | 누가   | 언제     | 어디서 | 무엇을    | 어떻게    | 왜   |
| ------ | ---- | ---------- | ------ | -------- | ------ | --------- | --------- | ---- |
| 게시판 | UID  |            | 작성자 | 작성일자 | IP주소 | 제목/내용 | 웹/모바일 | NA   |
| 댓글   | UID  | 게시판_UID | 작성자 | 작성일자 | IP주소 | 제목/내용 | 웹/모바일 | NA   |

- 1 : N 관계이므로 `게시판 PK == 댓글 FK`      



#### JpaRepository   

- 원하는 것을 명령하는, 즉, Query 가 되는 Respository      

  | CRUD   | 메소드명                                    |
  | ------ | ------------------------------------------- |
  | Read   | find*** 로 시작!  (  `ex`.  findById() ** ) |
  | Delete | delete*** 로 시작!                          |
  | Create | save                                        |
  | Update | 객체 조회 > 값 변경 > save                  |

  - Create 로 객체 생성할 때 Tip  
    - `@Builder` 사용       
      - lombok *annotation*    
      - 순서를 신경쓰지 않고 DB 적용 가능   


<hr>    
</br></br>    

## 6. 데이터 크롤링     

#### 1. 인터넷 상에서 자동으로 데이터를 수집하는 방법     

1. OpenAPI 등 공개된 API를 사용한다.

+ DATA.go.kr
+ NAVER Developers
+ Mydata API (사업자등록증, 제한된 사용자만 가능하다.)

2. HTTP Get Method

+ 정보가 게시되어 있는 대상 웹사이트를 HTTP Get 을 사용하여 html 코드를 얻고 Text Parsing 해서 사용
+ Java/ Go/ Python /C/ C++ / Perl / Node.js 등등 거의 대부분의 언어로 구현 가능

3. Selenium Webv Driver

+ 웹 브라우저 인스턴스를 생성 해 실행시킨 후 해당 인스턴스를 컨트롤
+ 티처블 머신 학습 시킬 떄 활용가능
+ 데이터 크롤링 목적으로 많이 활용한다.

#### 2. 웹크롤러 vs 웹 스크래퍼

1. 웹 크롤러(Wev Crawler)

+ 조직적, 자동화된 방법으로 웹을 탐색/수집하는 프로그램
  ex) 구글, 네이버등의 검색엔진 결과 데이터를 수집하기 위한 봇(Bot)

2. 웹 스크래퍼(Webv Scrapper)

+ 웹 사이트에서 정보를 추출하는 프포그램
  ex) 상품별 가격을 알기 위해 해당 상품을 파는 페이지들의 가격을 추출

+ 크롤러 보다는 대부분 단순 스크래퍼 개발 수요가 많음
+ 우리나라에서는 많은 기업들이 같은 의미로 혼용

#### 3. 웹 크롤링의 불법 여부    

+ 웹사이트 홈 디렉토리에 위치한 robots.txt 파일을 열어보고 해당사이트의 정책을 준수하지 않는다면 불법
+ 크롤링한 자료를 상업적인 용도로 사용하면 불법
  + 사람인 vs 잡코리아
  + 여기어때 vs 야놀자
+ HiQ vs Linkedin 의 경우는 불법이 아니라고 판명남
+ 원작자의 수익을 해치지 않았다면 합법
  ex) 옥션, 다나와
+ 비상업적인 용도라 하더라도 원작자에게 불이익을 주면 불법
+ 크롤러를 활용해 고의적으로 Abusing 하는경우 불법
  ex) 네이버 검색순위 조작, 음원 순위 조작

