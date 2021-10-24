## Profile Page   

- 자연스러운 follow 흐름을 위해 프로필 페이지 생성    

- 구현   

  - url 작성   

    `path('<username>/', views.profile, name='profile'),`   

    variable routing의 기본값은 str 이기때문에 `str:username` 이 아닌 `username`만 써도 ok      

  - views 함수 작성    

    ```python
    def profile(request,username):
        person = get_object_or_404(get_user_model(),username=username)
        context = {
            'person':person,
        }
        return render(request, 'accounts/profile.html', context)
    ```

  - profile 페이지 작성    

    ```html
    <h1>{{ person.username}} 의 프로필 페이지</h1>
    
      <hr>
    
      <h2>{{person.username }}가 작성한 게시글 </h2>
        {% for article in person.article_set.all %}
          <div>{{article.title}}</div>
        {% endfor %}
      <hr>
    
      <h2>{{person.username }}가 작성한 댓글</h2>
      {% for comment in person.comment_set.all %}
        <div>{{comment.content}}</div>
      {% endfor %}
    
      <hr>
    
      <h2>{{person.username}} 가 좋아요를 누른 게시글</h2>
      {% for article in  person.like_articles.all %}
        <div>{{article.title}}</div>
      {% endfor %}
      <a href="{% url 'articles:index' %}">[back]</a>
    
    ```

    base.html에 내프로필 추가   

    ```html
    <a href="{% url 'accounts:profile' user.username %}">내 프로필</a>
    ```

    index.html에 작성자 프로필로의 이동링크 추가    

    ```html
    <p>작성자 : <a href="{% url 'accounts:profile' article.user.username %}">{{ article.user }}</a></p>
    ```

</br>     

## Follow   

- User - User 간의 다대다(M:N) 관계

  - 하지만, 이 관계 형성을 위해 새로운 User 모델을 형성할 순 없으므로 자기자신과의 관계 형성 = 재귀 관계     

    ```python
    # accounts/models.py
    
    class User(AbstractUser):
        followings = models.ManyToManyField('self', symmetrical=False, related_name='followers')
    ```

  - url 작성    

    `path('<int:user_pk>/follow/', views.follow, name='follow'),`     

  - view 함수   

    ```python
    # accounts/views.py
    
    @ require_POST
    def follow(request,user_pk):
        if request.user.is_authenticated:
            person = get_object_or_404(get_user_model(), pk=user_pk)
     # 팔로우 하려는 사람과 당하는 사람이 다르다면, (= 나 자신은 팔로우할 수 없음)
            if request.user != person:
                # 내가 상대방(person)의 팔로워 목록에 있다면  
                if person.followers.filter(pk=request.user.pk).exists():
            #  if request.user in person.followers.all():
                    person.followers.remove(request.uesr)
                else:
                    person.followers.add(request.user)
            return redirect('accounts:profile', person.username)
        return redirect('accounts:login')
    ```

  - 프로필 페이지에 추가  (내 프로필 페이지에선 팔로우 버튼 안보이게)     

    ```html
    <div>
      <div>팔로잉 수 : {{person.followings.all | length}} / 팔로워 수 : {{person.followers.all | length}} </div>
    </div>
    {% if user != person %}
    <div>
      <form action="{% url 'accounts:follow' person.pk %}" method="POSt">
        {% csrf_token %}
        {% if user in person.followers.all %}
          <input type="submit" value='언팔로우'>
        {% else %}
          <input type="submit" value='팔로우'>
        {% endif %}
      </form>
    </div>
    {% endif %}
    ```

    - 반복되는 부분을 with 템플릿 구문으로 축약 가능  

    ```html
    {% with followings=person.followings.all fllowers=person.followers.all  %}
      <div>
        <div>팔로잉 수 : {{followings | length}} / 팔로워 수 : {{followers | length}} </div>
      </div>
      {% if user != person %}
      <div>
        <form action="{% url 'accounts:follow' person.pk %}" method="POST">
          {% csrf_token %}
          {% if user in followers %}
            <input type="submit" value='언팔로우'>
          {% else %}
            <input type="submit" value='팔로우'>
          {% endif %}
        </form>
      </div>
      {% endif %}
    {% endwith %}
    ```

   
