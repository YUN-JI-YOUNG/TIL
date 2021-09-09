## Web Framework

- static web page(정적 웹 페이지)

  - 서버에 미리 저장된 파일이 사용자에게 그대로 전달       
  - 서버가 추가적인 처리 없이 클라이언트에게 응답을 보내므로 모든 상황에서 모든 사용자에게  동일 정보 표시    
  - 일반적으로 HTML, CSS, JS로 작성, flat page라고도 함     

- Dynamic web page(동적 웹 페이지)

  - 서버가 추가적인 처리 과정 이후 클라이언트에게 응답을 보내므로 방문자와 상호작용해서 페이지 내용이 그때그때 다름     
  - 서버 사이드 프로그래밍 언어(python, java, C++ 등)가 사용되며, 파일을 처리하고 DB와의     상호작용이 이루어짐     

- Framework    

  - 프로그래밍에서 특정 운영체제를 위한 응용 프로그램 표준 구조를 구현하는 클래스, 라이브러리들의 모임    
  - 재사용 가능한 수많은 코드를 프레임워크로 통합하여 표준 코드를 다시 작성하지 않아도 같이 사용할 수 있도록 도움    
  - Application Framework라고도 함     

- Web Framework

  - 웹페이지 개발 과정에서 겪는 어려움을 줄이는 것이 주 목적
    - DB 연동, 템플릿 형태의 표준, 세션 관리, 코드 재사용 등의 기능 포함    
  - 동적 웹페이지, 웹 애플리케이션, 웹 서비스 개발 보조용으로 만들어지는 Application Framework의 일종     

- Framework Architecture       

  - 사용자 인터페이스로부터 프로그램 로직을 분리하여 애플리케이션 시각적 요소, 이면에서 실행되는 부분 등을 서로 영향 없이 쉽게 고칠 수 있는 애플리케이션을 만들 수 있다    

    - 반대로, Flask는 `하나의 파일`에 `URL, 함수, 템플릿, 모델 동작에 관한 내용 등`이 같이 존재하여 구동되므로 어떤 부분에서 문제가 생길 경우 서로 영향 받을 수 있음   

      -> 그러므로 간단한 프로그램에 주로 사용됨      

  - MVC Design Pattern (model - view - controller)    

    - 소프트웨어 공학에서 사용되는 디자인 패턴 중 하나        

  - MTV Pattern (model - template - view)       

    - Model   
      - 응용 프로그램의 데이터 구조 정의, DB의 기록을 관리(추가, 수정, 삭제)
    - Template = MVC 의 view         
      - 파일의 구조, 레이아웃을 정의하며 실제 내용을 보여주는 데 사용(presentation)    
    - View = MVC의 controller     
      - HTTP 요청 수신 -> HTTP 응답 반환    
      - model을 통해 요청을 충족시키는데 필요한 데이터에 접근     
      - template에게 응답의 서식 설정 맡김     

## Django

###### 하이 레벨의 파이썬 웹 프레임워크 (파이썬을 사용하는 동적 웹페이지)     

- 검증된 Python 언어 기반의 Web Framework
- 대규모 서비스에도 안정적이며 오랫동안 세계적인 기업들에 의해 사용됨         
  - Spotify, instagram, Dropbox, Delivery Hero 등      
- MTV Pattern    
- 코드 작성 순서는 데이터의 흐름에 맞춰 작성
  - `urls.py` -> `views.py` ->`models.py(필요한 경우에만)` ->  `templates`    

### Django Intro

- 프로젝트 생성  

  - 전체 포괄하는 프로젝트 폴더 생성   

  - 만약 가상환경 설정해서 pip 설치해야한다면, requirements.txt 가져와서

    `pip install -r requirements.txt`  

  - `$ django-admin startproject firstpjt .`       

    - `__init__.py` : python 에게 이 디렉토리를 하나의 python 패키지로 인식하게 함   
    - `asgi.py` : django 애플리케이션이 비동기식 웹 서버와 연결 및 소통하는 것을 도움      
      - Asynchronous(비동기) Server Gateway Interface
    - `wsgi.py` : django 애플리케이션이 웹서버와 연결 및 소통하는 것을 도움        
      - Web Server Gateway Interface
    - `settings.py` : 모든 설정(키, 디버그, 호스트, 템플릿, 앱 관련 정보 등) 포함  
    - `urls.py` : 사이트의 url과 적절한 views의 연결 지정       
    - `manage.py` : Django 프로젝트와 다양한 방법으로 상호작용 하는 커맨드라인 유틸리티    
      - `$python manage.py <command> [options]`    
        - `runserver` : 서버 실행
        - 기타 `startapp [appname]` , `creater`   등의 명령어 사용가능     

