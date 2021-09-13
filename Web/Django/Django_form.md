## Django Form Class

- HTML form, input 태그 등을 통해서 사용자로부터 데이터를 받음    
- 입력된 데이터의 유효성을 검증하고, 필요시에 입력된 데이터를 검증 결과와 함께 다시 표시하며,이러한 데이터에 대해 요구되는 다른 동작을 수행하는 것은  꽤나 많은 노력이 필요한 작업       
- Django는 일부 과중 작업, 반복 코드를 줄여줌으로써, 이 작업을 훨씬 쉽게 만듦    

### Django's Form

- Form은 장고의 주요 유효성 검사 도구들 중 하나    

- 공격, 우연한 데이터 손상 등에 대한 중요한 방어 수단    

- Django는 위와 같은 form 기능의 방대한 부분을 단순화하고 자동화할 수 있으며, 우리가 직접 작성한 코드보다 안전하게 수행 가능    

- Django가 제공하는 Form에 관련된 작업     

  1. 렌더링을 위한 데이터 준비 및 재구성  
  2. 데이터에 대한 HTML forms 생성    
  3. 클라이언트로부터 받은 데이터 수신 및 처리       

- Form 과 ModelForm 

  각각의 역할이 다를 뿐, 하나가 다른 하나보다 좋다든가 나쁘다든가는 없음

  - Form

    - 어떤 모델에 어떤 값을 저장해야 하는지 알 수 없으므로 유효성 검사 이후 딕셔너리 생성   

      -> 딕셔너리에서 일일이 데이터를 가져온 후, save() 호출 

      즉, model에 연관되지 않은 데이터 받을 때 사용, 

      = model에 저장하지 않고 데이터 그 자체로 처리할 수 있는 것들     

  - ModelForm

    - 이미 django가 해당 model에서 양식에 필요한 대부분의 정보를 이미 정의   

    - model 기반이므로 어떤 레코드를 만들어야할지 알고 있어 바로 save() 호출    

      즉, model에 연관되어 있는 데이터 받을 때 사용   

### Form class

- Django Form 관리 시스템의 핵심   

- form 내 field, field 배치, 디스플레이(표현) widget, label, 초기값, 유효하지 않은 필드에 관련된 에러 메시지까지도 결정   

- Django는 데이터를 받을 때 해야할 과중작업(데이터 유효성 검증, 데이터 검증 결과 재출력 및 이러한 데이터에 대해 요구되는 동작 수행 등), 반복 코드를 줄여준다.    

- 생성 과정

  - 해당 앱 내에 forms.py  파일 생성

  - Form 선언  (`class ArticleForm(forms.Form):`)

    - Model을 선언하는 것과 유사하게 class로 선언하며, 같은 필드타입 사용    

      -> but, TextField는 존재하지 않음

    - forms 라이브러리에서 파생된 Form 클래스를 상속받음     

  - input과 label로 받던 입력을 Django Form Class 로 대체    

    - 사용자입력을 받는 view 함수 내부에, form = ArticleForm() 처럼 인스턴스 변수 생성 후 context를 이용해서 form 을 랜더링할 때 같이 보내기
    - html 문서에서 submit만 남기고 input과 label 부분을 지우고 {{ form }} 으로 대체    
    - `{{ form.as_p }}` : 각각의 필드들을 <p> 태그로 감싸서 렌더링
    - `{{ form.as_ul }}` : 각각의 필드들을 <li> 태그로 감싸서 렌더링
      - ul 태그는 직접 작성해야 함
    - `{{ form.as_table }}` : 각각의 필드들을 <tr> 태그(행)로 감싸서 렌더링    
      - table 태그는 직접 작성해야 함    

- Django의 HTML input 요소 표현 방법    

  - form fields

    - input에 대한 유효성 검사를 처리하며, 템플릿에 직접 사용     

  - Widgets

    - Django Form 으로는 input 태그중 `textarea` 등 은 표현할 수 없어서 Widgets의 힘을 빌려야함

    - 웹페이지의 HTML input 요소를 렌더링하며, GET/POST 딕셔너리에서 데이터 추출 가능     

    - widgets은 단독으로 사용하지 않고, **반드시** form fields에 할당

      - Form을 class로 선언할 때, 필드 인자로 사용  

        ex> `forms.CharField(widget=forms.Textarea)` 

    - input 요소의 단순 raw한 렌더링 처리 담당   

      - 출력 방식 결정(select 나 radio 버튼 등도 모두 위젯으로 설정)

### ModelForm

- 이미 모델에서 필드를 정의했는데, form에서 필드를 재정의하는 중복 행위 발생 

  -> Django는 Model을 통해 Form Class를 만들 수 있는 헬퍼 제공     

- Model을 기반으로 Form class를 만들 수 있으며, 일반 Form처럼 객체를 생성하여 view에서 사용 가능    

