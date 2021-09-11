## Model

- 단일 데이터에 대한 정보 보유     
  - 사용자가 저장하는 데이터들의 필수적인 필드, 동작 포함    
- 저장된 데이터베이스의 구조(layout)
- django는 model을 통해 데이터에 접속 및 관리하며, 각각의 model은 하나의 DB 테이블에 매핑   
- 웹 애플리케이션의 데이터(DB)를 구조화하고 조작하기 위한 도구      

### Database(DB)

###### 체계화된 데이터의 모임 / databse != Model

- 쿼리(Query) : 데이터를 조회하기 위한 / 조건에 맞는 데이터를 추출하거나 조작하는 명령어   
  - '쿼리를 날린다' = 'DB를 조작한다'
- 기본 구조    
  - 스키마(Schema) : DB에서 자료의 구조, 표현방법, 관계 등을 정의한 구조    
    - column 과 해당 열의 datatype 이 작성되는 등 (id - INT, name - TEXT 등)           
  - 테이블(Table)  : 열과 행의 모델을 사용해 조직된 데이터 요소들의 집합     
    - 열(column) : 필드(field) or 속성 
      - 각 열에는 고유한 데이터 형식 지정      
    - 행(row) : 레코드(record) or 튜플     
      - 테이블의 데이터가 저장되는 곳     
    - SQL 데이터베이스에서는 테이블을 관계라고도 함         
  - PK(기본키) : 각 행의 고유값으로 Primary Key로 불리며, 반드시 설정해야 하고, DB 관리 및 관계 설정 시 주요하게 활용     
    - 실제 테이블의 필드명은 `id` 이지만, `pk`로도 조회가능(Django 자체적으로 숏컷 제공)    



### ORM 

###### object-Relational-Mapping 

- 객체 지향 프로그래밍 언어를 사용하여 호환되지 않는 유형의 시스템 간에(Django - SQL) 데이터를 변환하는 프로그래밍 기술   
- OOP 프로그래밍에서 RDBMS을 연동할 때, DB와 객체 지향 프로그래밍 언어 간의 호환되지 않는 데이터를 변환하는 프로그래밍 기법      
- Django는 내장 Django ORM 사용     
- 장점
  - SQL을 잘 알지 못해도 DB 조작 가능하며 SQL의 객체 지향적 접근으로 인한 높은 생산성    
- 단점
  - SQL을 잘 모르는 상태에서 ORM만으로는 완전한 서비스를 구현하기 어려운 경우가 있음     
- 현대 Web Framework의 요점은 웹 개발 속도 증가 (**생산성**)    
- DB를 객체로 조작하기 위해 ORM 사용



## Django Model

- models.py 작성

```python
# 스키마를 class 형태로 작성 
# pk와 속성을 포함해서 테이블의 컬럼(열)은 3개    
class Article(models.Model):     # models 모듈의 Model 클래스 상속 - 서브 클래스 
# 각 필드(컬럼)는 클래스 속성으로 지정 
# 각 속성은 각 DB의 열에 매핑(연결)  
    title = models.CharField(max_length=10)
    # models 의 CharField 메서드 = 데이터 타입
    content = models.TextField()                
    # TextField는 최대 길이(글자수 제한) 설정 불가
# 이렇게 필드 2줄만 작성해도 되는 것은 나머지는 Model 클래스가 갖고 있기 때문
```

