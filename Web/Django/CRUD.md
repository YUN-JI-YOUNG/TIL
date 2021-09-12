### CRUD

###### Create(생성), Read(읽기), Update(갱신), Delete(삭제)를 묶어서 일컫는 말

#### 1. CREATE

1. 인스턴스 변수 생성 후 속성 값 부여 

```python
article = Article()
article.title = 'first'
article.content = 'django!'
article.save()
```

2. 인스턴스 변수 생성 시 초기값 부여

```python
article = Article(title='second', content='django!!')
article.save()
```

3. Query API 메서드 중 create 이용 

```python
Article.objects.create(title='third', content='django!!!')
# 생성과 저장 한 번에 처리 가능, 또한 쿼리 표현식 리턴   
```

-> [참고 공식문서](https://docs.djangoproject.com/en/3.2/ref/models/querysets/#create)   

- CREATE 관련 메서드    
  - `save()` 
    - 객체를 데이터베이스에 저장하며, ID값이 django가 아니라 DB에서 계산되기 때문에 save()를 호출하기 전까지는 객체의 ID 값 알 수 없음    
    - 단순히 모델을 인스턴스화하는 것은 DB에 영향을 미치지 않으므로 **반드시 save() 필요**   
  - `str`
    - 표준 파이썬 클래스의 메소드인 str()을 `models.py` 의 해당 class 내에서 정의하여 각 object가 사람이 읽을 수 있는 문자열을 반환하도록 할 수 있음   
    - str 메소드를 정의하는 것은 출력형태를 바꿀 뿐, DB에 영향을 주는 행위가 아니므로 `makemigration` 필요 없음 (설계도 변경 없으므로)   
    - 작성하면 **반드시 shell_plus 재시작**  

#### 2. READ

###### CRUD중 가장 중요한 것이며, 조회를 얼마나 잘 다루는지에 따라 데이터를 잘 다루는지 결정 가능    

- QuerySet API 메소드를 사용해 다양한 조회를 하는 것이 중요!

- QuerySet API 메소드는 크게 2가지로 분류   

  - querysets을 리턴하는 메소드
    - ex> `all()`  : querysets 리턴 
  - querysets을 리턴하지 않는 메소드   
    - ex> `create()` : 생성한 객체 리턴  

- `all()` : 현재 QuerySet의 복사본 반환    

- `get()`  : `Article.objects.get(pk=10)` 

  - 주어진 lookup(조회) 매개변수와 일치하는 객체를 반환 (1개)    

  - **DoesNotExist 예외** : 객체를 찾을 수 없을 때 발생

    **MultipleObjectReturned 예외** : 둘 이상의 객체를 찾았을 때 발생     

  - 예외를 발생시키지 않으려면 primary key와 같이 고유성을 보장하는 조회에서 사용해야 함    

    - **pk를 조회할 때만 사용**하는 것이 의도에 맞는 것        

- `filter()` 

  - 주어진 lookup(조회) 매개변수와 일치하는 객체를 포함하는 새 Queryset 반환   
  - 만약, 찾는 값이 없다면, 빈 Queryset 반환 
  - 그러므로 **pk가 아닌 다른 값을 조회**할 때 더 적절     

#### 3. UPDATE

- get() 메소드를 사용하여 수정할 값을 조회해서 가져오기 -> 원하는 필드 값 변경 

  -> `save()`메소드 사용하여 저장     

- 수정 완료해서 저장하면 `updated_at` 갱신       

#### 4. DELETE

- get() 메소드를 사용하여 수정할 값을 조회해서 가져오기 -> delete() 메소드 사용하여 삭제    
- delete() 메소드는 {삭제된 개체수, {객체유형 : 삭제 수}} 딕셔너리 번환   
- 삭제 후 다시 create 하면 변화없이 맨 마지막에 저장됨
  - 1번을 삭제했다고 1번에 생성되는 것이 아님!         



### CRUD with views 

- 사이트 간 요청 위조  (Cross-Site-Request-Forgery)

  - 웹 애플리케이션 취약점 중 하나   
  - 사용자가 자신의 의지와 무관하게 공격자가 의도한 행동을 하여 특정 웹페이지를 보안에 취약하게 한다거나 수정, 삭제 등의 작업을 하게 만드는 공격 방법     
  - django는 CSRF에 대항하여 middleware와 template tag 제공    

- CSRF 공격 방어   

  - Security Token 사용 방식(CSRF Token)

    - 사용자의 데이터에 임의의 난수 값 부여(hash 값)  

      매 요청마다 해당 난수 값을 포함시켜 전송시키도록 함    

      -> 데이터 + hash값 전송 (위조 사이트는 hash값 검증 불가)    

      -> 이후 서버에서 요청을 받을 때마다 전달된 token 값이 유효한지 검증   

  - DB 변경이 가능한 POST, PATCH, DELETE Method 등에 적용 (GET 제외)

  - django는 csrf token 템플릿 태그 제공   

  -> 전송을 보내는 `new.html` 의 form태그 내부 첫번째에 작성 `{% csrf_token %}` 

  -> input type이 hidden으로 작성되고 value에 hash 값 존재 (새로고침할 때 마다 변경)  

  -> 해당 태그가 없다면 응답 요청한 해당 템플릿이 내가 렌더링한 템플릿이 맞는지 의심 : **Forbidden (403)**

-> hash값은 `settings.py` 내의 `MIDDLEWARE` 의 `'django.middleware.csrf.CsrfViewMiddleware',`  (middleware)가 검사해서 검증      

**즉, new에서 해쉬값을 포함해서 요청을 create에 보내면 middleware가 그 해쉬값을 검증**    

​    

- CsrfViewMiddleware

  - 실제 요청 과정에서 `urls.py` 이전에 Middleware의 설정 사항들을 순차적으로 거침   

    응답은 반대로 하단 -> 상단으로 미들웨어 적용    

- Middleware

  - 공통 서비스 및 기능을 애플리케이션에 제공하는 소프트웨어    
  - 데이터 관리, 애플리케이션 서비스, 메시징, 인증 및 API 관리를 주로 미들웨어 통해 처리   
  - 개발자들이 애플리케이션을 보다 효율적으로 구축할 수 있도록 지원    
  - 애플리케이션, 데이터 <-> 사용자 를 연결하는 요소처럼 작동      