- 생성 과정

  - 해당 앱 내에 forms.py  파일 생성

  - ModelForm 선언 (`class ArticleForm(forms.ModelForm):`)

    - forms 라이브러리에서 파생된 ModelForm 클래스 상속받음   

  - Meta Class 선언

    - 정의한 클래스 안에 메타 클래스를 선언하고, 어떤 모델을 기반으로 form을 작성할 것인지에 대한 정보를 지정   

    - 기반이 되는 model을 불러오기 위해 import (`from .models import Article` )

      ```python
      class ArticleForm(forms.ModelForm):
                                  # 필드 재정의 X
      class Meta:
          model = Article
         # fields = ('title', 'content', )
          fields = '__all__'      # -> 필드값이 많을 경우 대비  
         # exclude = ('title',)  -> 특정 필드값을 제외하고 싶을 경우 사용
          
      # 클래스 안의 클래스 = Inner class(Nested class)
      # 관련 클래스(같은 기능을 하는 클래스)끼리 그룹화 
      # -> 가독성 및 프로그램 유지 관리 지원 
      
      # Meta 데이터 : 데이터에 대한 데이터 
      # (사진 데이터에 대한 메타데이터(촬영 시각, 렌즈, 조리개 값 등))
      ```

    - Meta class

      - 모델의 정보를 작성하는 곳   
      - ModelForm은 기반으로 하는 Model의 정보가 필요한데, 그 정보를 Meta class가 구성하도록 설계됨 -> 해당 Model에 정의한 field 정보를 form에 적용하기 위함     
      - fields 는 보통 리스트나 튜플 형태로 작성, but, 필드값이 많으면 `__all__` 사용    
        - 전체를 출력하는 all 이라고 할지라도, 사용자 입력값이 아닌 DB에서 입력하는 (작성일, 수정일 등)은 출력되지 않음     
      - widgets 로 스타일 적용   

      ```python
      # 1.
      #class ArticleForm(forms.ModelForm):
      #                            
      #class Meta:
      #    model = Article
      #    fields = '__all__' 
      #    widgets= {
      #        'title': forms.TextInput(attrs={
      #            'class' : 'title',
      #            'placeholder':'Enter the title',
      #            'maxlength': 10,
      #        }
      #      )
      #    }
          
      # 2. (권장)
      class ArticleForm(forms.ModelForm):
          title = forms.CharField(
          	label='제목',
              widget=forms.TextInput(
              	attrs={
                      'class' : 'my-title',
                      'placeholder':'Enter the title',
                      'maxlength':10,		# 위젯 선언시 max_len 속성 등 사라짐
                  }
              ),
          )
                                  
      class Meta:
          model = Article
          fields = '__all__' 
      ```

      

- 유효성 검사    

  요청한 데이터가 특정 조건에 충족하는지 여부 확인하는 작업,    

  DB 각 필드 조건에 맞지 않는 데이터가 서버로 전송되거나, 저장되는 행위를 막는 것

  - `is_valid` 메소드 사용   
    - 유효성 검사 실행 -> boolean 반환   
    - 데이터 유효성 검사를 보장하기 위한 많은 테스트에 대해 django가 is_valid() 제공

  ```python
  def create(request):
      form = ArticleForm(request.POST)       
   # 폼에 입력값을 통째로 넣는다 / 원래는 필드값을 각각 입력해야 했음
      # 유효성 검사
      if form.is_valid():         # 유효성 검사를 통과한다면
          article = form.save()
          return redirect('articles:detail', article.pk)
      return redirect('articles:new')
  ```

  

    - `save()` 메소드    

      - 이전에 사용한 save() 메소드와는 다른, form 인스턴스에서 사용하는 메소드

      - form에 바인딩(연결)된 데이터에서 DB 객체를 만들고 DB에 저장   

      - ModelForm의 하위 클래스는 기존 모델 인스턴스를 키워드 인자 instance로 작성가능

        - instance 인자가 제공되면, update(덮어쓰기)    
        - instance 인자가 없으면, create (새로쓰기) 

        ```python
        # update
        article = Article.objects.get(pk=pk)
        form = ArticleForm(request.POST, instance=article)
        form.save()
        
        # create
        form = ArticleForm(request.POST)
        form.save()
        ```

        

- 추가 검증

  - 유효성 검증이 완료된 후, DB 저장 없이 시행되는 추가 검증
  - db엔 저장하지 않지만, 검증을 한번 더 할 추가필드를 만들고 싶다면, model이 아닌 form에 추가 작성하면 된다

  ```python
  class QuestionForm(forms.ModelForm):
      ...
      is_save = forms.BooleanField(required=False, label='wanna save?', help_text='저장하려면 체크하세요')
      ...
      class Meta:
          ...
          
  # HTML에 출력 + 데이터 검증은 하되 DB 저장은 하지 않는다
  # views 함수에서 유효성 검증이 되면, if 분기를 한번 더 넣음으로써 검증 가능
  # if form.cleaned_data['is_save']: 
  ```

  

