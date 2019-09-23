# SQLite

Created: Sep 23, 2019 9:06 AM

---

# Django ORM & DB

- 지금까지의 장고 ORM
    - Django 안에 Persistant Data(영구 데이터)를 저장해서 DBMS로 관리한다는 것까지 배움. 그리고 Django가 제공하는 ORM을 통해 Python코드 `Post.objects.all()`만으로 Data에 접근해서 이것을 가져왔었다.
    - 장고 ORM이 DBMS라는 언어를 한번 더 번역해주는 것이었음. 우리는 지금 SQLite를 사용했는데 이것의 언어를 알아서 번역해줌
    - SQL lite는 RDBMS임 즉 엑셀 같은 관계형 데이터베이스. SQL이라는 것이 Oracle DB, MS SQL, MySQL, Postgress SQL 등에서 다 사용됨
- SQL: DB를 사용하기 위한 특수한 언어
    - 언어를 따로 배워야 하는 비용이 들기 때문에, 우선은 SQL 건너뛰고 ORM사용해서 Python으로만 사용을 해왔었음
    - 하지만 추후 어느 프로그래머가 되든 간에 데이터를 다뤄야 하므로 SQL을 기본으로 해야 함 (컴공 전공생은 한 학기동안 배우는데, 우리는 이틀 동안 배운다)
    - 해외의 데이터베이스 수업은 SQL 수업이 아니다
        - 버클리 대학에는 Oracle Berkeley DB가 있음. 버클리가 직접 DB를 "만든다". 즉 SQL Lite를 만드는 수업을 진행한다!!
        - Berkeley CS186 Database management systems 만드는 수업을 들으면 DB를 배울 수가 있음
        - 원래 이 정도 수업은 석사 가야 배울 수가 있음. DB를 build up하는 것을 학부부터 배우는 학교도 있다.
    - 스타트업에서는
        - low sql 안 쓰고 ORM만 사용하는 경우들도 많다.
        - 대부분의 회사에서는 아직 Low SQL을 백엔드와 함께 뽑는다. 그러니 한번 정도는 취업 전에 해봐야 함
        - 서버사이드 프로그래머 아니라도 쓰게 될 것

- django에서 SQL 살펴보기 `shell_plus`
    - qs = Post.objects.all() 이렇게 Queryset이라는 객체를 다 넣을 수가 있음. 이것은 리스트와 같아서 인덱스로도 조회가 가능하다.
    - 이 Queryset에 대해 어떤 기능 쓸 수 있는지 보면 굉장히 다양한 함수들 사용 가능(dir)
    - 기능 중에 query라는 함수가 있다.
    - `qs.query` 라고 찍어보면 django.db.models.sql.ueqry.Query라는 것이 뜨는데 이것을 print해보면! QL의 생 문자들이 나온다. SELECT 뭐뭐 나오고, FROM 나오고 ORDER BY 뭐라고 나온다. 이런 SQL 언어와 DB 조작법을 이틀 동안 볼 것이다.

# DB란

- DB?
    - DB를 쓰지 않더라도 영구데이터를 저장하기 위해 파일을 사용해왔음
        - csv, text 이것들을 통칭해서 '파일'이라고 한다.
    - 왜 DB를 굳이 쓰나?
        - **체계화된 데이터의 모임!!!**
        - 데이터 조작을 편리하게 하기 위해 만들어진 것
        - 데이터 중복 최소화, 무결성, 일관성, 독립성, 표준화, 보안 유지 등등 (이것들은 나중에 SQLD나 정보처리기사 딸 때 외우는 것임)
        - 핵심은? 편하게 데이터를 관리하기 위해서다

- RDBMS
    - DB가 없던 시절에 시스템이 구축된 곳들도 여전히 많다(부산은행, 정부데이터 등등)
    - DB라는 것 자체도 모던한 컨셉이고 아직 나온 지 얼마 되지 않긴 했다.
    - 여러 컨셉이 있었는데 1970년도 이후부터 관계형DB가 관리의 편의성, 간편함 등등으로 지배하게 됨
    - 간단하게는 엑셀 스프레드시트라고 생각하면 된다. 가로 세로 칼럼, 행으로 데이터 관리함
    - 관계를 표현한다는 것은 행이라는 무의미해보일 수 있는 데이터를 칼럼 단위로 쪼개서 어느 특정 의미를 갖게끔 만들어주는 것이다.
        - SQLite를 써왔고 이걸 통해서 SQL을 배워볼 것이다. 매우 빈번하게 쓰이는 DB 중 하나다. 가벼움 때문에 모던한 웹프레임워크나 모바일에서 주로 쓰인다.

