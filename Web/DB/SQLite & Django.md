#### Django Aggregation

[공식문서](https://docs.djangoproject.com/en/3.2/topics/db/aggregation/) 

- 무언가를 종합, 집합, 합계 등의 사전적 의미

- 특정 필드 전체의 합, 평균, 최대값 등을 구할 때 사용

  -> aggregate 하면 해당 값 하나만 딕셔너리에 들어가게 되어 잔고가 최대인 사람의 이름을 출력한다거나 등의 추가 작업은 불가

  ```sqlite
  User.objects.all().aggregate(Avg('age')) 
  : 전체의 평균 나이
  
  User.objects.filter(last_name='김').aggregate(Avg('age'))
  : 김씨 사람들의 평균 나이
  
  User.objects.all().aggregate(Max('balance'))
  = User.objects.aggregate(Max('balance'))
  : 계좌 잔고 중 가장 높은 값
  
  ```


#### SQLite Aggregate Functions

- COUNT

  - `SELECT COUNT(*) FROM users;` 

    uers 내의 값 총 개수 조회

- AVG, SUM, MIN, MAX

  - 기본적으로 해당 컬럼이 숫자일때만 사용 가능

    ```sqlite
    SELECT AVG(age)  FROM users WHERE age>=30;
    :나이가 30세 이상인 사람들의 평균 나이 조회
    
    SELECT first_name, MAX(balance) FROM users;
    :계좌 잔액(balance)이 가장 높은 사람의 이름과 금액 조회
    
    SELECT AVG(balance)  FROM users WHERE age>=30;
    :나이가 30세 이상인 사람들의 평균 계좌잔액 조회
    ```

    
  - annotate()

    - 주석을 달다라는 사전적 의미를 가진다.

    - 마치 컬럼 하나를 추가하는 것과 같으며, 특정 조건으로 계산된 값을 가진 컬럼을 하나 만들고 추가하는 개념

    - 원본 테이블이 변하는 것은 아니므로, 1회용 주석처럼 사용됨

      `annotate()` 에 대한 각 인자는 반환되는 쿼리셋의 각 객체에 추가될 주석

    ```sqlite
    '지역별 인원수'를 Django plus_shell로 작성하면
    
    User.objects.values('country').annotate(Count('country'))
    <QuerySet [{'country': '강원도', 'country__count': 14}, ... ,
               {'country': '충청북도', 'country__count': 14}]>
    
    User.objects.values('country').annotate(num_countries=Count('country'))
    <QuerySet [{'country': '강원도', 'num_countries': 14}, ... ,
               {'country': '충청북도', 'num_countries': 14}]>
    
    User.objects.values('country').annotate(Count('country'), avg_balance=Avg('balance'))
    <QuerySet [{'country': '강원도', 'country__count': 14, 'avg_balance': 157895.0} ,..., 
               {'country': '충청북도', 'country__count': 14, 'avg_balance': 159610.7142857143}]>
               
               
    -> '지역별 인원수'를 SQL 문으로 변환하면
    
    SELECT country, COUNT(country) FROM users_user GROUP BY country;
    ```

    

#### LIKE

- Like operator

  - 패턴 일치를 기반으로 데이터를 조회하는 방법

  - sqlite는 패턴 구성을 위한 2개의 wildcards 제공

    - `%` : 0개 이상의 문자
    - `_` : 임의의 단일 문자

    `SELECT * FROM 테이블 WHERE 칼럼 LIKE '와일드카드패턴';` 

    - 와일드카드패턴

      `2%` : 2로 시작하는 값

      `%2` : 2로 끝나는 값

      `%2%` : 2가 들어가는 값

      `_2%` : 아무값이 하나 있고 두번째가 2로 시작

      `1___` : 1로 시작하고 총 4자리인 값

      `2_%_%` / `2__%` : 2로 시작하고 최소 3자리

    - 예시

    `SELECT * FROM users WHERE age LIKE "2_";` 

    20대인 사람 조회

    `SELECT * FROM users WHERE phone LIKE "02-%";`

    지역번호가 02인 사람 조회

    `SELECT * FROM users WHERE first_name LIKE "%준";`

    이름이 '준'으로 끝나는 사람 조회

    `SELECT * FROM users WHERE phone LIKE "%-5114-%";`

    중간번호가 5114인 사람 조회

#### ORDER BY

- 조회 결과 집합 정렬

- SELECT 문에 추가하여 사용

- 정렬 순서를 위한 2개의 kewyword 제공 (ASC - 오름차순 / DESC - 내림차순)

- 아무것도 작성하지 않으면 기본 오름차순

  `SELECT * FROM 테이블 ORDER BY 컬럼 ASC;`

  `SELECT * FROM 테이블 ORDER BY 컬럼1,컬럼2 DESC;` : 컬럼1,컬럼2순으로 정렬

  ​	`SELECT * FROM users ORDER BY age ASC LIMIT 10;`

  ​	나이순으로 상위 10명 조회

  ​	`SELECT * FROM users ORDER BY age,last_name ASC LIMIT 10;`

  ​	나이순, 성순으로 상위 10명 조회

  ​	`SELECT last_name,first_name FROM users ORDER BY balance DESC LIMIT 10;`

  ​	계좌 잔액 순으로 내림차순 정렬하여 성,이름을 10명 조회

#### GROUP BY

- 행 집합에서 요약 행 집합 생성

- SELECT 문의 optional 절

- 선택된 행 그룹을 하나 이상의 열 값으로 요약 행 생성

- 문장에 WHERE 절이 있는 경우 **반드시** WHERE 절 뒤에 작성

  `SELECT 컬럼1, aggregate_function(컬럼2) FROM 테이블 GROUP BY 컬럼1, 컬럼2;` 

  ​	aggregate_function : 위에 작성한 COUNT,MAX 등등

  `SELECT last_name, COUNT(*) FROM users GROUP BY last_name;`

  각 성씨(last_name)가 몇명씩 있는지 조회

  ​	`SELECT last_name, COUNT(*) AS name_count FROM users GROUP BY last_name;`

  ​	-> AS를 활용하여 COUNT(*) 컬럼명 변경 가능

  
</br>   
  

### ALTER TABLE

1. table 이름 변경

   `ALTER TABLE 테이블명 RENAME TO 바꿀이름;` 

2. 테이블에 새로운 column 추가

   `ALTER TABLE 테이블명 ADD COLUMN 컬럼이름 데이터타입;` 

   ​	`ALTER TABLE news ADD COLUMN created_at TEXT NOT NULL;` 

   ​	-> 기존 레코드는 해당 칼럼에 대해 빈 칸이 되므로 **에러 발생!**

   ​	해결 방법 -> `NOT NULL 설정 없이 추가` or `DEFAULT 속성으로 기본값 설정` 

   ​	`ALTER TABLE news ADD COLUMN subtitle TEXT NOT NULL DEFAULT '소제목';` 

3. column 이름 수정 

</br>   

### SQL & ORM

- Django 프로젝트 내에서 `python manage.py sqlmigrate users 0001` 명령어 작성하면 

  SQL문 확인 가능

  ```sqlite
  BEGIN;
  --
  -- Create model User
  --
  CREATE TABLE "users_user" ("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, "first_name" varchar(10) NOT NULL, "last_name" varchar(10) NOT NULL, "age" integer NOT NULL, "country" varchar(10) NOT NULL, "phone" varchar(15) NOT NULL, "balance" integer NOT NULL);
  COMMIT;
  ```

- `sqlite3 db.sqlite3` 명령어로 SQLite 실행 가능 

- `.shell clear` or `.exit` 명령어로 sqlite 쉘 정리 및 종료 가능

- SQL vs ORM

  - CREATE

    SQL 생성

    ```sqlite
    INSERT INTO users_user VALUES (102, '길동','김',100,'경상북도','010-1234-1234',100);
    
    - 확인
    SELECT * FROM users_user LIMIT 2 OFFSET 100;
    ```

    ORM 생성 - 여러 방법중, 3번째 방법 사용

    ```sqlite
    User.objects.create(
       ...: first_name='길동',
       ...: last_name='홍',
       ...: age=100,
       ...: country='제주도',
       ...: phone='010-1234-5678',
       ...: balance=10000
       ...: )
    ```

  - READ

    SQL 조회 

    ```sqlite
    1. SELECT * FROM users_user;
    
    2. SELECT * FROM users_user WHERE rowid=101;
    
    = SELECT * FROM users_user LIMIT 1 OFFSET 100; 
    ```

    ORM 조회 

    ```sqlite
    1. User.objects.all()
    
    2. User.objects.get(id=102)
    
    = User.objects.get(pk=102)
    ```

  - UPDATE

    SQL 수정

    ```sqlite
    UPDATE users_user SET first_name = "ju", last_name ="an" WHERE rowid = 101;
    
    조회 -> 
    SELECT last_name, first_name FROM users_user WHERE id = 101;
    ```

    ORM 수정

    ```sqlite
    user = User.objects.get(id=102)
    user.first_name = 'juan'
    user.save()
    ```

  - DELETE

    SQL 삭제

    ```sqlite
    DELETE FROM users_user WHERE id=101;
    ```

    ORM 삭제

    ```sqlite
    1.
    user = User.objects.get(id=102)
    user.delete()
    
    2.
    User.objects.get(id=102).delete()
    ```



- 활용

  [참고1-make queries](https://docs.djangoproject.com/en/3.2/topics/db/queries/)   

  [참고2-QuerySet API](https://docs.djangoproject.com/en/3.2/ref/models/querysets/)  

  - **유저 수 조회**

    ORM

    ```sqlite
    User.objects.count() 
    : 전체 객체 수 조회
    
    User.objects.filter(age__gte=30).count()
    : 나이가 30살 이상인 사람 인원 수 조회
    
    User.objects.filter(age__lte=20).count()
    : 나이가 20살 이하인 사람 인원 수 조회
    
    User.objects.filter(age=30, last_name='김').count()
    = User.objects.filter(age=30).filter(last_name='김').count()
    : 나이가 30살이면서 성이 김씨인 사람 수 조회
    
    User.objects.filter(Q(age=30) | Q(last_name='김')).count()
    : 나이가 30살이거나 성이 김씨인 사람 수 조회
    
    User.objects.filter(phone__startswith='02-').count()
    : 지역번호가 02로 시작하는 사람 인원수 조회
    	끝나는 기준은 endswith
    	포함이면 contains
    
    ```

    - OR을 활용하고 싶다면, Q object 활용해야함 
      shell_plus 에선 자동으로 임포트되어있지만, 일반 shell에선 import 필요

      ```python
      from django.db.models import Avg, Case, Count, F, Max, Min, Prefetch, Q, Sum, When
      ```

    SQL

    ```sqlite
    SELECT COUNT(*) FROM users_user;
    : 전체 객체 수 조회
    
    SELECT COUNT(*) FROM users_user WHERE age>=30;
    : 나이가 30살 이상인 사람 인원 수 조회
    
    SELECT COUNT(*) FROM users_user WHERE age=30 AND last_name='김' ;
    : 나이가 30살이면서 성이 김씨인 사람 수 조회
    
    SELECT COUNT(*) FROM users_user WHERE age=30 OR last_name='김' ;
    : 나이가 30살이거나 성이 김씨인 사람 수 조회
    
    SELECT COUNT(*) FROM users_user WHERE phone Like '02-%' ;
    : 지역번호가 02로 시작하는 사람 인원수 조회
    ```

  - **조건 조회**

    ORM

    ```sqlite
    User.objects.filter(age__gte=30)  
    : 나이가 30살 이상인 사람 조회
    
    	gte = greater than or equal to
    	gt = greater than
    	lt / lte
    
    User.objects.filter(age=30).values('first_name') 
    : 나이가 30살인 사람들의 이름 조회
    
    print(User.objects.filter(age=30).values('first_name').query)
    : 해당 명령어에 대응하는 SQL문 출력
    -> SELECT "users_user"."first_name" FROM "users_user" WHERE "users_user"."age" = 30
    
    User.objects.filter(country='강원도',last_name='황').values('first_name')
    : 주소가 강원도이면서, 황씨인 사람의 이름
    
    User.objects.order_by('-age')[:10]
    : 나이순으로 상위 10명 조회
    
    User.objects.order_by('balance','-age')[:10]
    : 잔액이 적은순, 나이가 많은 순으로 상위 10명 조회
    
    User.objects.order_by('-last_name','-first_name')[4]
    : 성, 이름순으로 내림차순 정렬했을 때 5번째 사람 조회
    ```

    SQL

    ```sqlite
    SELECT first_name FROM users_user WHERE age = 30;
    : 나이가 30살인 사람들의 이름 조회
    
    SELECT * FROM users_user ORDER BY age DESC LIMIT 10;
    : 나이순으로 상위 10명 조회
    
    오름차순은
    SELECT * FROM users_user ORDER BY balance LIMIT 10;
    = SELECT * FROM users_user ORDER BY balance ASC LIMIT 10;
    
    SELECT * FROM users_user ORDER BY balance ASC, age DESC LIMIT 10;
    : 잔액이 적은순, 나이가 많은순으로 상위 10명 조회
    
    SELECT * FROM users_user ORDER BY last_name DESC, 
    first_name DESC LIMIT 1 OFFSET 4;
    : 성, 이름순으로 내림차순 정렬했을 때 5번째 사람 조회
    ```

    

