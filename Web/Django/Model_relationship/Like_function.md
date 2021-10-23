### Like (좋아요 기능)      

- 대표적인 다대다(M:N) 관계   

- User - Article 사이의 관계이므로 관계 설정   

  ```python
  # article/models.py
  
  class Article(models.Model):
      ...
      like_users = models.ManyToManyField(settings.AUTH_USER_MODEL)
      ...
  ```

  - 관계형 필드가 Article에 있으므로 `Article > User`는 참조, `User > Article`는 역참조        

    - 1:N 관계에서 사용하는 manager
      - article.user   (해당 게시글을 만든 유저)   
      - user.article_set   (내가 작성한 모든 게시글 - 역참조)    
    - M:N 관계에서 사용하는 manager   
      - article.like_users   (해당 게시글을 좋아요한 유저)    
      - user.article_set  -> user.like_articles   (내가 좋아요한 게시글 - 역참조)    

  - 이므로 `user.article_set` 역참조 이름 충돌로 에러 발생       

    -> realted_name 인자를 이용해서 M:N 관계 필드에 추가   

    `like_users = models.ManyToManyField(settings.AUTH_USER_MODEL, related_name='like_articles')`     

  - 중개 테이블 이름 규칙에 따라 `articles_article_like_users` 로 중개 테이블 생성      

- 구현   

  - url 작성   

    `path('<int:article_pk>/likes/', views.likes, name='likes'),`   

  - views 함수 작성    

    ```python
    @require_POST
    def likes(request,article_pk):
        if request.user.is_authenticated:
            article = get_object_or_404(Article, pk=article_pk)
            # 해당 게시글에 좋아요를 누른 유저 목록 안에 있다면
            if article.like_user.filter(pk=request.user.pk).exists():
      #     if request.user in article.like_users.all():
                # 좋아요 취소
                article.like_users.remove(request.user)
            else:
                # 좋아요 누르기
                article.like_users.add(request.user)
            return redirect('articles:index')
        return redirect('accounts:login')
    ```

    - exists()   
      - QuerySet에 결과가 포함되어 있으면 True, 아니면 False 반환   
      - 규모가 큰 QuesySet의 컨텍스트에서 검색할땐 `in` 보다 빠름   
      - 고유한 필드가 있는 모델이 QuerySet의 구성원인지 여부를 찾는 가장 효율적인 방법    

  - index에 like 출력    

    ```html
    <div>
        <form action="{% url 'articles:likes' article.pk %}" method="POST">
          {% csrf_token %}
          {% if user in article.like_users.all  %}
            <input type="submit" value='좋아요 취소'>
          {% else %}
            <input type="submit" value='좋아요'>
          {% endif %}
        </form>
    </div>
    ```

    
