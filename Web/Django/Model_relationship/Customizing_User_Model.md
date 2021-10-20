## Customizing authentication in Django   

### User model 대체   

- 일부 프로젝트에서는 장고의 내장 User 모델이 제공하는 인증 요구사항이 적절 X   

  - username 대신 email을 식별 토큰으로 사용하는 것이 더 적합한 사이트의 경우      

- 장고는 User를 참조하는데 사용하는 **AUTH_USER_MODEL** 값을 제공하여 디폴트 user model을 재정의할 수 있도록 함   

  - 확장성 증가      

- 장고는 새 프로젝트를 시작하는 경우 커스텀 유저 모델을 설정하는 것을 강력하게 권장   

  - 기본 내장 User 모델을 사용하더라도, 커스텀 유저 모델 설정하길 권장   

    - 대체(override)한다는 것이 중요한 것!  커스텀은 자유       

      -> 대체해두면, 추후 User 모델 변경 가능    

  - 모든 migrations 또는, 첫 migrate를 실행하기 전에 반드시 이 작업을 마쳐야 함    

-  **AUTH_USER_MODEL**

  - User를 나타내는데 사용하는 모델
  - models.py 에서만 사용 -> 그 외는 get_user_model()       
  - 프로젝트가 진행되는 동안은 변경 X     
    - 모델 관계에 영향을 미치니 중간 변경은 훨씬 더 어려운 작업 필요        
  - 프로젝트 시작시 설정하기 위한 것이며, 참조하는 모델은 첫 migration에서 사용할 수 있어야 함   
  - Default : `auth.User` (django.contrib.auth 앱의 User 클래스)      

</br>    

### Custom User 모델 정의   

1. 관리자 권한과 함께 완전한 기능을 갖춘 User 모델을 구현하는 기본 클래스인 AbstractUser를 상속받아 새로운 User 모델 작성   

   - 내장 User 모델도 AbstractUser 를 상속받아 작성된 모델   

   ```python
   # accounts/modles.py
   
   from django.contrib.auth.models import AbstractUser
   
   class User(AbstractUser):
       pass
   ```

   

2. **AUTH_USER_MODEL** 을 방금 작성한 User 모델을 사용하도록 변경   

   - settings.py 내의 `AUTH_USER_MODEL = 'accounts.User'`  작성    

3. admin site에 Custom User 모델 등록   

   ```python
   # accounts/admin.py
   
   from django.contrib.auth.admin import UserAdmin
   from .models import User
   
   admin.site.register(User, UserAdmin)
   ```

   

4. 프로젝트 중간에 진행하게 되면 DB를 모두 초기화한 후 마이그레이션 진행    

   - 현재는 DB를 초기화해도 아무 문제 없으므로 초기화로 진행   
   - 초기화 = `db.sqlite3`  + `migrations 폴더 내의 숫자 붙은 파일` 모두 삭제       

</br>   

### Built-in auth forms   

- Custom User model로 대체하면 몇가지 빌트인 auth form이 동작하지 않으므로 해결 필요       

  - UserCreationForm과 UserChangeForm은 기존 내장 User 모델을 사용하는 ModelForm 이므로 새로 설정한 Custom User Model로 대체 필요    

    -> Meta의 model 부분만 수정하면 됨    

    -> 만약 `model = get_user_model()` 처럼 함수로 작성했다면 대체할 필요 X      

  ```python
  class CustomUserCreationForm(UserCreationForm):
  
      class Meta:
          model = get_user_model()
          fields = UserCreationForm.Meta.fields + ('email',)
          # 원래 필드 + email   
  ```

- get_user_model()     
  - 현재 프로젝트에서 활성화된 사용자 모델 반환   
    - User 모델을 커스텀한 상황에선 Cutsom User 모델 반환   
  - 그러므로 장고에선 User 클래스를 직접 참조하는 것은 최대한 지양하고,    `django.contrib.auth.get_user_model()` 을 사용하여 참조해야 한다고 강조    

