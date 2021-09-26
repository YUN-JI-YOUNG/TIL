[TOC]



### 1. 환경 세팅

#### 1-1. 가상환경 들어가기

```python
$ source ../venv/Scripts/activate
```



#### 1-1. pip 설치 안된 것이 있다면 설치

```python
# requirements.txt 가 있다면
$ pip install -r requiremenets.txt

# requirements.txt 가 없다면
이전 환경에서 $ pip freeze > requiremenets.txt 로 생성 후 설치
```



###  2. 프로젝트 생성

```python
$ django-admin startproject crud .
```



### 3. 앱 생성 및 리스트 등록 / 언어 설정

```python
$ python manage.py startapp articles

# crud/settings.py
INSTALLED_APPS = [
    'articles',
    ...
]

LANGUAGE_CODE = 'ko-kr'

TIME_ZONE = 'Asia/Seoul'
```



### 4. base.html 생성

```python
templates 폴더 생성 > base.html 생성

1. 부트스트랩 CDN 삽입

2. 템플릿 상속이 가능하도록 block 지정 + container 클래스 지정으로 기본 여백 설정
  <div class="container">
    {% block content %}
    {% endblock content %}
  </div>

3. base_dir 설정
#crud/settings.py

TEMPLATES = [
    {
        ...
        'DIRS': [BASE_DIR, 'templates'],
        ...
    }
]
        
```



### 5. URL 매핑

```python
# crud/urls.py

from django.urls import path,include
urlpatterns = [
    ...
    path('articles/', include('articles.urls')),
    ...
]
```

url에서 articles까지 잘라내고 그 이후엔 articles 폴더 내부의 urls.py 라는 파일로 넘긴다 

-> articles 폴더 내부에 urls.py 파일 생성한 후 url 설정

```python
from django.urls import path
from . import views				# 함수를 불러오기 위해 import

app_name='articles'				# 앱을 더 생성할 경우를 대비해서 이름공간 생성
urlpatterns = [
    path('', views.index, name='index'),
]
```



### 6. views 함수 설정

```python
def index(request):
    return render(request, 'articles/index.html')
```

-> 앱 내의 templates 폴더 > articles 폴더 생성한 후 views 함수에 따른 html 문서 생성

(다른 앱이 있을 경우 장고에서 html 문서 탐색할 때 잘못 탐색하지 않도록 앱 이름으로 된 폴더 생성)

html 문서에서 base.html 상속

```html
{% extends 'base.html' %}
{% block content %}
  <h1>Index</h1>
  <hr>
{% endblock content %}
```



### 7. DB 설정을 위한 모델 설정

```python
# articles/models.py
from django.db import models

class Article(models.Model):
    title = models.CharField(max_length=30)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
```

- models 모듈 내의 Model 클래스 상속받아서 서브 클래스 생성
- 내부에 필드(속성) 작성

-> 설계도 작성 및 DB 적용

```python
# 설계도 작성
$ python manage.py makemigrations

# DB 적용
$ python manage.py migrate
```



### 8. ModelForm 설정

```python
# 위에서 설정한 모델을 기반으로 하는 form 설정

from django import forms
from .models import Article

class ArticleForm(forms.ModelForm):

    class Meta:
        model = Article
        fields = '__all__'
```



### 9. 관리자 계정 설정

```python
$ python manage.py createsuperuser
-> email 미입력해도 ok

# articles/admin.py
from .models import Article

admin.site.register(Article)

```

- 만약, 관리자 페이지에 나타나는 목록을 조정하고 싶다면, ArticleAdmin 클래스 추가 후 등록

```python
class ArticleAdmin(admin.ModelAdmin):
    list_display = ['pk', 'title', 'content', 'created_at', 'updated_at']

admin.site.register(Article, ArticleAdmin)
```



### 10. 본격적인 views 함수 및 html 파일 생성

#### 10-1. index

- views.py 에 index 함수 작성

```python
#articles/views.py

def index(request):
  #  articles = Article.objects.all()	# 1번째 글이 제일 위로
    articles = Article.objects.order_by('-pk')	# 최신순 정렬을 위해 거꾸로
    context={
        'articles':articles,
    }
    return render(request, 'articles/index.html', context)
```

- index.html 작성

```html
# articles/templates/articles/index.html

{% extends 'base.html' %}
{% block content %}
  <h1>Articles</h1>
  <a href="{% url 'articles:create' %}">NEW</a>			# 새글 생성
  <hr>
    
  {% for article in articles %}
  <h2>{{article.pk}}</h2>
  <h2>{{article.title}}</h2>
  <a href="{% url 'articles:detail' %}">DETAIL</a>
  <hr>
  {% endfor %}
    
{% endblock content %}
```



### CRUD 순서대로 작성

#### 10-2. create

- url 설정

