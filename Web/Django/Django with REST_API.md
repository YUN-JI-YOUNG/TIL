## Response   

- 이젠 장고에 html 문서가 아닌 json 형태로 응답   

- 프로젝트 진행을 위한 더미 데이터 생성     

  - django-seed 라이브러리 이용 

     `python manage.py seed articles --number=20`     

- JSON을 응답하는 방법   

  1. JSONResponse 객체 활용   

     ```python
     def article_json_1(request):
         articles = Article.objects.all()
         articles_json = []
     
         for article in articles:
             articles_json.append(
                 {
                     'id': article.pk,
                     'title': article.title,
                     'content': article.content,
                     'created_at': article.created_at,
                     'updated_at': article.updated_at,
                 }
             )
         return JsonResponse(articles_json, safe=False)
     ```

     - Content-type   

       - 데이터의 media type을 나타내기 위해 사용   
       - 응답 내에 있는 컨텐츠의 컨텐츠 유형이 실제로 무엇인지 클라이언트에게 알려줌   
       - 개발자도구 > Network > json-1/ > Response Headers > Content-type 확인    

     - JSONResponse objects    

       - JSON로 인코딩된 응답을 만드는 HttpResponse의 서브 클래스   

       - safe 파라미터    

         - dict 이외의 객체를 직렬화(해석) 하려면 False로 설정     

           ex> 데이터를 리스트로 넣으면 False로 설정     

       - 직렬화 (Serialization)   

         - 데이터 구조나 객체 상태를 동일하거나 다른 컴퓨터 환경에 저장하고 나중에 재구성할 수 있는 포맷으로 변환하는 과정   
         - 장고에선 Queryset 및 Model Instance 같은 복잡한 데이터를 Python 데이터 타입(JSON, XML 등의 유형으로 쉽게 변환 가능)으로 만들어 준다.      

  2. Django Serializer   

     ```python
     def article_json_2(request):
         articles = Article.objects.all()
         data = serializers.serialize('json', articles)
         return HttpResponse(data, content_type='application/json')
     ```

     - 장고의 내장 HttpResponse를 활용한 JSON 응답 객체   
     - 주어진 모델 정보를 활용하기 때문에 첫번째 방법과 달리 필드를 개별적으로 만들어줄 필요 X   

  3. Django REST framework   

     - 장고 REST framework (DRF) 라이브러리를 사용한 JSON 응답  

     - `pip install djangorestframework`  

       -> INSTALLED_APPS 에 `'rest_framework'` 추가     

     ```python
     from .serializers import ArticleSerializer
     from rest_framework.response import Response
     
     @api_view()
     def article_json_3(request):
         articles = Article.objects.all()
         serializer = ArticleSerializer(articles, many=True)
         return Response(serializer.data)
     ```

     - Article 모델에 맞춰 자동으로 필드를 생성해 serialize 해주는 ModelSerializer    

       - serializer에 대한 유효성 검사기를 자동으로 생성    

       - `.create()` , `.update()`  의 간단한 기본 구현 포함    

       - 생성 방법   

         - `serializers.py` 파일 생성   
         - 모델폼과 굉장히 유사하게 작성    

         ```python
         from rest_framework import serializers
         from .models import Article
         
         class ArticleSerializer(serializers.ModelSerializer):
         
             class Meta:
                 model = Article
                 fields = '__all__'
         ```

     - DRF 라이브러리   

       - Web API 구축을 위한 강력한 Toolkit 제공   

         |          | **Django** | DRF        |
         | -------- | ---------- | ---------- |
         | Response | HTML       | JSON       |
         | Model    | ModelForm  | serializer |

</br>  



## Single Model   

- Build RESTful API   

  |             | GET          | POST    | PUT        | DELETE     |
  | ----------- | ------------ | ------- | ---------- | ---------- |
  | articles/   | 전체 글 조회 | 글 작성 |            |            |
  | articles/1/ | 1번글 조회   |         | 1번글 수정 | 1번글 삭제 |



- DRF with Single Model   

  - 단일 모델의 data를 직렬화하여 JSON으로 변환하는 방법에 대한 학습   

  - 단일 모델을 두고 CRUD 로직을 수행 가능하도록 설계   

  - API 개발을 위한 핵심 기능을 제공하는 도구 활용   

    - DRF built-in form   
    - Postman   
      - API를 구축하고 사용하기 위해 여러 도구를 제공하는 API 플랫폼   
      - 설계, 테스트, 문서화 등의 도구를 제공함으로서 API를 더 빠르게 개발 및 생성할 수 있도록 도움   

  - ModelSerializer   

    - Model의 필드를 어떻게 직렬화 할지 설정하는 것이 핵심    

    - 작성 후 장고 쉘에서 확인 가능    

      - `from articles.serializers import ArticlesSerializer`    

      - `serializer = ArticleSerializer()`  

      - 단일 객체가 아닌 QuerySet 등을 직렬화 하기 위해서는 many 키워드 인자 필요   

        `articles = Article.objects.all()` 

        `serializer = ArticleSerializer(articles, many=True)`     

  - api_view decorator   

    `from rest_framework.decorators import api_view` 

    `@api_view(['GET','POST'])`     

    - 기본적으로 GET 메서드만 허용 + 다른 메서드 요청에 대해서는 405 Method Not Allowed로 응답     
    - View 함수가 응답해야하는 HTTP 메서드의 목록을 리스트의 인자로 받음   
      - 장고의 데코레이터와 유사   
    - DRF에선 **필수**로 작성해야 view 함수가 정상적으로 동작    

</br>   