- 스키마(Schema)
    - 장고에서 우리는 스키마를 models.py에서 정의해왔다.
    - 각 칼럼과 각 칼럼에 들어갈 데이터들을 정의했다.
    - 데이터베이스의 구조와 제약조건(자료의 구조, 표현방법, 관계)에
    - RDBMS는 모든 것을 표로 구현하게 되는데 이 구조를 보여주는 '메타데이터'가 스키마다.

- RDBS Table의 기본 구조
    - 관계형 DB에서 열, 칼럼에 고유한 데이터 형식이 저장된다.
    - 동의어: 열, 칼럼, 필드
    - 동의어: 행, 레코드
    - PK, 기본키: 데이터를 조회해서 번경시키는 역할을 위해 Primary Key라고 해서 각 행에 고유값을 부여하여 관리한다. 장고에서는 우리가 굳이 이것 지정을 안 해도 알아서 만들어준다.
    - 이렇게 생긴 게 기본적인 RDBMS Table의 구조다.
        - 각각의 시트가 Table이고
        - 이 Table들끼리 묶여 있는 게 DB다.

# SQL

## SQL?

- SQL (Structured Query Language)
    - 오늘 나오는 대부분의 것에서 SQL 문장 사용함
    - 이 SQL이라는 언어 자체가 너무 유명하고 널리 쓰여서~
    - 단순히 RDBMS뿐만 아니라 분산형 DB에서도 사용한다. (Hadoop이라고 하는 것 등)
    - 블록체인에서도 SQL이 쓰이려고 하고 있다.
    - 데이터를 다룰 때 표준이 되는 언어가 되었다!
- SQL 문법

    1. 데이터 정의 언어 (DDL)

    2. 데이터 조작 언어 (DML)

    3. 데이터 제어 언어 (DCL; 거의 안 할 것)

- SQL Keywords

    CRUD = Insert, Delete, Update, Select 사용한다.

    - 쿠팡 채용공고에서 이러한 SQL 문법 사용하기도 함

        SELECT Makers FROM *;

        개발자의 기본 소양이 SQL이다.

## SQL 설치하기