```python
urlpatterns = [
    ...
    path('create/',views.create, name='create'),
    ...
]
```



- create 함수 작성
  - 새글을 생성하려고 할때 = request.method = "GET"일 때
    - 새글 생성하는 페이지 자체는 DB에 아무런 조작을 가하지 않으므로 GET 방식

```python
# articles/views.py

def create(request):
    form = ArticleForm()
    context={
        'form':form,
    }
    return render(request, 'articles/create.html', context)
```

​			하지만, 새글을 DB에 저장하려고 할 때는 POST 방식이어야 하므로 분기 필요

```python
def create(request):
    if request.method == "POST":
# POST방식으로 넘어온 데이터를 form에 담기
        form = ArticleForm(data=request.POST)		
# 유효성 검사하기 -> 통과하면 save로 DB에 저장 -> 저장한 값을 article 이라는 객체에 담기
        if form.is_valid():					
            article = form.save()
            return redirect('articles:detail', article.pk)
        
# POST 방식이 아니면, form에 빈 폼을 담기 -> 즉, 값을 입력할 수 있는 상태
    else:
        form = ArticleForm()
# 유효성 검사에 통과 못했을 경우는 context에 form에 에러메시지가 포함되서 담기니까 그 form을 넘겨서 출력
    context={
        'form':form,
    }
    return render(request, 'articles/create.html', context)
```

- create.html 작성

```html
{% extends 'base.html' %}
{% block content %}
  <h3>NEW</h3>
  <form method="POST">
   					  # {{form}} 위치는 input요소 대체니까 form태그 안으로!!!!
    {{form.as_p}}				# form을 사용함으로써 input 요소 대체
    <button>submit</button>
  </form>
  <a href="{% url 'articles:index' %}">BACK</a>
{% endblock content %}
```



#### 10-3. detail

- url 설정

```python
urlpatterns = [
    ...
    path('<int:pk>/', views.detail, name='detail'),
    ...
]
# variable Routing - url을 변수처럼 사용하는 것, 즉, url로 들어온 int형 변수인 pk값을 함수에 같이 넘겨줘서 인자로 사용함
```

- detail 함수 설정
  - detail 페이지를 불러오는 것은 DB에 아무런 조작을 가하지 않으므로 GET 방식

```python
def detail(request, pk):
    article = Article.objects.get(pk=pk)
    context={
        'article':article,
    }
    return render(request, 'articles/detail.html', context)
```

- detail.html 설정

```html
{% extends 'base.html' %}
{% block content %}
  <h3>Detail</h3>
  <hr>
  <h3>글 번호: {{article.pk}} </h3>
  <h3>글 제목: {{article.title}} </h3>
  <p>글 내용: {{article.content}} </p>
  <p>글 생성시각: {{article.created_at}} </p>
  <p>글 수정시각: {{article.updated_at}} </p>

  <a href="{% url 'articles:update' article.pk %}">EDIT</a>

  <form action="{% url 'articles:delete' article.pk %}">
    {% csrf_token %}
    <button class="btn btn-link text-decoration-none">DELETE</button>
  </form>
  <hr>

  <a href="{% url 'articles:index' %}">BACK</a>
{% endblock content %}
```



#### 10-4. Update  

- url 설정

```python
urlpatterns = [
    ...
    path('<int:pk>/update', views.update,name='update'),
    ...
]
# 몇 번째 글을 수정할 것인지 알아야하므로 pk 정보 받기
```

- update 함수 설정

  detail에서 edit버튼을 눌러서 update.html 를 렌더링하는건 DB에 조작을 가하지 않으므로 

  GET 방식

```python
def update(request, pk):
    article = Article.objects.get(pk=pk)	# 기존 내용을 article 객체에 저장
    form = ArticleForm(instance=article)	# 기존 내용을 덮어쓴 폼을 form에 저장
    context={
        'article':article,
        'form':form,
    }
    return render(request, 'articles/update.html', context)
```

​	만약, update.html에서 submit 버튼을 눌러서 db에 업데이트를 한다면 POST 방식이어야 함

```python
def update(request, pk):
# 기존 내용 불러오기
    article = Article.objects.get(pk=pk)
    if request.method == 'POST':
# 기존 내용 덮어쓴 폼의 기존 내용을 POST방식으로 보낸 데이터로 갱신하여 form에 저장
        form = ArticleForm(request.POST, instance=article)
 # 유효성 검사 -> 통과하면 저장 -> 해당 상세페이지로 리다이렉트
        if form.is_valid():		
            form.save()
            return redirect('articles:detail', pk)
    else: 
        form = ArticleForm(instance=article)
    context={
        'article':article,
        'form':form,
    }
    return render(request, 'articles/update.html', context)
```



- update.html 작성

