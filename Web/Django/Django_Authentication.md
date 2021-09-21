### Django Authentication 시스템

- 장고 인증 시스템은 `django.contrib.auth` 에 `Django contrib module`로 제공
- 필수 구성은 settings.py에 이미 포함되어 있으며, 앱 리스트에 이미 등록
  - `django.contrib.auth`  : 인증 프레임워크의 핵심, 기본 모델 포함
  - `django.contrib.contenttypes`  : 사용자가 생성한 모델과 권한 연결 가능
- Authentication(인증)
  - 신원 확인
- Authorization (권한, 허가)
  - 권한 부여 = 인증된 사용자가 수행할 수 있는 작업 결정



#### 쿠키와 세션

###### 쿠키 > 세션

- HTTP 

  - HTML 문서와 같은 리소스들을 가져올 수 있도록 해주는 프로토콜
  - 웹에서 이루어지는 모든 데이터 교환의 기초
  - 클라이언트 - 서버 프로토콜이기도 함
  - 특징
    - 비연결지향 
      - 서버는 요청에 대한 응답을 보낸 후 연결 끊음
    - 무상태
      - 연결을 끊는 순간 클라이언트와 서버 간의 통신이 끝나며 상태 정보 유지 X
      - 클라이언트와 서버가 주고받는 메세지들은 서로 독립적
    - 클라이언트와 서버의 지속적인 관계를 유지하기 위해 쿠키와 세션이 존재    
      - HTTP의 특징만으로는 한번 로그인을 하고 창을 이동하는 등의 작업을 하면 로그인이 풀려야하는데, 유지된다는 것은 따로 신호가 있다는 것

- 쿠키

  - 서버가 웹 브라우저에 전송하는 작은 데이터 조각

  - 사용자가 웹사이트를 방문할 때, 해당 사이트의 서버를 통해 사용자의 컴퓨터에 설치되는 작은 기록 정보 파일

    - **브라우저는 쿠키를 로컬에 KEY-VALUE 형식으로 저장하고, 동일 서버에 재요청 시 저장된 쿠키를 함께 전송**

    - 즉, 웹페이지에 접속하면 요청한 웹페이지를 받으며 쿠키 저장 +

      클라이언트가 같은 서버에 재요청 시 요청과 함께 쿠키도 함께 전송

  - HTTP 쿠키는 상태가 있는 세션을 만들어 줌   

  - 두 요청이 동일한 브라우저에서 들어왔는지 여부를 판단할 때 주로 사용

    - 무상태 HTTP 프로토콜에서 상태정보를 기억시켜주므로 이를 이용해 로그인 상태 유지 가능

  - SW가 아니므로 프로그램처럼 실행되지않아 악성코드를 설치할 수 없지만, 행동을 추적하거나 쿠키를 훔쳐서 해당 사용자의 계정 접근권한 획득 가능

  - 사용목적

    - 세션관리

      - 로그인, 아이디 자동완성, 공지 하루 안보기, 팝업체크, 장바구니 등의 정보 관리

        -> 비회원상태에서 장바구니를 담고, 쿠키를 지우면 장바구니 내의 아이템 삭제됨

    - 개인화

      - 사용자 선호 및 테마 등의 설정

    - 트래킹

      - 행동 기록 및 분석

- 세션

  - 사이트와 특정 브라우저 사이의 상태를 유지키시는 것

  - 클라이언트가 서버에 접속하면 

    서버가 특정 session id 발급하고, 

    클라이언트는 발급 받은 session id를 쿠키에 저장   

    -> 클라이언트가 다시 서버에 접속하면 요청과 함께 session id가 저장된 쿠키를 서버에 전달

    - 쿠키는 요청때마다 서버에 전송되므로 서버에서 session id를 확인해 알맞은 로직 처리

  - session id는 세션 구별을 위해 필요

- 쿠키 수명

  1. session cookies
     - 현재 세션이 종료되면 삭제됨 
       - 보통 웹페이지를 닫으면 삭제
     - 브라우저가 **현재 세션** 이 종료되는 시기 정의
       - 일부 브라우저는 재시작시 세션 복원을 사용해 세션쿠키가 오래 지속 가능하도록 함
  2. Persistent cookies (or Permanent cookies)
     - expires 속성에 지정된 날짜 혹은 Max-Age 속성에 지정된 기간이 지나면 삭제

- Django에서의 session

  - Django의 세션은 미들웨어를 통해 구현
  - database-backed sessions 저장방식을 기본값으로 사용
    - 세션을 DB에 저장하고, 클라이언트에게 세션 id만 전달
    - 설정을 통해 cached, file-based, cookie-based 방식으로 변경 가능
  - 특정 session id를 포함하는 쿠키를 사용하여 각 브라우저와 사이트가 연결된 세션 알아냄
    - 세션 정보는 DB의 django_session 테이블에 저장
  - 모든 것을 세션으로 사용하려고 한다면, 이용자 수가 많을 때 서버 과부화 위험

