### Static File

[공식문서](https://docs.djangoproject.com/ko/3.2/howto/static-files/)

- 정적 파일이며 응답할 때, 별도의 처리 없이 파일 내용을 그대로 보여주는 파일    

- 구성

  - INSTALLED_APPS에 `'django.contrib.staticfiles',` 가 있는지 여부 확인   

  - 마지막 부분에 `STATIC_URL = '/static/'` 정의  

    - 실 환경에서 STATIC_ROOT에서 정적파일을 참조할 때 사용하는 URL

      - 개발 단계에서는 실제 정적파일들이 저장되어 있는 기본 경로 (`app/static/`) 및 STATICFILES_DIR에 정의된 추가 경로 탐색    

      - STATICFILSE_DIR은 추가 파일 디렉토리에 대한 전체 경로를 포함하는 문자열 목록

        ```python
        # settings.py
        
        STATICFILES_DIRS=[
            BASE_DIR / 'static',		# 이미지가 있는 실제 경로 작성
        ]
        
        # 만약 BASE_DIR/ static/ images/ image.jpg라면,
        # 추후 static 태그를 사용할 때 아래와 같이 작성하면 된다.
        <img src="{% static 'images/image.jpg' %}" alt="sample image2">
        ```

        

    - 실제 파일이나 디렉토리가 아니며 URL로만 존재하고, 값을 설정한다면,반드시 `/` 로 끝나야 함

  - 템플릿에서 static 템플릿 태그 사용하여 URL 빌드     

    - `{% load static%}` 

      - 사용자 정의 템플릿 태그 세트를 로드하여 해당 라이브러리,패키지에 등록된 모든 태그와 필터 로드   

    - `{% static ' ' %}` 

      - STATIC_ROOT에 저장된 정적파일에 연결   

      - img 태그의 src 속성에 입력

      - 기본 경로 및 추가 경로 `이후 경로`를 작성

        - 만약 앱 내의 static 폴더 내의 articles 폴더 내에 있는 이미지라면, 

          `{% static 'articles/다운로드.jpg' %}` 로 작성

    - STATIC_ROOT

      - collectstatic이 배포를 위해 정적파일을 수집하는 디렉토리의 절대 경로이며 모든 정적 파일을 한 곳에 모아 넣는 경로    

      - 개발과정에서 setting.py의 DEBUG값이 True이면 해당 값이 적용되지 않고, 작성하지 않는 한, setting.py에 작성되어 있지 않음 

        -> 실제 배포하는 실 서비스 환경에서 장고의 모든 정적 파일(관리자 페이지 디자인 등)을 다른 웹 서버가 직접 제공하기 위한 것   

        -> 장고는 본인의 정적파일이므로 따로 경로 지정 필요 X        

      - collectstatic을 이용한 정적파일 수집 방법   

        - STATIC_ROOT 작성 `STATIC_ROOT = BASE_DIR / 'staticfiles' ` 

          -> `$ python manage.py collectstatic` 명령어 작성 -> `staticfiles/admin 폴더` 내에 모든 정적파일을 모아준다

  - 앱의 static 폴더에 정적 파일 저장  

    - templates과 비슷    



#### Media file

- 사용자가 웹에서 업로드하는 정적파일 = 유저가 업로드한 모든 정적 파일     

- FileField

  - 파일 업로드에 사용하는 모델 필드이며, `upload_to` 선택인자 보유

    - upload_to  

      - 업로드 디렉토리, 파일 이름을 설정하는 2가지 방법 제공   

        - 문자열 값 or 경로 지정 : 파일 업로드 날짜/시간으로 대체되는 파이썬의 strftime() 형식 포함 가능

          ```python
          upload_to='uploads/%Y/%m/%d' 
          # MEDIA_ROOT/2021/09/08 로 저장
          
          upload_to='uploads/'
          # MEDIA_ROOT/uploads 로 저장
          ```

        - 함수 호출 방식

          - 2개의 인자(instance, filename) 사용   
            - instance : FileField가 정의된 모델의 인스턴스
            - filename : 기존에 제공된 파일 이름

          ```python
          def articles_image_path(instance,filename):
              return f'user_{instance.user.pk}/{filename}'
          # MEDIA_ROOT / user_pk / filename 으로 저장하겠다
          
          class Article(models.Model):
              image = models.ImageField(upload_to=articles_image_path)
          ```

          

- ImageField

  - 이미지 업로드에 사용하는 모델 필드이며, FileField를 상속받는 서브클래스이기 때문에, FileField의 모든 속성 및 메소드 사용가능하며, 유효성 검사 필요    
  - ImageField로 생성된 객체는 최대 100자로 정해져있으며, max_length로 길이 변경 가능
    - DB에는 이미지 경로가 문자열로 저장되기 때문   

- MEDIA_ROOT  

  - 성능을 위해 DB에는 미디어 파일이 아닌, 경로가 저장되므로 

    업로드된 파일(미디어 파일)들을 보관할 디렉토리의 절대 경로   

  - STATIC_ROOT와는 **반드시** 다른 경로로 지정

  - 경로 지정하면 사용자가 파일을 업로드했을 때 해당 경로에 자동 생성   

  ```python
  MEDIA_ROOT = BASE_DIR / 'media'
  ```

- MEDIA_URL

  - MEDIA_ROOT  에서 제공하는 미디어를 처리하는 URL 이며, 업로드된 파일의 주소를 만들어주는 역할 = 웹 서버 사용자가 사용하는 public URL   
  - STATIC_URL 처럼, 값을 설정한다면 `/` 로 끝나야하며, STATIC_URL 과는 다른 경로 필요 

  ```python
  MEDIA_URL = '/media/'
  ```  

</br>   

## 이미지  


### 이미지 업로드 (CREATE)

- `models.py` 에 `ImageField`를 갖는 속성 작성

  - 선택인자 2가지

    - upload_to -> FileField 인자 /  MEDIA_ROOT의 하위 경로 결정
      - 실제 이미지가 저장되는 경로 지정
    - blank = True : 이미지 필드에 빈 값이 허용되도록 설정 (이미지 선택적 업로드) 
      - 기본 값은 False
      - True로 설정되어 있으면, 빈 문자열이어도 유효성 검사 통과 가능     

    [참고]

    - Model field option "null"

      - 기본 값 : False

      - True면 빈 값을 DB에 NULL로 저장

      - blank = 유효성 검사, null = DB 이므로, form에서 빈 값 허용은 **blank=True** 사용

      - 주의사항 

        - CharField, TextField, ImageField 같은 문자열 기반 필드엔 사용을 피해야함 

          왜냐하면, True로 설정 시 `데이터 없음`에 빈 문자열/ NULL 의 2가지 의미를 갖게 되는데, 이렇게 두 개의 가능한 값을 갖는 것은 중복이므로 

          대부분의 경우에서 Django는 NULL이 아닌 빈 문자열을 사용하는 것이 규칙    

  ```python
  # upload_to - 문자열로 작성
  image = models.ImageField(upload_to='images/', blank=True)
  # media/images 저장
  
  # upload_to - 파이썬의 strftime() 형식
  image = models.ImageField(upload_to='%Y/%m/%d', blank=True)
  # media/2021/09/08 저장
  ```

  ```python
  # upload_to - 함수 호출 방식
  def articles_image_path(instance,filename):
      return f'user_{instance.pk}/{filename}'
  
  class Article(models.Model):
      ...
      image = models.ImageField(upload_to=articles_image_path)
  
  # media/user_None 저장 -> 업로드 당시엔 저장 전이므로 pk 값이 없음
  # 다만 수정하면 media/user_7 저장 -> 저장된 후이므로 pk 값 존재
  ```

  

- migrate하기 위해 ImageField를 사용하기 위해선 **Pillow 라이브러리** 필요 ->  [문서](https://pypi.org/project/Pillow/)   

- 실제 생성 후 개발자 도구로 살펴보면, `<input type='file'> ` 

- form 요소의 인코딩 요소 - `enctype="multipart/form-data"`

  - 파일 / 이미지 업로드 시에 반드시 사용
  - `<input type='file'> `  사용할 경우에 사용  

- form 요소의 accept 속성 [공식문서](https://developer.mozilla.org/ko/docs/Web/HTML/Element/Input/file#attr-accept)

  - `accept="image/*"` : `image/*`는 "모든 이미지 파일"을 의미

  - 파일 업로드 시 파일 형식을 image로 기본 지정(필터링) 

    -> 파일 검증은 아니며, 다른 파일도 업로드 가능 

  - `<input type="file" accept="image/*,.pdf">` : pdf 파일도 가능   

  - 사용자 경험 측면에서 도움   

- 파일은 request.POST가 아닌, request.FILES 에 들어있으므로 views 함수 수정 필요   

  ```python
  def create(request):
      if request.method == 'POST':
          form = ArticleForm(request.POST, request.FILES)
          ...
   # , 로 연결할 수 있는 것은 첫번째 인자가 data고, 두번째 인자가 files이기 때문
  # 즉, form = ArticleForm(data=request.POST, files=request.FILES)
  ```

  

### 이미지 업로드 (READ)

- 사용자가 업로드한 파일 제공   

  - MEDIA_ROOT , MEDIA_URL 설정 완료한 후, 프로젝트의 urls.py 에 아래처럼 설정   

    ```python
    # crud/urls.py
    
    from django.conf import settings
    from django.conf.urls.static import static
    
    urlpatterns = [
        # ... 
    ] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
    ```

- 업로드 파일의 경로 `{{article.image.url}}` , 업로드 파일의 이름 `{{article.image}}`  

- 만약, 업로드 파일 이름이 겹친다면 Django가 알아서 뒤에 난수 덧붙임

  ​	ex. `sample.jpg` / `sample_Udjof.jpg ` 

- STATIC_URL과 MEDIA_URL 

  - 서버에 요청하는 url을 `urls.py` 가 아닌, 

    `settings.py` 에 작성한 후 urlpatterns에 추가하는 형식



### 이미지 업로드(UPDATE)

- 이미지는 하나의 덩어리이기 때문에 일부 수정은 불가 -> 새로운 이미지로 덮어씌워야 함

- update 함수 수정

  ```python
  def update(request, pk):
      article = get_object_or_404(Article, pk=pk)
      if request.method == 'POST':
          form = ArticleForm(request.POST, request.FILES, instance=article)
          ...
  ```

  - 이미지 삭제 : 수정 페이지에서 이미지 옆에 취소버튼 클릭하고 수정버튼 클릭
  - 이미지 수정 : 새로운 이미지 다시 불러온 후 수정버튼 클릭

- detail 페이지에서 이미지가 없으면 에러 발생 -> if 분기로 해결

  ```html
  {% if article.image %}
      <img src="{{ article.image.url}} " alt="{{article.image}}">
    {% else %}
      <img src="{% static 'images/unnamed.png' %}" alt="default image">
    {% endif %}
  ```

  - 이미지(`article.image`)가 있는 경우에만 이미지 출력, 없으면 기본 이미지 출력
    - else분기 - 기본 이미지 설정은 선택사항!   

  

### 이미지 리사이징

- 실제 원본 이미지를 서버에 그대로 업로드하는 것은 서버에 부담이 큰 작업 

  -> css 스타일 수정은 출력을 변경하는 것이므로 영향 X 

  -> 업로드 될때 이미지 자체를 리사이징

  - django-imagekit 라이브러리 활용   

    - `pip install django-imagekit`
    - INSTALLED_APPS 에 `'imagekit'` 추가
    - [model 작성법](https://github.com/matthewwithanm/django-imagekit#specs)

    ```python
    # 원본 저장 X
    from imagekit.models import ProcessedImageField
    from imagekit.processors import ResizeToFill
    # ppt에선 Thumbnail를 import -> 직접 해보기
    
    class Article(models.Model):
        ...
        image = ProcessedImageField(
            upload_to = 'thumbnails/',
            processors = [ResizeToFill(100,50)],
            format = 'JPEG',
            options ={'quality':60}
        )
        ...
        
    # 참고용 - 원본 저장
    # image_thumbnail 은 실제 사용하고난 후 -> media/CACHE/.. 폴더 내에 생성
    from imagekit.models import ImageSpecField
    from imagekit.processors import ResizeToFill
    
    class Article(models.Model):
        ...
        image = models.ImageField(upload_to='origins/', blank=True)
        image_thumbnail = ImageSpecField(
            source='image',
            processors=[ResizeToFill(100,50)],
            format='JPEG',
            options={'quality': 90}
        )
        ...
    
    -> detail.html 에서 변경
    # 기존
     <img src="{{ article.image.url}} " alt="{{article.image}}">
        
    # 변경
     <img src="{{ article.image_thumbnail.url}} " alt="{{article.image_thumbnail}}">
    ```

    

 