```html
{% extends 'base.html' %}
{% block content %}
  <h3>EDIT</h3>

  <form method="POST">
    {% csrf_token %}
    {{form.as_p}}
    <button>submit</button>
  </form>

  <a href="{% url 'articles:detail' article.pk %}">BACK</a>
{% endblock content %}

```

   submit 버튼 누를때만 db에 조작을 가하므로 POST 방식 

  back으로 돌아가는건 조회이므로 GET 방식



#### 10-5. Delete

- url 설정

```python
urlpatterns = [
    ...
    path('<int:pk>/delete', views.delete, name='delete'),
    ...
]
```

- delete 함수 설정

  삭제는 DB에 직접 조작을 가하므로  무조건 POST 방식의 요청일 때만 함수 실행

  POST 이외의 방식이면, 해당 게시글의 상세정보 표시

```python
def delete(request, pk):
    if request.method == 'POST':
        article = Article.objects.get(pk=pk)
        article.delete()
        return redirect('articles:index')
    else:
        return redirect('articles:detail', pk)
```

​	delete는 html 페이지가 없으므로 함수만 설정하면 ok



### 11. 이미지 업로드 설정 추가

- setting 설정

  1. 정적 파일(static files) 설정

  - 기본 경로 설정 확인

  ```python
  # 기본 세팅
  STATIC_URL = '/static/'
  
  # 위의 세팅대로라면 기본적으로 앱 폴더 내의 static 폴더 탐색
  ```

  - static 파일의 기본 경로가 아닌 추가 경로에서 불러오는 경우 불러올 경로 설정

  ```python
  STATICFILES_DIRS =[BASE_DIR/'static']
  ```

  

  2. 미디어 파일(media files) 설정

  - 업로드된 파일들을 보관할 디렉토리의 절대 경로 설정 
    - 성능을 위해 DB에는 이미지 파일 자체가 아닌 경로가 저장

  ```python
  MEDIA_ROOT = BASE_DIR/'media'
  
  # STATIC_ROOT와 반드시 다른 경로 지정
  # 사용자가 파일을 업로드하면 이 경로에 파일이 자동 생성
  ```

    - 업로드된 파일의 주소를 만들 때 사용할 경로 설정
      - MEDIA_ROOT 와는 상관 없이 설정 가능
        - MEDIA_ROOT : 저장 경로 
        - MEDIA_URL : 이미지 주소 경로

  ```python
  MEDIA_URL = '/media/'
  
  # 위와 같이 경로를 지정하면 업로드된 파일의 주소가 http://127.0.0.1:8000/media/image.jpg 로 설정됨.
  
  # 만약 MEDIA_URL = '/images/' 로 지정한다면  http://127.0.0.1:8000/images/image.jpg
  
  # STATIC_URL와 반드시 다른 경로 지정
  ```

  

  3. 썸네일 이미지 설정

  - 썸네일 이미지를 제작하고 싶다면 django-imagekit 필요

  ```python
  $ pip install django-imagekit
  ```

  ```python
  INSTALLED_APPS = [
      ...
      'imagekit',
      ...
  ]
  ```

  

- urls 설정

```python
#crud/urls.py

from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    ...
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```



- model 설정 

  - 원본 저장

  ```python
  # articles/models.py
  
  class Article(models.Model):
      ...
      image = models.ImageField(blank=True)
      ...
  ```

  - 원본과 섬네일 크기 둘 다 필요한 경우

  ```python
  # 원본 저장 -> 섬네일과 상세페이지 이미지 크기 다르게 출력 가능
  from imagekit.models import ImageSpecField
  from imagekit.processors import ResizeToFill
  
  class Article(models.Model):
  	...
      image = models.ImageField(blank=True)
      image_thumbnail = ImageSpecField(
          source='image',
          processors=[ResizeToFill(100,50)],
          format='JPEG',
          options={'quality':80},
      )
      ...
  ```

  - 섬네일 크기만 필요한 경우

  ```python
  # 원본 저장 X
  
  from imagekit.models import ProcessedImageField
  from imagekit.processors import ResizeToFill
  
  class Article(models.Model):
      ...
      image = ProcessedImageField(
          upload_to='thumbnails/',
          processors = [ResizeToFill(100,50)],
          format='JPEG',
          options={'quality':90}
      )
      ...
  ```



- 마이그레이션  

  - 그 전에, `ImageField` 사용하기 위해선 Pillow 라이브러리 필요 

  ```python
  $ pip install Pillow
  ```

  - model에 필드 추가되었으므로 마이그레이션

  ```python
  $ python manage.py migrations
  $ python manage.py migrate
  ```



