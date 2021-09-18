## Database

- 체계화된 데이터의 모임 = 여러 사람이 공유하고 사용할 목적으로 통합 관리되는 정보의 집합
- 몇 개의 자료파일을 조직적으로 통합하여 항목의 중복을 없애고, 구조화하여 기억시켜놓은 자료의 집합체  
- 구조화 검색과 생산의 효율화를 꾀한 것

- 장점
  - 데이터 중복 최소화
  - 데이터 무결성( 유효성 검증 )
  - 데이터 일관성
  - 물리적 / 논리적 독립성
  - 데이터 표준화
  - 데이터 보안 유지



### 관계형 데이터베이스 (RDB)

###### 키와 값들의 간단한 관계를 표형태로 정리한 데이터 베이스

- 관계형 모델에 기반
- 스키마 : DB에서 자료의 구조, 표현방법, 관계 등 전반적인 명세를 기술한 것     
- 테이블 : 열(칼럼/필드)과 행(로우/레코드)의 모델을 사용해 조직된 데이터 요소들의 집합   
- 열 : 각 열에는 고유한 데이터 형식이 지정     
- 행 : 실제 데이터가 저장되는 형태  
- 기본키 : 각 행의 고유 값으로 반드시 설정해야 하며, DB 관리 및 관계 설정시 주요하게 활용



### 관계형 데이터베이스 관리 시스템 (RDBMS)

###### 관계형 모델을 기반으로 하는 데이터베이스 관리 시스템 (ex. MYSQL, SQLite, ORACLE 등등)

