### Handling HTTP requests

HTTP 요청을 처리하는 방법   

1. Django shortcut functions   

   - `django.shortcuts` 패키지는 개발에 도움될 수 있는 여러 함수, 클래스 제공   

   - `render()`, `redirect()`, `get_object_or_404()`, `get_list_or_404()`

     - get_object_or_404()

       - 모델 manager objects에서 get()을 호출할 때, 해당 객체가 없을 경우 `DoesNotExist 예외` 대신 Http 404를 raise      

       ```python
       try:
           question = Question.objects.get(pk=pk)
       except Question.DoesNotExist:
           from django.http.response import Http404
           raise Http404(f'No question with id {question_pk}')
           
       # 원래는 이 코드를 위처럼 pk로 찾는 코드가 있을 때마다 사용해서 검증했어야 하지만, 중복이므로 함수화를 하여 get_object_or_404() 사용
       ```

       

       - 원래라면, 코드 실행 단계에서 발생한 에러에 대해 브라우저는 http 상태코드를 500으로 인식함 -> 500은 그냥 서버가 모르는 상황이라는 뜻이므로 해당 코드를 받으면  뭐가 틀렸는지 알기 힘듦          

     - get_list_or_404()

       - 조회하고자 하는 리스트가 비어있을 때, http 상태코드 404 반환   

2. View decorators

   - 다양한 HTTP 기능을 지원하기 위해 view에 적용할 수 있는 여러 데코레이터 제공     

   - view 함수를 좀 더 단단하게 만들 수 있음    

   - HTTP 요청에 따라 적절한 예외처리, 데코레이터를 통해 서버를 보호하고 클라이언트에게 정확한 상황(http 상태 코드)을 제공하는 것의 중요성 강조! 

     - 데코레이터 : 함수에 기능을 추가할 때, 그 함수를 수정하지 않고 기능을 연장해주는 함수    

     - Allowed HTTP methods

       - 요청 메소드에 따라 view 함수에 대한 액세스 제한   

       - 요청이 조건을 충족하지 못하면 HttpResponseNotAllowed 리턴 (405 Method Not Allowed)

       - `require_http_methods()`, `require_POST()`, `require_safe()`, `require_GET()- 장고에서 사용하지 않길 권장, 대신 require_safe()` 

       - require_http_methods

         - `@require_http_methods(['GET','POST'])` 처럼 여러 메소드 허용할 때 사용    
           - create, update 처럼 GET과 POST 두개만 받아야할 경우   

         ```python
         @require_safe = @require_http_methods(['GET','HEAD'])
         @require_POST = @require_http_methods(['POST'])
         ```

       - require_safe()

         - require_GET() 대신에 require_safe() 사용하는 것이 더 좋다
         - detail, index처럼 GET 메소드만 받는 경우엔 require_safe() 사용