- CharField(max_length=None, **options)  

  - 길이의 제한이 있는 문자열을 넣을 때 사용하며 `max_length` 는 필수 인자

  - 필드의 최대 길이(문자), DB 레벨과 Django의 유효성 검사(값 검증)에서 활용      

    - 길이 제한 맞지 않는 데이터는 필터링 가능     

  - 참고 : [공식문서](https://docs.djangoproject.com/en/3.2/ref/models/fields/#charfield) 

    

- TextField(**options)  

  - 글자의 수가 많을 때 사용   
  - `max_length` 옵션 작성시 자동 양식 필드인 textarea 위젯에 반영은 되지만, 모델과 DB 수준(DB 레벨 or 유효성 검사 등)에는 적용되지 X
    - 사용자가 강제로 늘려서 보내면 필터링 불가    
    - `max_length` 옵션 사용한다고 에러가 나진 않지만 CharField 에서 사용 권장      

  - 참고 : [공식문서](https://docs.djangoproject.com/en/3.2/ref/models/fields/#textfield) 

- DateField's options ( DateField = DateTimeField의 부모 클래스 )

  - auto_now_add -> `created_at` 필드에서 사용

    - 최초 생성 일자    

    - django ORM이 최초 insert시에만 현재 날짜, 시간으로 갱신 

      = 테이블에 어떤 값을 최초로 넣을 때만 갱신    

  - auto_now -> `updated_at` 필드에서 사용   

    - 최종 수정 일자   
    - django ORM이 save 할 때마다 현재 날짜, 시간으로 갱신    



### Migrations(마이그레이션)

###### Django가 model에 생긴 변화를 DB에 반영하는 방법    

- 실행 3단계

  1. `models.py` - model 변경사항 발생 시

  2. `python manage.py makemigrations` : migrations 파일 생성

  3. `python manage.py migrate` : DB 반영 (모델과 DB 동기화)

- 마이그레이션 실행 및 DB 스키마를 다루기 위한 몇가지 명령어 

  - makemigrations

    - model을 변경한 것에 기반한 새로운 마이그레이션 생성 (= DB에 보내기 위한 설계도)     

    - DB에 영향을 주는 변경사항이 아니면 makemigration 해도 반응 없음    

      -> 에러나지 않으니까 확실하지 않으면 하고 볼 것!    

    - 각 앱 폴더 내의 `migrations` 폴더에 파일 생성 (ex> `0001_initial.py`  ) 

    - 이 설계도는 ORM에 의해 SQL 언어로 변환되어 DB에 전달       

    - DB는 기본적으로 빈 데이터를 허용하지 않으므로 필드의 기본값이 없는데 마이그레이션을 생성하려고 하면, 아래 2가지 중 선택      

    ```python
    1) Provide a one-off default now (will be set on all existing rows)
    # 지금 기본값을 정하거나
    2) Quit, and let me add a default in models.py
    # 나가서 코드에서 정해라
    -> 1 선택 > 메서드에 따라 임의로 장고에서 설정한 기본값을 사용할거면 엔터
    ```

  - migrate

    - makemigrations 으로 **설계도 생성 후** migrate 사용!

    - 마이그레이션을 DB에 반영하기 위해 사용 (즉, 설계도를 실제 DB에 반영하는 과정)     

      = 모델에서의 변경 사항들과 DB의 스키마가 동기화를 이루는 과정      

    - migrate 한 후, db 파일 (`db.sqlite3`) 오른쪽버튼 > `open database` > 파일목록 맨 아래 > `SQLITE EXPLORER` > `db.sqlite3` 

      -> `앱이름_클래스이름` 으로 이루어진 테이블 중 원하는 테이블 클릭하면 스키마 확인 가능 

      -> 테이블 이름 오른쪽 ▶ 버튼 클릭 시 데이터 확인 가능    

  - sqlmigrate   

    - sqlmigrate [앱 이름] [설계도 번호]  
      - ex. `sqlmigrate articles 0001`
    - 마이그레이션에 대한 SQL 구문을 보기 위해 사용하며, 마이그레이션이 SQL문으로 어떻게 해석되어 동작할지 미리 확인 가능    

  - showmigrations    

    - 프로젝트 전체의 마이그레이션 상태 확인하기 위해 사용하며, 마이그레이션 파일들이 migrate 되었는지 여부 확인 가능    
    - `[X]` : 완료 체크     



### DataBase API

###### DB를 조작하기 위한 도구

- django가 기본적으로 ORM을 제공함에 따른 것    

  -> DB를 편하게 조작할 수 있도록 도움   

- 파이썬으로 객체 지향적으로 작성    

- Model을 만들면 django는 객체들을 make, read, modify, delete 가능한 database-abstract API 자동 생성    

- database-abstract API / database-access API 라고도 함    

- 문법적 구문 - Making Queries    

  - [Class Name] `.` [Manager] `.` [QuerySet API] 

    - ex> `Article.objects.all()`  

  - Manager

    - 데이터베이스와 통할 수 있는 여러 메서드(QuerySet API )들을 갖고 있음   
    - django 모델에 데이터베이스 query 작업이 제공되는 인터페이스   
    - **기본적으로 모든 django 모델 클래스에 objects라는 Manager 추가**       

  - QuerySet

    - 데이터베이스로부터 전달받은 객체 응답 목록     

    - 대괄호(`[]`) 로 감싸져있어서 리스트처럼 접근 가능

      -> html 문서에서 for 태그 사용 가능    

    - queryset 안의 객체는 0개, 1개 혹은 여러개일 수 있음   

    - DB로부터 조회, 필터, 정렬 등 수행 가능    

- Django shell

  - DB와 소통하기 위한 쉘 / DB API를 작성(input)하면 해당하는 queryset 반환(output)

  - 일반 파이썬 쉘에선 django 프로젝트 환경에 접근 불가(= DB에도 접근 불가)    

    -> django 프로젝트 설정이 road된 파이썬 쉘을 활용해 DB API 구문 테스트    

  - [django-extensions](https://django-extensions.readthedocs.io/en/latest/)  : 기본 Django shell보다 더 많은 기능을 제공하는 shell_plus 설치

    `pip install django-extensions` 

    -> `settings.py` 파일의 `INSTALLED_APPS` 리스트에 `*django_extensions*` **추가 필수!**

  - `python manage.py shell_plus` 로 실행   

  - ` pip install ipython` : 컬러링 위한 부가적인 pip 설치   

  - `exit()` 로 종료       