[https://www.sqlite.org/index.html](https://www.sqlite.org/index.html)

- 다운로드 받기
    - 다운로드 > 아래쪽 Precompiled Binaries for Windows에서 sqlite-dll-win64비트랑 sqlite-tools 32를 다운받는다.
    - 각각 압축을 푼다. 하나는 tools>sqlite3.exe가 들어 있고, 하나는 dll 내에 파일들 있음. installer가 이처럼 없을 수도 있다. 아래와 같이 손수 설치하면 된다.
- 우리가 원하는 폴더로 옮겨보자
    - 홈 화면에서 `mkdir sqlite`
    - 이 폴더 내에 아까 다운받은 파일들을 (폴더 말고 내부의 파일들) 가져온다. 총 5개 파일을 옮겨옴
- CLI에서 sqlite 쓰기
    - 깔끔하게 CLI에서 이 sqlite3 쓰고 싶다!
    - 환경변수에 저장하기 (우리가 노리는 이 파일의 경로를 따오자 우클릭 > 경로보면 `C:\Users\student\sqlite` 나와있음
    - CLI는 명령어 쳤을 때 환경변수 PATH에서 이 경로 안에 있는 프로그램인지를 찾는다. 그러니 여기에 Sqlite 경로를 넣어줘야 함
    - 내 PC > 속성> 고급시스템 > 환경변수 > Path 편집 > 새로만들기에서 이 경로 복붙하기
    - 터미널 껐다 켜보면 이제 sqlite3를 CLI가 인식할 수 있다.
- gitbash 설정 바꿔주기
    - gitbash는 알게 모르게 여러 프로그램을 사용한다.
        - cygwin이 gitbash가 돌아가고 있는 환경 중의 하나다.
        - winpty: window sw package라는 것도 사용 중
    - winpty 사용해야 이런 interactive shell을 활용 가능함
        - `winpty sqlite3` 라고 해야 sqlite3 명령어 쓰는 창 뜸
    - 앞으로 sqlite쓸 때마다 이렇게 쓰면 짜증난다. bashrc 편집하기
        - ~에서 `code ~` bashrc 파일을 연다.
        - `alias sqlite3='winpty sqlite3'` 이렇게 단축어 지정해주기. `=`하고 띄어쓰면 안 됨!! 그리고 잊지 말고 저장을 꼭 해준다
        - `source ~/.bashrc` 소스로 호출해준다~!
    - `sqlite --version`  버전 찍어봤을 때 3.29.0 나오면 정상

# DB Basic CRUD

## R: Table 보기

- 장고 프로젝트 내에서 db보기
    - 장고 프로젝트 폴더 CRUD_DB 내에 있는 db.sqlite3 파일을 보고 싶다면
    - `sqlite3 db.sqlite3`

- R: `SELECT`
    
- 표 읽기(보기)  `SELECT * FROM examples;`
    
- 테이블 보기
    - `.tables` 하면 이 db 내의 테이블들이 다 나온다.
    - SQL문은 반드시 `;`로 문장이 끝났다는 것을 알려줘야 한다.
    - sqlite> `SELECT * FROM posts_post;`
    - 한글 깨져서 나온다면: bash 우클릭 > Options > Text속성에서 C말고 ko-kr의 캐릭터셋이 euckr 말고 UTF-8유니코드로 바꿔주면 된다.
        - 기본적으로 db에 담을 때 유니코드로 많이 씀
        - 이 옵션 설정은 문자의 decoding style을 무엇으로 할지 지정해준 것이다.
    - sqlite> `SELECT title FROM posts_post;` 하면 제목만 쪼로록 나온다.

- 파일 가져오기
    - `mv ~/Downloads/users.csv .` 다운로드에 있는 csv파일을 현재 폴더에 옮기기
        - move 옮기자 이 경로의 파일을 . 현재 폴더로

- Database 생성하기
    - 아무것도 없는 상태에서 `sqlite3` 를 쳐서 쉘로 들어가서 `.databases` 데이터베이스 뭐 있나 보면 아무 것도 없다.
        - 이렇게 db가 없는 상태에서도 sqlite3를 통해 볼 수는 있다는 것을 알아두자.
    - `sqlite3 tutorial.sqlite3` 튜토리얼 db를 만들어준다.
- 튜토리얼 디비에 csv 넣기

    `.mode csv` 모드를 csv로 설정해서 csv파일 인식하게 해준다.

    `.import hellodb.csv examples` examples라는 table로 해서 이 csv 파일 내용 임포트

    `.tables` 보면 examples가 들어가 있게 된다.

- 조금 더 이쁘게 보자
    - 그냥 `SELECT * FROM examples;` 하면 내용만 못나게 보임
    - `.headers on` 헤더가 달린 표로 볼 수 있다.
    - `.mode column` 칼럼을 만들어서 예쁘게 잘라서 보기

## C: Table 생성하기

- CREATE

    ```sqlite
    CREATE TABLE classmates(
    	id INTEGER PRIMARY KEY,
      name TEXT
);
    ```
    
    - classmates라는 테이블을 만들 것인데, id필드는 integer로 primary key로써 사용하고, name은 Text로 할 것이다.
    - **콤마** 쓰는 것 잊지 말기!!
        - 콤마가 없으면 하나의 칼럼으로 인식하므로 망함
    - 여러 줄로 이렇게 명령문 쓰고 나면 ;로 닫아줘야 하나의 명령으로 인식함
- 필드의 속성
    - 대부분 integer, text만 사용한다.
    - 동적 데이터 타입!
        - 다른 db는 하나하나 다 지정해줘야 하는데 SQL은 엄청난 강점임
        - SQL은 숫자라고 해주면 알아서 맞춰주고, TEXT도 알아서 VARCHAR 이런 거 맞춰준다. 실수도 double이든 float든 real이든 다 유연하게 데이터를 저장해준다.
        - BLOB이라는 바이너리 파일은 DB마다 다르긴 하다.
- 스키마 확인하기

    `.schema classmates`

    - 이건 위에서 내가 필드 속성을 다 지정해줬는데...

    `.schema examples` 

    - 임포트해온 것은 DB의 필드 속성이 지정되어 있지 않고 그냥 TEXT로만 되어 있다 ㅠㅠ
    - 가장 심각한 것은 id가 TEXT로 들어가 있다 이건 망한 DB다...
    - csv를 임포트하고 싶다면: 우리가 schema를 먼저 지정하고 나서 csv를 임포트해야 필드속성 알아서 넣어준다!!!

## D: DROP

- 테이블 삭제하기

    `DROP TABLE examples;` 

    - 망한 DB인 examples를 DROP으로 지워보면 없어짐 (.tables 보면 사라져있음)

# Data CRUD

## U: 테이블에 데이터 조작하기

- 테이블에 데이터 추가, 읽기, 수정, 삭제

## C: Data 추가하기 `INSERT`

```sqlite
INSERT INTO 테이블명 (데이터 넣을 칼럼명1, 칼럼명2, ...) VALUES (데이터1, 데이터2, ...);
```

`INSERT INTO classmates (name, age) VALUES ('홍길동', 23);` 

- 스키마 단계에서 특정 필드가 비어 있으면 안 되게끔 지정할 수도 있다.

- 칼럼들 다 넣기 귀찮음
    - 모든 칼럼에 해당하는 값들 다 넣을 거라면 굳이 칼럼명 안 써줘도 된다.
    - 전체 칼럼에 다 넣는다면 VALUES만 넣어도 된다.

        `INSERT INTO classmates VALUES ('이삼성', 50, '수원');`

- 중복값 처리?
    - 지금은 db에 id가 없어서 중복값을 다 같은 거로 생각함
    - sQLite는 따로 primary key 칼럼 작성하지 않으면 없음
    - `SELECT rowid FROM classmates;`
        - rowid가 뭔가 나온다.
    - `SELECT rowid, * FROM classmates;` 보면 뭔가 rowid가 pk처럼 나오긴 함. pk처럼 다룰 수는 있지만 완전히 pk를 대체하는 것은 아니다.

- NULL값 처리?
    - 구멍이 숭숭난 데이터베이스 만들지 않기 위해서 schema를 타이트하게 짜줘야 한다.
    - 꼭 필요한 정보라면 공백으로 비워두면 안 된다. `NOT NULL`
    - 여태까지 만든 표를 지워보고 새 테이블 스키마 작성 잘 해서 만들어보자
        - integer로 된 Primary key를 id칼럼이라는 이름으로 만들자
            - PRIMARY KEY 띄어쓰기가 되어 있어야 한다!
            - Primary Key는 반!드!시! INTEGER여야 한다.
            - INTEGER와 INT는 별명으로 묶여 있어서 동일하긴 한데, PK는 INTEGER라고 해주자...
            
        - NOT NULL을 쓰자

        - `,` 쉼표 빼먹지 말자!!!
        
            ```sqlite
            sqlite> CREATE TABLE classmates (
               ...> id INTEGER PRIMARY KEY,
               ...> name TEXT NOT NULL,
           ...> age INT NOT NULL,
               ...> address TEXT NOT NULL
               ...> );
            ```
    
- 데이터 넣어보기
    - 이제 필수값을 안 넣어주면 에러 낸다.

        id값을 안 넣었더니 에러냄. 다른 칼럼들도 안 넣으면 넣으라고 에러 낸다. 값 자체가 안 들어간다.

        ```sqlite
        sqlite> INSERT INTO classmates VALUES('김사피', 30, '서울');
    Error: table classmates has 4 columns but 3 values were supplied
        ```

    - PK는 고유값이어야 한다.

        고유값이 아니면 에러를 낸다! 으아 귀찮아졌다. 
    
    ```sqlite
        sqlite> INSERT INTO classmates VALUES(1, '이삼성', 50, '수원');
    Error: UNIQUE constraint failed: classmates.id
        ```

    - 칼럼을 지정해서 넣어보자

        id 빼고 나머지 칼럼들을 지정한 후에 데이터를 넣어주니 된다!
    
        자동으로 id가 inclement된다!! 자동으로 id숫자가 올라간다.
    
        ```sqlite
        sqlite> INSERT INTO classmates (name, age, address) VALUES('이삼성', 50, '수원';
    sqlite> SELECT * from classmates;
        id          name        age         address
    ----------  ----------  ----------  ----------
        1           김사피         30          서울
    2           이삼성         50          수원
        ```
    
- not null constraint
    
        모든 칼럼값의 데이터 넣지 않으면 Not Null 제약에 걸려서 에러
    
        ```sqlite
        sqlite> INSERT INTO classmates (age, address) VALUES (15, '과천');
        Error: NOT NULL constraint failed: classmates.name
        ```
    
- PK 칼럼
    - 직접 pk 칼럼 내용 입력해주거나
    - pk외 다른 칼럼들 지정해서 데이터를 입력해줘서 자동생성시키거나
    - 장고의 pk 칼럼을 보자

        ```sqlite
        student@M701 MINGW64 ~/codes/django/BOARD (master)
        $ sqlite3 db.sqlite3
        SQLite version 3.29.0 2019-07-10 17:32:03
        Enter ".help" for usage hints.
        sqlite> .tables
        auth_group                  django_admin_log
        auth_group_permissions      django_content_type
        auth_permission             django_migrations
        auth_user                   django_session
        auth_user_groups            posts_post
        auth_user_user_permissions
        sqlite> .schema posts_post
        CREATE TABLE IF NOT EXISTS "posts_post" ("id" integer NOT NULL PRIMARY KEY AUTOINCR
        EMENT, "title" varchar(100) NOT NULL, "content" text NOT NULL, "image_url" varchar(
300) NOT NULL, "created_at" datetime NOT NULL, "updated_at" datetime NOT NULL);
        ```
        
        - rowid를 쓰는 게 아니라 id로 되어 있고 PRIMARY KEY로 지정되어 있다.
        - 모든 칼럼들이 NOT NULL로 되어 있다!
            - 장고가 NULL값을 싫어해서다.
            - 장고가 NULL임을 알 수 있도록 알아서 None이라는 것을 넣어놓는다.
            - ORM으로 blank=True를 하는 것 말고도 다른 처리를 할 수 있다.
        - NULL값은 정말 다루기 어렵다
            - 데이터 엔지니어 되면 NULL 다 한번 cleansing해주고 개고생한다.

## R: Data 읽어보기 `SELECT` , `LIMIT` , `WHERE`

- LIMIT

    ```sqlite
SELECT * FROM classmates LIMIT 2;
    ```
    
    - 표에서 상위 2개 항목만 보여줌
    - 데이터에서 딱 한 값만 지정하는 경우가 많으므로 `LIMIT 1`으로 지정해서 가져오는 일 많다!
- LIMIT 간격 지정하기
    - 띄엄띄엄 가져오기

        ```sqlite
        sqlite> SELECT * FROM classmates LIMIT 3 OFFSET 1;
        2           이삼성         50          수원
        3           오동잎         15          과천
        10          아몬드         10          제주
        sqlite> SELECT * FROM classmates;
        1           김사피         30          서울
        2           이삼성         50          수원
        3           오동잎         15          과천
    10          아몬드         10          제주
        ```
    
- 3개를 들고 올 건데, 1개를 띄우고 3개를 가져온다.
    
- Pagenation할 때 OFFSET을 활용한다.
    
        게시판에서 만개의 게시글이 다 보이는 게 아니라, 페이지마다 10개 20개 이렇게 짤려서 나온다. 1~20개까지 있다면 다음의 20개 이후 게시글 보고 싶을 때 `OFFSET 20` 해서 offset을 걸고 다음 다음 간격으로 넘어가서 본다.

        - OFFSET은 limit수 곱하기 1, limit수 곱하기 2... limit수 곱하기 Page넘버 별로 하면 된다.
        - Reversed라면 DB가 자체로 소팅을 하는 것으로 하면 됨
    
- WHERE (중요!!)
    - 특정 값인 것을 찾는다.
        
        - 가장 기본적인 DB검색 query는 WHERE를 기반으로 하게 된다.
- `Post.objects.get(pk=1)` 이런 식의 것을 sql문으로 하면
    
        sqlite> SELECT * FROM classmates WHERE id=1;
    1           김사피         30          서울
    
- 칼럼명으로 찾으면 된다.
    
        sqlite> SELECT * FROM classmates WHERE NAME='오동잎';
    3           오동잎         15          과천
    
    - 게시판에서 id로 찾기, 제목으로 찾기 이런 게 이 기능인 것!
    - 완전 데이터 내용이 동일해야만 찾는다.
- 서치하는 내용을 포함하는 것 모두 찾으려면? 다음에..
    
- 중복 없이

    테이블에서 특정 칼럼 값을 중

    - 우리 데이터 안에 몇 살들이 있는지 보고 싶다. 파이썬에서 set으로 만드는 것과 비슷하다고 생각하면 된다.

        ```sqlite
        sqlite> SELECT DISTINCT age FROM classmates;
        30
        50
        15
    10
        ```
    
    - 데이터 분석의 기초는 갖고 있는 것에서 내가 어떤 정보를 조회할 수 있는지가 기본이다.
        - 큰 데이터가 아니고서야 Linear Regression만 잘 하면 된다.
        - 너무 팬시하게 ML해야만 하는 것처럼 생각하지만 DB의 기초는 어떤 정보 가져와서 어떤 인사이트 도출해낼 수 있는지가 제일 중요하다.
        - SQL이 그 기본이다.

## D: 데이터 삭제하기 `DELETE`

- ORM에서도 pk 찾아서 그 후에 .delete() 했었다.
- 무얼 지울지 먼저 찾아줘야 하므로 `WHERE` 사용한다.

    ```sqlite
    sqlite> DELETE FROM classmates WHERE id=2;
    ```

- 고유값으로 지우는 게 최고임!!
- 중복값으로 지우게 된다면?

    ```sqlite
sqlite> DELETE FROM classmates WHERE age=27;
    ```
    
    - 해당 나이에 해당하는 사람이 다 날아간다.
- SQLite와 Django의 차이점
    - SQLite는 기본적으로 일부 행을 삭제하고 새 행을 삽입하면 삭제된 행의 값을 재사용하려고 시도한다.
        - primary key에 autoincrement가 안 들어가 있을 경우에...!
        - 왜냐면 Lite는 매우 작은 폰 같은 기기를 타겟으로 한 것이기 때문에 이 기능을 사용하지 않는 것을 권장하는 것임... 하지만 우리는 autoincrement 쓸 것이다.
    - Django는 삭제되어도 잔존id 남아있다.
    - 삭제된 데이터의 id를 재사용할 경우 문제점!
        - 1번 게시글에 여러 댓글들이 달려 있을 때, 1번 게시글을 삭제하면 CASCADE로 댓글들을 다 지웠는데 실제로는 다 안 지운다.
        - Foreign Key 칼럼(post_id)에 어떤 Default값으로 주고 댓글을 실제로 날리지는 않고 남겨둔다! 이런 것을 sort deletion이라고 한다. (데이터 분석을 위해 남겨두는 것임)
        - 그런데 이 때 1번 게시글이 생겨버리면 댓글이 거기로 붙어버림!
        - 만약... User table에 Password 테이블이 서로 연결되어 있다면 문제가 심각해짐
            - 내가 무슨 기계에서 접속했는지 session 테이블로 다 저장해두고...
            - 근데 이게 맘대로 연결되면 내가 남의 아이디로 들어가서 글 다 볼 수 있게 되는 것임
- PK칼럼에 반드시 `AUTOINCREMENT` 꼭 써주자!!
    - 이전에 썼던 pk를 사용하지 않고 그냥 AUTOINCREMENT시켜줘야 한다!

        CREATE TABLE tests (
        	id INTEGER PRIMARY KEY AUTOINCREMENT,
        	name TEXT NOT NULL);

    - autoincrement 연습

        ```sqlite
        sqlite> CREATE TABLE tests (
           ...> id INTEGER PRIMARY KEY AUTOINCREMENT,
           ...> name TEXT NOT NULL
           ...> );
        sqlite> .tables
        classmates  tests
        sqlite> .schema tests
        CREATE TABLE tests (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        name TEXT NOT NULL
        );
        sqlite> INSERT INTO tests (name) VALUES ('홍길동'), ('임꺽정');
        sqlite> SELECT * FROM tests;
        1           홍길동
        2           임꺽정
        sqlite> DELETE FROM tests WHERE id=2;
        sqlite> SELECT * FROM tests;
        1           홍길동
        sqlite> INSERT INTO tests (name) VALUES ('왕건이');
        sqlite> SELECT * FROM tests;
        1           홍길동
        3           왕건이
        ```

## U: 데이터 수정하기 `UPDATE`

- `UPDATE ~ SET ~ WHERE`

    ```sqlite
    sqlite> SELECT * FROM classmates;
    1           김사피         30          서울
    3           오동잎         15          과천
    10          아몬드         10          제주
    sqlite> UPDATE classmates SET name='곽곽이' WHERE id=10;
    sqlite> SELECT * FROM classmates;
    1           김사피         30          서울
    3           오동잎         15          과천
    10          곽곽이         10          제주
    ```

- update도 고유값을 기준으로 해야 한다.

    중복값인 나이를 기준으로 하면 다 바뀜

    ```sqlite
    1           김사피         30          서울
    3           오동잎         15          과천
    10          곽곽이         10          제주
    11          고니          28          남원
    12          용이          28          남원
    
    sqlite> UPDATE classmates SET name='아귀' WHERE age=28;
    
    sqlite> SELECT * FROM classmates;
    1           김사피         30          서울
    3           오동잎         15          과천
    10          곽곽이         10          제주
    11          아귀          28          남원
    12          아귀          28          남원
    ```

- 여러 어트리뷰트를 변경할 때는 SET뒤에 다 넣어주기만 하면 된다.

# csv파일

- 넣을 db 파일 열어주기

    ```bash
    student@M701 MINGW64 ~/codes/database
    $ sqlite3 tutorial.sqlite3
    ```

- csv 모드 열어서 임포트하기

    ```sqlite
    sqlite> .mode csv
    sqlite> .import users.csv users
    ```

- 그런데 이렇게 가져오면 스키마가 다 TEXT로 된 것이므로 스키마 지정한 후에 임포트 하는 것이 낫다.
    - phone은 하이픈 등 때문에 텍스트로 가져오는 것이 다루기 편하다.

        ```sqlite
        sqlite> CREATE TABLE users2 (
           ...> id INTEGER PRIMARY KEY AUTOINCREMENT,
           ...> first_name TEXT,
           ...> last_name TEXT,
           ...> age INTEGER,
           ...> country TEXT,
           ...> phone TEXT,
       ...> balance INTEGER);
        ```

    - 근데? 스키마 미스매치 에러 남

        에러는 났지만 표 자체는 잘 생성되긴 함
    
    ```sqlite
        sqlite> .import users.csv users2
    users.csv:1: INSERT failed: datatype mismatch
        ```
    
        왜냐면 이 csv파일은 헤더가 붙어있기 떄문에 텍스트에서 숫자로 converting이 안 된 것!!
    
    - 그래서 Header exclusion을 꼭 해줘야 한다. SQL lite는 경량형 DB라서 우리가 직접 해줘야 함. 다른 SQL에서는 이거 알아서 빼고 컨버팅 해주는데...
        
        - Header Exclusion 하기 나중에...

## R: 데이터 조회하기

- WHERE문 심화
    - users에서 age가 30 이상이고 성이 김인 사람의 성과 나이만 불러온다면?

        ```sqlite
    sqlite> SELECT last_name, age FROM users WHERE age>=30 and last_name='김';
        ```
    
- `COUNT` 데이터 개수 보기
    
- 카운트라는 함수를 써준다고 생각하면 된다.
    
        sqlite> SELECT COUNT(*) FROM users;
    1000
    
- `AVG` 평균 보기
    
- 특정 칼럼의 평균값을 알 수 있다.
    
        sqlite> SELECT AVG(age) FROM users;
    27.346
    
- 30세 이상인 사람들의 평균 나이 보기
    
        ```sqlite
    sqlite> SELECT AVG(age) FROM users WHERE age >= 30;
        35.1763285024155
        ```
    
- 최대, 최소값
    - 특정 칼럼의 최소, 최대값

        sqlite> SELECT MAX(age) FROM users;
        40
        sqlite> SELECT MIN(age) FROM users;
        15

    - 가장 계좌잔액이 높은 사람의 이름과 액수 가져오기

        ```sqlite
        sqlite> SELECT first_name, MAX(balance) FROM users;
    "선영",990000
        ```
    
- 패턴을 비교하는 법! `LIKE`
    - LIKE는 WHERE절의 핵심이다!
    - `_`  LIKE는 와일드카드를 써서 해당하는 것들의 자리만 정해준다
        
        - 반드시 해당 자리에 하나의 문자가 존재해야 한다.
    - `%` 해당 문자가 있을 수도 있고 없을 수도 있다고 지정함
    - `2%` 이로 시작함 2뒷부분은 있을 수도 있고 없을 수도 있다. 2, 23, 267...
    - `%2` 이로 끝남. 2, 12, 4212 ...
- 20대? `2_` 이렇게 찾으면 된다!! 2로 시작하고 뒤에 하나 더니까.
    
        ```sqlite
        sqlite> SELECT * FROM users WHERE age LIKE '2_' LIMIT 5;
        6,"서준","이",26,"충청북도",02-8601-7361,530
        9,"서현","김",23,"제주특별자치도",016-6839-1106,43000
        10,"서윤","오",22,"충청남도",011-9693-6452,49000
    12,"미정","류",22,"충청남도",016-4336-8736,52000
        15,"지원","박",24,"경상북도",02-3783-1183,35000
        ```
    
- 오름차순, 내림차순 정렬하기 `ORDER BY`
    - `ASC` 오름차순 (생략 가능하다!)
    - `DESC` 내림차순
    - 계좌잔액 순으로 내림차순 정렬하여 5명만 뽑아보기

        ```sqlite
        sqlite> SELECT * FROM users ORDER BY balance DESC LIMIT 5;
        627,"선영","김",37,"전라북도",02-1610-2940,990000
        124,"상현","나",30,"경상북도",010-4571-2869,99000
        235,"정호","이",20,"전라북도",011-1193-3920,99000
        259,"상철","이",17,"전라북도",011-3990-3978,99000
    500,"지아","최",16,"전라북도",02-4150-9018,9900
        ```

    - 정렬 기준을 2개 이상으로 사용해도 된다.
    
        ```sqlite
        sqlite> SELECT * FROM users ORDER BY age, last_name ASC LIMIT 5;
        295,"정수","강",15,"충청북도",02-7245-5623,500
        61,"우진","고",15,"경상북도",011-3124-1126,300
    998,"시우","고",15,"제주특별자치도",016-3732-8726,270
        791,"현숙","곽",15,"충청남도",016-7423-1481,710000
        11,"서영","김",15,"제주특별자치도",016-3046-9822,640000
    ```
    
- 여러 개의 order by를 사용할 경우에는 가장 왼쪽 칼럼부터 순차적으로 진행하기 때문에 순서를 고려해야 한다.
    - 여러 정렬 기준을 교차적으로 사용하기
    
        young and rich 뽑아보기
    
        ```sqlite
        sqlite> SELECT * FROM users ORDER BY balance DESC, age LIMIT 10;
        627,"선영","김",37,"전라북도",02-1610-2940,990000
        259,"상철","이",17,"전라북도",011-3990-3978,99000
        235,"정호","이",20,"전라북도",011-1193-3920,99000
        124,"상현","나",30,"경상북도",010-4571-2869,99000
        500,"지아","최",16,"전라북도",02-4150-9018,9900
    768,"준서","박",17,"전라남도",010-9213-9802,9900
        357,"은정","유",17,"강원도",016-8867-7897,980000
        751,"서윤","안",29,"경상남도",011-2018-8263,980000
        296,"미영","문",31,"전라남도",016-3620-8275,980000
        327,"하윤","고",32,"제주특별자치도",02-7876-4073,980000
        ```
    
    - 물론 지금은 칼럼이 INT가 아니라 TEXT로 되어 있어서 정렬이 제대로 되고 있지는 않다.

## U: DB 수정하기 +

- 이름 바꾸기: `ALTER`
    - 테이블 이름 바꾸기 `ALTER TABLE ~ RENAME TO`

        ```sqlite
    sqlite> ALTER TABLE users2 RENAME TO users;
        ```
    
- 칼럼 이름 바꾸기 `ALTER TABLE ~ ADD COLUMN`
        - tests라는 테이블에 created_at이라는 칼럼을 추가할 건데 여기에 데이터는 DATETIME형식으로 넣을 것이고 NULL허용 안 할 것이다.
    
        ```sqlite
            sqlite> ALTER TABLE tests ADD COLUMN created_at DATETIME NOT NULL;
        Error: Cannot add a NOT NULL column with default value NULL
            ```
    
        - 이미 갖고 있는 데이터들에는 NULL값이 들어가야 하므로 이걸 허용하지 않는다고 하면 에러남
        
            - 대책은 1) NOT NULL을 빼거나 혹은  2) Default값을 주면 된다.
        - Default value를 같이 주면 된다!
        
            ```sqlite
            sqlite> ALTER TABLE tests ADD COLUMN gender TEXT NOT NULL DEFAULT 'female';
            ```

