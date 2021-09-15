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