- SQLite

  - 서버 형태가 아닌 파일 형식으로 응용 프로그램에 넣어 사용하는 **비교적 가벼운 DB** 
  - 구글 안드로이드 OS에 기본 탑재된 DB로 임베디드 소프트웨어에도 많이 활용
  - 로컬에서 간단한 DB 구성 가능
  - 오픈소스 프로젝트이므로 자유롭게 사용 가능
  - FAQ [공식문서](https://www.sqlite.org/faq.html#q2)  [번역문](https://runebook.dev/ko/docs/sqlite/faq)

- SQL

  - 관계형 DB 관리시스템의 데이터 관리를 위해 설계된 특수 목적의 프로그래밍 언어

  - DB 스키마 생성 및 수정  

  - 자료의 검색 및 관리 및  DB 객체 접근 조정 관리

  - 분류 (명령어)

    - DDL - 데이터 정의 언어
      - 관계형 데이터베이스 구조(테이블, 스키마) 정의   
      - 테이블 만든 후엔 사용 빈도 ↓
      - ex. CREATE, DROP, ALTER
    - DML - 데이터 조작 언어
      - 데이터를 저장, 조회, 수정, 삭제 등 수행
      - 가장 많이 사용
      - ex. INSERT, SELECT, UPDATE, DELETE  (순서대로 CRUD)
    - DCL - 데이터 제어 언어
      - 데이터베이스 사용자의 권한 제어
      - GRANT, REVOKE, COMMIT, ROLLBACK 

  - [구글 드라이브](https://abit.ly/ssafy-db) 

  - `sqlite3 db.sqlite3` 으로 db.sqlite3 데이터베이스 조회 가능

  - 명령어 앞에 `.` 이 붙는 명령어는 sqlite 프로그램의 기능을 실행하는 것

  - `.tables` 명령어로 테이블 확인 가능  / `.exit` 명령어로 나가기 가능 등

  - SELECT 문은 특정 테이블의 레코드(행) 반환   

  - 터미널 view 변경

    - `.headers on` 이후 SELECT 문은 헤더와 같이 나타남
    - `.mode column` 이후 SELECT 문은 헤더 정보가 행으로 나뉘어 나타남

  - CREATE

    - 테이블 생성

      `CREATE TABLE classmates (id INTEGER PRIMARY KEY, name TEXT);` 

      `CREATE TABLE classmates (name TEXT, age INTEGER, address TEXT);` 

      -> Primary Key 컬럼을 따로 작성하지 않으면, 값이 자동으로 증가하는 PK 옵션을 가진 rowid 컬럼이 정의됨

    - `.schema classmates` 명령어로 방금 전 생성한 classmates의 스키마 확인 가능    



### CRUD

- CREATE

  - INSERT

    - 특정 테이블에 레코드(행) 삽입

      `INSERT INTO 테이블이름 (컬럼1, 컬럼2, ...) VALUES (값1,값2,..);`

      `INSERT INTO classmates(name,age) VALUES('홍길동', 23);`  

      -> 자료를 공백으로 비우면 안되므로 `NOT NULL` 설정 필요!

      `INSERT INTO classmates VALUES('홍길동', 23, '서울');` 

      -> 모든 필드값을 다 삽입한다면, 필드명은 생략 가능  

    - 여러 값을 한 번에 넣고자 한다면, 튜플 형식으로 묶어서 가능

    ```sqlite
    INSERT INTO classmates VALUES
    ('홍길동', 30, '서울'),
    ('김철수', 30, '대전'),
    ('이싸피', 26,'광주'),
    ('박삼성',29,'구미'),
    ('최전자',28,'부산');
    ```

    

  - NOT NULL 설정

    ```sqlite
    CREATE TABLE classmates (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    age INT NOT NULL,
    address TEXT NOT NULL
    );
    ```

    - 다른 필드에는 `INT` / `INTEGER` 구분 상관없지만, PRIMARY KEY는 무조건 `INTEGER` 

      [참고](https://www.sqlitetutorial.net/sqlite-primary-key/)

    - NOT NULL 설정 이후엔 공백있으면 INSERT 불가

      `INSERT INTO classmates VALUES('홍길동', 30, '서울');`

      **Error: table classmates has 4 columns but 3 values were supplied** 

    - 하지만, id값 제외하고 명시적으로 컬럼값을 지정하면 INSERT 가능 

      `INSERT INTO classmates (name,age,address) VALUES('홍길동', 30, '서울');` 

    - Primary key는 유니크하므로 같은 번호에 다시 정보 입력 불가

      `Error: UNIQUE constraint failed: classmates.id` 



- READ

  - SELECT

    - 테이블에서 데이터 조회

      `SELECT 컬럼명 FROM 테이블이름;` 

      `SELECT * FROM classmates;` : 전체 조회

      `SELECT rowid, * FROM classmates;` : rowid 포함 조회

      `SELECT rowid,name FROM classmates;` : rowid, name 값만 조회

    - SELECT문은 가장 복잡한 문이며, 다양한 절(clause)과 함께 사용   

      - ORDER BY, DISTINCT, WHERE, LIMIT, GROUP BY ...

    - LIMIT

      - 쿼리에서 반환되는 행수 제한
      - 처음부터가 아닌, 특정 행부터 시작해서 조회하기 위해 **OFFSET** 키워드와 함께 사용하기도 함
      - 파이썬에서 리스트 슬라이싱과 비슷

      `SELECT rowid,name FROM classmates LiMIT 1;` : 상단 1개만 조회

      `SELECT rowid,name FROM classmates LiMIT 1 OFFSET 2;` : 3번째 값 1개만 조회

    - WHERE

      - 쿼리에서 반환된 행에 대한 특정 검색 조건 지정   

      `SELECT rowid,name FROM classmates WHERE address='서울';` 

    - SELECT DISTINCT

      - 조회 결과에서 중복 행 제거
      - DISTINCT 절은 SELECT 키워드 바로 뒤에 작성

      `SELECT DISTINCT age FROM classmates;` : age값 중복 없이 조회   



- UPDATE

  - 기존 행의 데이터 수정

  - SET 절(clause)로 각 열에 대해 새로운 값 설정 가능     

  - 조건을 통해 특정 레코드 수정하기 -> 중복 불가능한 값인 rowid 기준으로 수정

    `UPDATE 테이블이름 SET 컬럼1=값1, ... WHERE 조건;` 

    `UPDATE classmates SET name='홍길동', address='제주도' WHERE rowid=5;` 

    id가 5인 레코드의 이름과 주소 수정 



- DELETE

  - 테이블에서 행 제거

    `DELETE FROM 테이블이름 WHERE 조건;` 

    중복 불가능한 값인 rowid를 조건으로 삭제

    `DELETE FROM classmates WHERE rowid=5;` 

    -> 다시 INSERT 하면 해당 자료는 다시 id 번 값으로 할당됨

  - SQLite는 기본적으로 rowid 재사용

  - 재사용없이 다음 id 값을 사용하기 위해서 AUTOINCREMENT 속성 필요 (ex. Django pk)

    - AUTOINCREMENT

      - SQLite가 사용되지 않은 값이나 이전에 삭제된 행의 값 재사용 방지
      - 테이블 생성 단계에서 설정 가능

      `CREATE TABLE 테이블이름 (id INTEGER PRIMARY KEY AUTOINCREMENT, ... );` 

  - `DROP TABLE 테이블이름` : 테이블 자체 삭제