<hr>   

### Read(전체) & Create   

###### article_list 함수로 처리    

#### GET   

- 전체글 조회

  - serializers 모델 작성   

    ```python
    class ArticleListSerializer(serializers.ModelSerializer):
    
        class Meta:
            model = Article
            fields = ('id','title',) 
    ```

    - 인덱스 페이지에선 id와 title 정도만 필요하므로 필요한 필드만 받아오기     

  

  - url 작성   

    ```python
    path('articles/',views.article_list),
    ```

  - view 함수 작성  

    ```python
    @api_view(['GET'])
    # article_list 함수로 게시글 조회 & 생성 처리 
    def article_list(request):
        articles = get_list_or_404(Article)  
        serializers = ArticleListSerializer(articles, many = True)
        return Response(serializers.data)
    ```

    - 일반적인 웹 서버에서는 `get_list_or_404` 함수를 쓰면 게시글이 하나도 없을 경우 404 에러 출력하므로 사용하지 않고 대신 `Article.objects.all()` 등을 사용       

    - 하지만 API 서버로서 작동할 때에는 이렇게 쓰는게 맞으므로 사용   

      -> 이젠 페이지를 넘겨주는 것이 아니기 때문에 찾는 객체가 없으면 에러 출력하는 게 올바른 반응         



#### POST   

- 게시글 생성    

  - 생성이 완료되면 201 Created 상태 코드 및 메시지 응답   

  - views 함수에 POST 메서드 추가   

    ```python
    from rest_framework import serializers, status
    
    @api_view(['GET','POST'])
    def article_list(request):
        if request.method == 'GET':
            articles = get_list_or_404(Article)  
            serializers = ArticleListSerializer(articles, many = True)
            return Response(serializers.data)
    
        # 게시글 생성   
        elif request.method == 'POST':
            serializer = ArticleSerializer(data=request.data) 
            if serializer.is_valid():
                serializer.save()
                return Response(serializer.data, status=status.HTTP_201_CREATED)
            return Response(serializer.errors, status = status.HTTP_400_BAD_REQUEST)
    ```

    - `is_valid()` , `.save()` 등의 메서드들은 장고 유저들이 혼동하지 않도록 메서드 이름을 맞춰둔 것 -> 실제 같은 메서드인 것은 아님      

    - status code를 단순히 `status=201` 같은 표현으로 사용할 수 있지만  

      DRF 에서는 status 모듈을 통해 정의된 상수 집합을 제공하고, 해당 집합 사용을 권장        

    - status 모듈에는 HTTP status code 집합이 모두 포함 -> status code를 보다 명확하고 읽기 쉽게 만들어준다.          

    - 유효성 검사를 통과하지 못하면 자동으로 400 코드를 리턴하는데 굳이 return 문을 또 쓰고 싶지 않다면 `raise_exception` 인자 사용   

      ```python
      elif request.method == 'POST':
              serializers = ArticleSerializer(data=request.data)
              if serializers.is_valid(raise_exception=True):
                  serializers.save()
                  return Response(serializers.data, status=status.HTTP_201_CREATED)
      ```

      -> 마지막줄 생략 가능   

      - raise_exception    
        - is_valid()는 유효성 검사 오류가 있는 경우 serializers.ValidationError 예외를 발생시키는 선택적 raise_exception 인자 사용 가능   
        - DRF에서 제공하는 기본 예외처리기에 의해 자동 처리    
        - `Default `: 응답으로 HTTP status code 400 반환      

</br>   

<hr>   

### Read(상세) & Delete & Update   

###### article_detail 함수로 처리   

#### GET   

- 특정 게시글 조회   

  - serializers 모델 작성   

    ```python
    class ArticleSerializer(serializers.ModelSerializer):
    
        class Meta:
            model = Article
            fields = '__all__'
    ```

    - 특정 게시글을 조회하는 것이므로 필드를 모두 받아와야 함   

  - url 작성   

    ```python
    path('articles/<int:article_pk>/', views.article_detail),
    ```

  - view 함수 작성    

    ```python
    @api_view(['GET'])
    def article_detail(request, article_pk):
        article = get_object_or_404(Article, pk=article_pk)
        serializer = ArticleSerializer(article)
        return Response(serializer.data)
    ```


  #### DELETE    

- 게시글 삭제   

  - 삭제가 완료되면 204 No Content 상태 코드 및 메시지 응답    

  - view 함수에 DELETE 메서드 추가   

    ```python
    @api_view(['GET','DELETE'])
    def article_detail(request, article_pk):
        article = get_object_or_404(Article, pk=article_pk)
        if request.method == 'GET':
            serializer = ArticleSerializer(article)
            return Response(serializer.data)
        
        # 게시글 삭제   
        elif request.method == 'DELETE':
            article.delete()
            data = {
                'delete':f'데이터 {article_pk}번이 삭제되었습니다.'
            }
            return Response(data, status=status.HTTP_204_NO_CONTENT)
    ```


#### PUT   

- 게시글 수정   

  - view 함수에 PUT 메서드 추가   

    ```python
    @api_view(['GET','DELETE','PUT'])
    def article_detail(request, article_pk):
        article = get_object_or_404(Article, pk=article_pk)
        ....
        
        # 게시글 수정   
        elif request.method == 'PUT':
            serializer = ArticleSerializer(instance=article, data=request.data)
            if serializer.is_valid(raise_exception=True):
                serializer.save()
                return Response(serializer.data)
    ```

    - 장고처럼 게시글 생성과 굉장히 비슷( = instance 인자에 article 넣어주는 것)      



</br>  