- INDEX 수정

  - index.html 에 정적 이미지 추가

    - static 태그를 사용하기 위해, 처음에 static 태그 세트를 로드

    ```html
    {% load static %}
    ```

    - static 태그를 사용하여 이미지 출력

    ```html
    {% block content %}
      <img src="{% static 'image.jpg' %}" alt="fall">
    	...
    {% endblock content %}
    ```

    ​	-> 해당 이미지는 BASE_DIR / static / image.jpg 경로에 존재

    ​	-> 템플릿처럼 앱 폴더 내에 static 폴더 만들어서 이미지 넣어둬도 ok

  - index.html에 섬네일 이미지 추가

    - 위에 이어서 추가
      - 이미지가 없어도 페이지가 정상적으로 렌더링되도록 if 분기 추가

    ```html
    {% for article in articles %}
        {% if article.image %}
          <img src="{{article.image_thumbnail.url}}" alt="{{article.image_thumbnail}} ">
        {% endif %}
        ...
      {% endfor %}
    ```

    

- CREATE 수정 

  - create.html에 이미지 업로드를 위한 수정

    - `form 태그`에 속성 추가 

    ```html
    <form method="POST" enctype="multipart/form-data">
        
    -> 이 속성값이 없다면, 이미지를 업로드해도 request에 해당 이미지 파일이 담기지 않음
        파일 업로드시 필수 속성!
    ```

  - views.create 함수 수정

    ```python
    # form태그에 추가한 인코딩 속성으로 request.FILES에 업로드된 파일이 담겨서 넘어오므로 form에 같이 담기
    if request.method=='POST':
    	form = ArticleForm(request.POST, request.FILES)
        ...
    ```



- UPDATE 수정

  - views.update 함수 수정

    ```python
    # create 함수와 마찬가지로 파일이 담겨진 request.FILES 추가
    
    if request.method=='POST':
    	form = ArticleForm(request.POST,request.FILES,instance=article)
        ...
    ```

    

- DETAIL 수정

  - detail.html 에 이미지 출력 추가

    - 원본 사이즈로 이미지 출력

    ```html
    {% if article.image %}
      <img src="{{article.image.url}}" alt="{{article.image}}">
    {% endif %}
    ```





### (Optional-1) 데코레이터 작성

```python
from django.views.decorators.http import require_http_methods, require_POST, require_safe

-> 특정 method 형식일때만 함수 실행, 허용하지 않는 method라면 http 405 상태코드
= return HttpResponseNotAllowed 

그냥 http 상태코드 500을 볼 때보다 보다 정확한 상황을 알 수 있음
```

다른 함수들은 데코레이터 써도 코드자체에 변화는 없지만, delete 함수는 변화

```python
@require_POST
def delete(request, pk):
    article = get_object_or_404(Article,pk=pk)
    article.delete()
    return redirect('articles:index')
```





### (Optional-2) 장고 숏컷 패키지 사용 - get 메서드 대체 

```python
from django.shortcuts import render, redirect, get_object_or_404
# render, redirect를 임포트한 숏컷 패키지에서 추가로 임포트

detail, delete, update 함수에서 get() 메서드로 객체를 찾던 코드를 변경

# 기존
article = Article.objects.get(pk=pk)
# 변경
article = get_object_or_404(Article,pk=pk)
```

- get() 메서드가 객체찾기 실패하면 

  -> 코드 실행 단계에서의 에러이므로  http 상태코드 500을 보여줌

- but, get_object_or_404 는 객체찾기 실패하면 

  http 404 를 raise

  -> 보다 정확한 상황 인식 가능



### (Optional-3) Create.html + update.html

create.html이랑 update.html이 둘 다 폼에 사용자 입력값을 받는 페이지라서 유사한 부분이 정말 많으므로 둘을 합칠 수 있다.

- 새로운 페이지 form.html 생성

```html
{% extends 'base.html' %}
{% block content %}
  {% if request.resolver_match.url_name == 'create' %}
  <h3>New</h3>
```

-> 신호 보내는 곳이 create 라면, 제목을 New 로 출력

```htm
  {% else %}
  <h3>Edit</h3>
  {% endif %}
```

-> 아니라면, 즉 , update라면, 제목을 Edit으로 출력

```html
  <form method="POST">
    {% csrf_token %}
    {{form.as_p}}
    <button>submit</button>
  </form>
```

-> 폼 부분과 제출 버튼은 공통

```html
 {% if request.resolver_match.url_name == 'create' %}
    <a href="{% url 'articles:index' %}">BACK</a>
  {% else %}
    <a href="{% url 'articles:detail' article.pk %}">BACK</a>
  {% endif %}
{% endblock content %}
```

-> Back을 누를때, create 에서 누른거면, index 페이지로 돌아가고, 

​							update에서 누른거면 해당 게시글의 상세 페이지로 이동

- views.py의 update함수와 create 함수에서 페이지 렌더링 부분만 변경

```python
# 기존 create
return render(request, 'articles/create.html', context)
# 변경
return render(request, 'articles/form.html', context)

# 기존 update
return render(request, 'articles/update.html', context)
# 변경
return render(request, 'articles/form.html', context)
```

