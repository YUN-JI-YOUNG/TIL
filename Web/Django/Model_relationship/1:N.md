## 1 : N 관계 설정      

User 모델 직접 참조는 권장하지 않으므로 `settings.AUTH_USER_MODEL` 사용    

- Article(1) : Comment(N)    

  - 위에서 한 것처럼 Comment 클래스에  `article = models.ForeignKey(Article, on_delete=models.CASCADE)` 로 Article - Comment 간의 모델 관계 정의(역참조)   

- User(1) : Article(N)    

  - User - Article 간 모델 관계 정의 (역참조)    

  ```python
  # article/modles.py
  
  from django.conf import settings
  
  class Article(models.Model):
      user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)   
      ...
      
  ```

  - 이후 migrate 하려고 하면, null 값이 허용되지 않는 user_id 필드의 디폴트값이 설정되지 않아서 설정하라고 함 -> 1로 설정 후 완료     
  - 게시글 작성 페이지에서 user 필드가 출력되는데, 이 게시글을 누가 작성한건지 설정할 수 있는 것은 어색하므로 ArticleForm의 fields 수정

  ```python
  # article/forms.py
  
  class ArticleForm(forms.ModelForm):
  
      class Meta:
          model = Article
          fields = ('title','content',)
  ```

  - 이후 게시글 작성하려고 하면, NOT NULL 필드인 article.user_id 누락으로 에러 발생   

    -> 작성자 정보가 누락되었기 때문에 views.py에서 작성자 정보 저장 필요   

    ```python
    # articles/views.py
    def create(request):
        ...
            if form.is_valid():
                article = form.save(commit=False)
                article.user = request.user
                article.save()
                return redirect('articles:detail', article.pk)
        ...
    
    ```

  - 추가적으로 자신이 작성한 게시글만 삭제 + 수정 가능하도록 설정 가능    

    ```python
    # 삭제
    
    def delete(request, pk):
        article = get_object_or_404(Article, pk=pk)
        if request.user.is_authenticated:
            if request.user == article.user:
                article.delete()
                return redirect('articles:index')
        return redirect('articles:detail', article.pk)
    # 로그인이 안되어있거나, 작성자가 일치하지 않으면 detail 페이지로 리다이렉트  
    ```

    ```python
    # 수정  
    
    def update(request, pk):
        article = get_object_or_404(Article, pk=pk)
        if request.user == article.user:
            if request.method == 'POST':
                ...
        else:
            return redirect('articles:index')
        context = {
            'article': article,
            'form': form,
        }
        return render(request, 'articles/update.html', context)
    ```

  - index 페이지에서 작성자 출력 가능   

    - `<p><b> 작성자 : {{article.user}} </b></p>`

  - detail 페이지에서 해당 게시글의 작성자가 아니라면 수정/삭제 버튼 출력 X    

    - ` {% if user == article.user %}  {% endif %}`   

  

- User(1) : Comment(N)    

  - User와 Comment 간의 모델 관계 정의 (역참조)   

  ```python
  # articles/models.py
  
  class Comment(models.Model):
      ...
      user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
      ...
      
  ```

  - User - Article 관계 정의할 때와 마찬가지로, migrate 할 때, user_id의 디폴트값을 1로 설정해주고, User 필드가 출력되는 것을 막기 위해 CommentForm 수정     

  - 작성자 정보 누락으로 발생하는 에러를 막기 위해 views.py 의 comments_create 함수에 

    `comment.user = request.user` 를 추가한 후 저장되게 수정   

  - 로그인한 사용자만 댓글 작성 폼이 보이도록 수정   

    ```html
    articles/detail.html 
    
    {% if request.user.is_authenticated %}
        <form action="{% url 'articles:comments_create' article.pk %}" method="POST">
          {% csrf_token %}
          {{comment_form}}
          <input type="submit">
        </form>
    {% else %}
      <a href="{% url 'accounts:login' %}">[댓글을 작성하려면 로그인하세요.]</a>
    {% endif %}
    ```

  - `{{comment.user}} - {{comment.content}}` 로 댓글 작성자 출력도 가능     

  - `{% if uesr == comment.user %}` 로 내가 작성한 댓글만 삭제 버튼 보이게 수정 가능   

  - 추가적으로, 실제로 view 함수를 수정하여 내가 작성한 댓글만 삭제할 수 있게 수정 가능   

    ```python
    # articles/views.py 
    
    def comments_delete(request,article_pk,comment_pk):
        ...
            if request.user == comment.user:
                comment.delete()
       	...
    ```

    