- django 서버 실행

  - `$ python manage.py runserver` 
  - 생성된 url 클릭    

- Application 생성   

  - `$ python manage.py startapp articles`  -> articles 는 [appname]    
    - `admin.py` : 관리자용 페이지 설정하는 곳     
    - `apps.py`  : 앱의 정보가 작성된 곳
    - `models.py` : 앱에서 사용하는 Model 정의하는 곳 (DB조작, 필드 정의 등)
    - `test.py` : 프로젝트의 테스트 코드 작성하는 곳    
    - `views.py` : view 함수들이 정의 되는 곳     
  - 일반적으로 Application 명은 복수형 권장    

- Project & Application    

  - project는 Application의 집합이며 여러 앱이 포함될 수 있음    
  - Application(앱)은 실제 요청을 처리하고 페이지를 보여주는 등의 역할을 담당하고, 일반적으로 하나의 역할 및 기능 단위로 작성        

- 앱 등록

  - 프로젝트에서 앱 사용하기 위해선 반드시 `settings.py`파일 내의  `INSTALLED_APPS` 리스트에 앱 이름 추가    
  - **INSTALLED_APPS**
    - Django installation에 활성화된 모든 앱을 지정하는 문자열 목록    
  - **반드시 생성 후 등록! **    
  - INSTALLED_APPS 에 먼저 작성(등록)하고 생성하려고 하면 앱이 생성되지 않음     
  - #Local apps > #Third party apps > #Django apps 순서로 등록
    - 해당 순서를 지키지 않아도 초반엔 큰 문제는 없지만, 추후 advanced한 내용을 대비하기 위해 지키는 것 권장      
    - 추후 html 등 파일 찾을 때 INSTALLED_APPS에 작성된 순서대로 찾아 들어감



### 요청과 응답

- URLS

  - HTTP 요청(request)를 알맞은 view에게 전달(중간 다리 역할)
  - `firstpjt` 폴더 내의 `urls.py` 의 urlpatterns 내에 추가    
    - 추가하기 전, 상단에 `from [appname] import views` 추가 필요
    - `path('index/', views.index)` : 서버주소 뒤에 **/index** 로 요청을 보내면, views의 index라는 함수를 실행       
    - `path('', views.index)` : 서버주소만 입력했을 때, views의 index 함수 실행   

- view

  - http 요청을 수신하여 응답을 반환하는 함수 작성

    - ex> index 함수 

      ```python
      def index(request):
        return render(request, 'index.html')
      
      #articels 폴더 내의 templates 폴더에 있는 `index.html` 파일을 렌더링
      #-> '서버주소/index' 를 입력하면 index.html 파일에 작성한 대로 웹페이지 표시
      # templates 폴더는 직접 생성해야함 -> 폴더명은 고정시킬 것
      ```

  - model을 통해 요청에 맞는 필요 데이터에 접근

  - template에게 http 응답 서식 맡김