- 미들웨어에서의 인증 시스템

  - SessionMiddleware
    - 요청 전반에 걸쳐 세션 관리
  - AuthenticationMiddleware
    - 세션을 사용하여 사용자를 요청과 연결
  - Middleware
    - http 요청과 응답 처리 중간에서 작동하는 시스템
    - 장고는 http 요청이 들어오면 미들웨어를 거쳐 해당 URL에 등록된 view로 연결해주고, 응답 역시 미들웨어를 거쳐서 내보냄
    - 데이터 관리, 애플리케이션 서비스, 메시징, 인증 및 API 관리 담당



### 로그인

- session을 Create하는 로직과 동일

  - user를 Create하는 것은 회원가입 -> 로그인과 다름

- Django는 session의 메커니즘에 대해 생각하지 않게 도와주며, 인증에 관한 built-in forms 제공

- AuthenticationForm [공식문서](https://docs.djangoproject.com/en/3.2/topics/auth/default/#django.contrib.auth.forms.AuthenticationForm)

  - 사용자 로그인을 위한 폼으로, request를 첫번째 인자로 취함

- login 함수

  `login(request, user, backend=None)` 

  - 현재 세션에 연결하려는 인증된 사용자가 있는 경우 필요
  - view 함수에서 사용되며, HttpRequest 객체와 User 객체 필요
  - django의 session framework를 사용하여 세션에 user의 ID 저장 (== 로그인)

- get_user()

  - AuthenticationForm 의 인스턴스 메소드

    ```python
    class AuthenticationForm(forms.Form):
        ....
        def get_user(self):
            reutnr self.user_cache
    ```

  - user_cache는 인스턴스 생성 시에 None으로 할당되며 , 유효성 검사를 통과했을 경우 로그인한 사용자 객체로 할당

  - 인스턴스의 유효성 먼저 확인 후, 유효할 때만 user를 제공하려는 구조

- 로그인에 성공하면 session id 발급 -> DB의 django_session 테이블에서 확인 가능

  로그인에 성공한 후 개발자 도구 > Application > cookies 에서 sessionid 확인 가능

```python
# views.py 

@require_http_methods(['GET','POST'])
def login(request):
    # 로그인된 상태라면 메인페이지로 이동
    if request.user.is_authenticated:
        return redirect('articles:index')

    if request.method == 'POST':
        form = AuthenticationForm(request, request.POST)
        if form.is_valid():
            # 로그인
            auth_login(request, form.get_user())
            return redirect(request.GET.get('next') or 'articles:index')
    else:
        form = AuthenticationForm()
    context ={
        'form':form,
    }
    return render(request, 'accounts/login.html', context)
```

```html
# base.html

<a href="{% url 'accounts:login' %}">Login</a>

# 로그인 안된 상태에서만 로그인 버튼 보이기

{% if request.user.is_authenticated %}
  <h3>Hello, {{user}}</h3>
{% else %}
  <a href="{% url 'accounts:login' %}">Login</a>
{% endif %}
```





### 인증 데이터 in templates

- context processors
  - 템플릿이 렌더링될 때 자동으로 호출 가능한 context 데이터 목록
  - settings.py에서 확인 가능
  - `'django.contrib.auth.context_processors.auth',` 에 user 정보 포함
  - 현재 로그인한 사용자를 나타내는 auth.User 인스턴스가 템플릿 변수 `{{user}}`에 저장됨
    - 로그인하지 않으면 `AnonymousUser` 인스턴스



### 로그아웃

- session을 Delete하는 로직과 동일

- logout 함수

  `logout(request)` 

  - request객체를 인자로 받고, 반환값 X

  - 로그인하지 않은 경우 오류를 발생시키지 않음

  - 현재 요청에 대한 session data를 DB에서 삭제하고, 쿠키에서도 sessionid 삭제

    -> 이전 사용자의 세션 데이터로의 접근 방지 목적

```python
@require_POST
def logout(request):
    # 로그인된 사용자만 로그아웃 가능
    if request.user.is_authenticated:
        auth_logout(request)
    return redirect('articles:index')
```

```html
# base.html
# 로그인한 상태에서만 로그아웃 버튼 보이기

{% if request.user.is_authenticated %}  
  <form action="{% url 'accounts:logout' %}" method="POST">
    {% csrf_token %}
    <input type="submit" value='Logout'>
  </form>
{% endif %}
```



