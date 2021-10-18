## Foreign key

- 외래 키

- 관계형 데이터베이스에서 한 테이블의 필드 중 다른 테이블의 행을 식별할 수 있는 키   

  = 참조하는 테이블에서 1개의 키에 해당하고, 이는 참조되는 측 테이블의 기본 키를 가리킴   

  = 참조하는 테이블의 행 1개의 값 = 참조되는 측 테이블의 행 값   

- 참조하는 테이블의 행 여러개가 참조되는 테이블의 동일한 행을 참조할 수 있음    

- 게시글과 댓글간의 모델 관계 설정   

  - 댓글(참조하는 측)의 foreign key = 게시글(참조되는 측)의 id       

- 특징    

  - 키를 사용하여 부모 테이블의 유일한 값을 참조 (참조 무결성)     
    - 참조 무결성   
      - 데이터베이스 관계 모델에서 관련된 2개 테이블간의 일관성      
      - 외래키 속성값은 부모 테이블의 기본키값으로 존재해야 함      
  - 외래 키의 값이 반드시 부모 테이블의 기본 키일 필요는 없지만, 유일한 값이어야 함      
   
</br>    

### ForeignKey Field     

  `article = models.ForeignKey(Article, on_delete=models.CASCADE)`     

  -> 명시적 모델 관계파악을 위해 클래스의 소문자 단수형으로 작성하는 것이 바람직        

  - 1:N 관계에서 사용   

  - 2개의 위치 인자가 반드시 필요   

    - 참조하는 model class   

    - on_delete 옵션    

      - 외래키가 참조하는 객체가 사라졌을 때, 외래 키를 가진 객체 처리 방법 정의    

        ex> 게시글에 댓글이 달렸는데 해당 게시글이 삭제되었을 경우 댓글 처리 방법      

      - `CASCADE` : 부모 객체가 삭제되면 이를 참조하는 객체도 삭제(기본값)    

      - 데이터 무결성을 위해 매우 중요한 설정   

        - 데이터의 정확성과 일관성을 유지하고 보증하는 것을 가리키는 것    
        - 개체 무결성   
          - pk의 개념과 관련   
        - 참조 무결성   
          - FK(외래키)의 개념과 관련    
        - 범위 무결성    
          - 정의된 형식(범위)에서 관계형 DB의 모든 컬럼이 선언되도록 규정    

  - 대댓글의 경우 -> 재귀 관계 (자신과 1 : N)     

    참조하는 model class = 'self'     
   
</br>    

### 1:N 관계 매니저(related manager)   

  Article(1) : Comment(N)

  - 역참조(`'comment_set'`)       

    - Article(1) >  Comment(N)     

    - Article 모델에는 변화가 있는 것은 아니므로 `article.comment` 처럼 사용은 불가능하고, `article.comment_set` manager가 생성됨      

      `article.comment_set.all()` 처럼 `objects` 가 아닌 새로운 manager 생성      

    - 게시글에 몇개의 댓글이 작성되었는지 Django ORM이 보장할 수 없음      

      - article은 comment가 있을 수도 있고 없을 수도 있음   

    - `related_name`     

      - 역참조시 사용할 이름(`comment_set` manager) 변경 옵션    

        `article = models.ForeignKey(Article, on_delete=models.CASCADE, related_name='comment')`  로 설정하면, `article.comment` 사용 가능        

  - 참조(`'article'`)      

    - Comment(N) > Article(1)                
    - 댓글의 경우 어떠한 댓글이든 반드시 참조하고 있는 게시글이 있으므로 `comment.article`  과 같이 접근 가능      
    - 실제 ForeignKeyField(`article`) 또한 Comment 클래스에서 작성        

   
</br>    



### 댓글 CRUD     

- 댓글 update(수정) 부분은 자바스크립트 사용 필요 -> 현재 X     

#### CREATE

- `save(commit=False)`    

  - DB에 저장하지 않은 인스턴스 반환   
  - 저장하기 전 객체에 대한 사용자 지정처리를 수행할 때 유용하게 사용   

  ```python
  def comments_create(request,pk):
      article = get_object_or_404(Article, pk=pk)
      comment_form = CommentForm(request.POST)
      if comment_form.is_valid():
          comment = comment_form.save(commit=False)     
          comment.article = article
          comment.save()
      return redirect('articles:detail', article.pk)
  ```



#### READ    

- 역참조 실행  

  ```python
  # views.detail
  comments= article.comment_set.all() 추가
  
  # templates
  <h4>댓글 목록</h4>
    <ul>
      {% for comment in comments %}
        <li>{{comment.content}}</li>
      {% endfor %}
    </ul>
  ```

  

#### DELETE

- 명시적 url 작성   

  ```python
  # views
  def comments_delete(request,article_pk,comment_pk):
      comment = get_object_or_404(Comment,pk=comment_pk)
      comment.delete() 
      return redirect('articles:detail', article_pk)  
  
  # urls
  path('<int:article_pk>/comments/<int:comment_pk>/delete/', views.comments_delete, name='comments_delete'),
  
  # variable routing 2번 사용 가능  
  ```

  