- templates

  - 실제 내용을 보여주는데 사용되는  파일

  - 파일의 구조나 레이아웃 정의 (HTML 등)

  - Template 파일 경로의 기본 값은 app 폴더 안의 templates 폴더로 지정되어 있음    

    - app 폴더는 내가 만든 application 폴더 (수업에선 articles)   

  - 추가설정(`settings.py` 파일 내)

    - LANGUAGE_CODE

      - 모든 사용자에게 제공되는 번역 결정    
      - 이 설정이 적용되려면 USE_I18N 이 활성화되어 있어야 함       
        - `USE_I18N` : Django의 번역 시스템 활성화 여부 지정      
      - 각 언어별 코드는 [language-identifiers](http://www.i18nguy.com/unicode/language-identifiers.html) 참고    
      - 한국 - `LANGUAGE_CODE = 'ko-kr'`

    - TIME_ZONE     

      - DB 연결의 시간대를 나타내는 문자열 지정       

        - `USE_L10N`  : 데이터의 지역화된 형식을 기본적으로 활성화할지 여부 지정 

          True -> Django는 현재 locale의 형식을 사용하여 숫자와 날짜 표시    

      - USE_TZ가 True이고 이 옵션이 설정되면, DB에서 날짜 시간을 읽으면 UTC 대신 새로 설정한 시간대의 인식 날짜, 시간 반환    

      - USE_TZ가 False이고 이 옵션이 설정되면, error 발생    

        - `USE_TZ` : datetimes가 기본적으로 시간대를 인식하는지 여부 지정

          True -> Django는 내부적으로 시간대 인식하여 날짜 / 시간 사용    

      - 각 나라별 코드는 [List of tz database time zones](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) 참고   

      - 한국 시간대 - `TIME_ZONE = 'Asia/Seoul'`

</br>   

### Template

- Django Template

  - 데이터 표현을 제어하는 도구이자 표현에 관련된 로직

  - 사용하는 빌트인 시스템 : `Django template language(DTL)` 

    - 조건, 반복, 변수 치환, 필터 등의 기능 제공   

      -> 일부 프로그래밍 구조를 사용할 수 있지만 해당 python 코드로 실행되는 것은 X

    - 단순히 파이썬이 HTML에 포함된 것이 아니며, 프로그래밍적 로직이 아니라 프로젠테이션을 표현하기 위한 것      

- DTL Syntax

  - Variable  `{{ variable }}`  
    - render()를 사용하여 veiws.py에서 정의한 변수를 template 파일로 넘겨 사용    
    - 변수명은 밑줄로는 시작할 수 없으며, 공백이나 구두점 문자 또한 사용 불가    
    - dot(.)을 사용하여 변수 속성 접근 가능    
    - render()의 세번째 인자로 {'key' : value} 와 같이 딕셔너리 형태로 넘겨주며, 여기서 정의한 key에 해당하는 문자열이 template에서 사용 가능한 변수명이 됨     
      - 보통 `context = { 'foods' : foods, }` 등으로 context 변수안에 다 넣은 후, `render(request, ' .html', context)` 처럼 context를 세번째 인자로 넘겨준다.  
  - Filters `{{ variable|filter }}`
    - 표시할 변수를 수정할 때 사용    
      - ex> `{{ name|lower }}` : name 변수를 모두 소문자로 출력     
    - 60개의 빌트인 template filters 제공   
    - chained가 가능하며 일부 필터는 인자를 받기도 함      
      - ex> `{{ variable|truncatewords:30 }}`  : 변수의 맨 첫 단어부터 30개 출력    
  - Tags `{% tag %}`    
    - 출력 텍스트를 만들거나 반복 or 논리를 수행하여 제어 흐름 만드는 등 변수보단 복잡한 일 수행    
    - 일부 태그는 시작, 종료 태그 필요    
      - ex> `{% if % } {% endif %}` 
    - 약 24개의 빌트인 templated tags 제공     
  - Comments `{# #}`
    - 라인의 주석 표현 위해 사용    
    - 유효하지 않은 템플릿 코드 포함가능   
    - 한 줄 주석에만 사용 가능 (줄바꿈 X)
      - 여러줄 주석은 `{% comment %} 와 {% encomment %} 사이` 에 입력    

- 코드 작성 순서는 데이터의 흐름에 맞춰 작성

  - `urls.py` -> `views.py` ->`models.py(필요한 경우에만)` ->  `templates`    

- 템플릿 상속(Template inheritance)    

  - 기본적으로 코드의 재사용성에 초점을 맞춤        
  - 사이트의 모든 공통 요소 포함, 하위 템플릿이 재정의(override) 할 수 있는 블록을 정의하는 기본 "skeleton" 템플릿 생성 가능     
  - Tags
    - `{% extends '' %}` : 상속받을 자식 템플릿 최상단에 작성    
    - `{% block content%} {% endblock%}` : 자식 템플릿에서 재지정할 수 있는 블록 정의   
      - 부모 템플릿:  body 태그 끝나기 전에 사이 비워두고 작성   
      - 자식 템플릿 : extends 태그 바로 밑에 작성 후 그 사이에 내용 작성    

- Django template system (**django 설계 철학**)

  - **표현과 로직(view)를 분리 **    
    - 템플리 시스템은 표현을 제어하는 도구이자 표현에 관련된 로직일 뿐, 기본 목표를 넘어서는 기능은 지원하지 않아야 함    
  - **중복 배제**    
    - 대다수의 동적 웹 사이트는 공통 header, footer, navbar 같은 공통 디자인을 갖으므로, 이러한 요소를 한 곳에 저장하기 쉽게 하여 중복 코드 배제 -> 템플릿 상속의 기초         



### HTML Form

- "form"

  - 웹에서 사용자 정보를 입력하는 여러 방식 제공 
    - text, button, checkbox, image, submit 등      
  - 사용자로부터 할당된 데이터를 서버로 전송하는 역할   
  - 핵심 속성    
    - action : 입력 데이터가 전송될 URL 지정
    - method : 입력 데이터 전달 방식 지정    

- "input"

  - 사용자로부터 데이터를 입력받기 위해 사용   

  - type 속성에 따라 동작 방식 달라짐    

  - 핵심 속성

    - name

    - 중복 가능, 양식 제출시 name에 설정된 값을 넘겨서 값을 가져올 수 있음    

    - 주요 용도는 GET/ POST 방식으로 서버에 전달하는 파라미터로 

      `?key=value&key=value` 형태로 전달     

- "label"

  - 사용자 인터페이스 항목에 대한 설명(caption) 나타냄    

  - label 을 input 요소와 연결   

    1. input 에 id 속성 부여

    2. label에 input의 id와 동일한 값의 for 속성 부여      

  - 시각적 기능 뿐아니라 화면 리더기에서 label을 읽어 사용자가 입력해야 하는 텍스트가 무엇인지 더 쉽게 이해할 수 있도록 돕는 프로그래밍적 이점 존재    

  - label을 클릭해서 input에 focus를 맞추거나 activate 가능    

- "for"      

  - for 속성의 값과 일치하는 id를 가진 문서의 첫번째 요소 제어   

    - 연결된 요소가 labelable elements인 경우 이 요소에 대한 labeled control이 됨    

      - labelabel elements : label 요소와 연결할 수 있는 요소    

        ex> button, input 등등

- "id"     

  - 전체 문서에서 고유해야 하는 식별자 정의    
  - linking, scripting, styling 할 때, 요소 식별 목적      

- HTTP

  - 웹에서 이루어지는 모든 데이터 교환의 기초      
  - 주어진 리소스가 수행할 원하는 작업을 나타내는 request method 정의
  - HTTP request method 종류   
    - GET, POST, PUT, DELETE 등등    
    - GET : 서버로부터 정보 조회하는 데 사용   
      - 데이터를 가져올 때만 사용해야 함 
      - 데이터를 서버로 전송할 때 body (X) , Query String Parameters (O) 를 통해 전송     
      - 보안이 필요없는 단순한 정보 조회 -> 회원 정보 등 보안이 필요하면 POST 방식 사용   
      - 서버에 요청하면 HTML 문서 파일 한장 받음 -> 이때 요청 방식이 GET     

### URL

- Django URLs

  - Dispatcher(발송자)로서의 URL   
  - 웹 애플리케이션은 URL을 통한 클라이언트의 요청에서부터 시작   

- Variable Routing

  - url 주소를 변수로 사용하는 것     

  - url 일부를 변수로 지정하여 view 함수의 인자로 넘길 수도 있음    

  - 즉, 변수 값에 따라 하나의 path() 에 여러 페이지 연결 가능    

    - ex> `path('/accounts/uesr/<int:uesr_pk>/', ...)` 라면,

      `accounts/uesr/1` : 1번 user,  `accounts/user/2` : 2번 user 등등

  - URL path converters     

    - 작성하지 않으면 기본 값(str)  

      즉, `path('hello/<name>/', ..)` **==** `path('hello/<str:name>', ..)`

    - `str` : '/'를 제외하고 비어있지 않은 모든 문자열과 매치  

    - `int` : 0 or 양의 정수와 매치    

    - `slug` : ASCII 문자 or 숫자, 하이픈 및 밑줄 문자로 구성된 모든 슬러그 문자열과 매치   

- App URL mapping

  - app의 view 함수가 많아지면 사용하는 path() 증가, app 작성 증가       

    이 때, 프로젝트의 `urls.py`에서 모두 관리하는 것은 프로젝트 유지보수에 좋지 않음    

    -> 하나의 `urls.py`에 작성하려면 `pages_urls.py` , `articles_urls.py`  등으로 복잡해짐     

    -> 그러므로 각 `app에 urls.py` 생성 -> 모듈 import해서 `urlpatterns = [path()]` 작성   

    -> `프로젝트 urls.py` 에서 각 `app의 urls.py` 파일로 URL 매핑 위탁      

    ```python
    from django.urls import path, include
    
    path('articles/', include('articles.urls'))
    
    # 두번째 인자로 views.index 대신, include('[app_name].urls') 작성         
    ```

    - include()    
      - 다른 URLconf들(app/urls.py) 을 참조할 수 있도록 도움      
      - 함수 include()를 만나면, URL의 그 시점까지 일치하는 부분을 잘라내고, 남은 문자열 부분 처리를 위해 include된 URLconf로 전달   
    - **Django는 명시적 상대경로 권장 (from .module import ..)** 

- Name URL patterns

  - 링크에 url을 직접 작성하는 것이 아니라, path() 함수의 name 인자를 정의해서 사용    

  - Django Template Tag 중 하나인 url 태그를 사용해서 path() 함수에 작성한 name 사용 가능    

    ```python
    path('index/', views.index, name='index'),  
    <a href="{% url 'index' %}"> 메인 페이지 </a>  
    ```

    - url 설정에 정의된 특정 경로들의 의존성 제거 가능   

  - url template tag `{% url ''%}`  

    - 주어진 URL 패턴 이름 및 선택적 매개변수와 일치하는 절대 경로 주소 반환    

    - 템플릿에 URL 하드코딩하지 않고 DRY 원칙 준수하는 링크 출력 방법      

      - 반복하지 말 것(DRY)[¶](https://docs.djangoproject.com/ko/3.2/misc/design-philosophies/#don-t-repeat-yourself-dry)

        ```
        고유한 개념 및 데이터는 단 한 번, 단 한 곳에 존재하는 것으로 족합니다. 중복성은 나쁜 것이고, 정규화는 좋은 것입니다. 그러한 이유로, 본 프레임워크는 최소한의 것들을 가지고 최대한의 것을 만들어내도록 합니다.
        ```

</br>    

### 이름공간(namespace)   

###### 객체를 구분할 수 있는 범위를 나타내는 말, 일반적으로 하나의 이름 공간에서는 하나의 이름이 단 하나의 객체를 가리킴 

- 서로 다른 app의 같은 이름을 가진 url name은 이름공간 설정으로 구분   
- templates, static 등 django는 정해진 경로 하나로 모아서 보기 때문에, 중간에 폴더를 임의로 만들어 이름공간 설정     

- URL namespace
  - 서로 다른 앱에서 동일 URL 이름을 사용하는 경우에도 이름이 지정된 URL 사용 가능   
  - `urls.py`의 `urlpatterns` 위에  "app_name" 속성 값 작성   
    - `app_name = articles` 
  - 실제 URL 작성할 땐 url 태그를 사용하면서 주소 이름 앞에 `app_name: ` 추가    
    - `{% url 'articles:index' %}` 
- Template namespace    
  - 기본적으로 `app_name/templates/ `경로에 있는 templates 파일들만 탐색 가능   
    - INSTALLED_APPS 에 작성한 app 순서대로 탐색 후 랜더링   
  - templates 폴더 구조를 `app_name/templates/app_name` 로 변경하면 임의로 이름 공간 생성 가능   

