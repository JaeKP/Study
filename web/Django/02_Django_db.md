# Django frame work_DB

> url이 어떤 요청인지 식별해서 그에 맞는 응답을 보내준다. (view함수에서 이를 설정 한다. )
>
> db에서 읽어온 데이터를 Template과 합쳐서 렌더링한다.

<br>

[toc]

<br>

## 1. Model

>  웹 애플리케이션의 데이터를 구조화하고 조작하기 위한 도구
>
> model.py에서는 클래스를  활용한다.  

<br>

- 단일한 데이터에 대한 정보를 가진다. (통일된, 구조화된)
  - 사용자가 저장하는 데이터들의 필수적인 필드(각각의 데이터)들과 동작(데이터를 처리하는 동작)들을 포함한다.
  -  필드 예시) 이름, 나이, 주소 등 
- 저장된 데이터 베이스의 구조 (lay-out)
- django는 model을 통해 데이터에 접속하고 관리한다.
- **일반적으로 각각의 model은 하나의 데이터베이스 테이블에 매핑된다.** 

<br>

### 1) Database

- 데이터 베이스(DB)
  - **체계화된** 데이터들의 모임 (규격에 맞게, 통일된 형태로)
- 쿼리 (Query)
  - 데이터를 조회하기 위한 명령어
  - 조건에 맞는 데이터를 추출하거나 조작하는 명령어
  - "Query를 날린다" => DB를 조작한다. (DB에 명령!)

<br>

### 2) Database의 기본 구조

#### (1) 스키마 (Schema)

> 데이터 베이스의 구조와 제약 조건 (자료의 구조, 표현방법, 관계)에 관련한 전반적인 명세를 기술한 것이다. 

- 데이터베이스에서 자료의 구조, 표현방법, 관계 등을 정의한 구조 (structure)
- 어떻게 데이터를 구조화해서 저장할 것인가에 대한 전반적인 명세를 기술한 것

`예시` 자료 구조: list형태?, dictionary 형태?등  / 표현방법:  정수?, 문자열? 날짜?등

| column | datatype |
| ------ | -------- |
| id     | INT      |
| age    | INT      |
| phone  | TEXT     |
| email  | TEXT     |

<br>

#### (2) 테이블 (Table) 

> 열(컬럼/필드)과 행(레코드/값)의 모델을 사용해 조직된 데이터 요소들의 집합이다
>
> SQL 데이터베이스 에서는 테이블을 관계라고도 한다. 

- 열(column): 필드(field) or 속성
  - 각 열에는 고유한 데이터 형식이 지정된다. ex)INTEGER, TEXT, NULL 등
- 행(row): 레코드(recoed) or 튜플
  - 테이블의 데이터는 행에 저장된다. 
  - 예시의 경우 `usertable`에 3명의 고객정보가 저장되어 있으며, 행은 3개가 존재한다. 
  - 데이터는 행 단위로 관리한다. (생성 및 삭제) 
  - 읽어오거나 수정하는 것은 일부만 뽑아 올 수 있다.  

`예시`

|      |      A      |  B   |  C   |       D       |        E         |
| :--: | :---------: | :--: | :--: | :-----------: | :--------------: |
|  1   | **id (PK)** | name | age  |     phone     |      email       |
|  2   |    **1**    | hong |  42  | 010-1234-1234 |  hong@gmail.com  |
|  3   |    **2**    | kim  |  16  | 010-1234-5678 |  kim@naver.com   |
|  4   |    **3**    | kang |  29  | 010-1111-2222 | kang@hanmail.net |

>  PK(기본키): 각 행(레코드)의 고유값으로 Primary Key로 불린다. 절대 중복되지 않는 값으로서 데이터를 식별하기 위한 유니크한 값이다. 
>
> 반드시 설정하여야 하며, 데이터 베이스 관리 및 관계 설정시 주요하게 활용된다.  

<br>

## 2. ORM

>  db와 통신을 하기위해서는 일반적으로 SQL을 사용한다.
>
> 그런데 SQL을 몰라도 서로 통신을 하기위해 도움을 주는 ORM이라는 시스템이 존재한다.

### 1) ORM이란

**DB** <=============================================> **Python 코드** 

**DB** <=== SQL satement===> **ORM** <====Python Object ====> **Python 코드** 

<br>

- object-Relational-Mapping
- 객체 지향 프로그래밍 언어를 사용하여 호환되지 않는 유형의 시스템 간에 (Django-SQL)데이터를 변환하는 프로그래밍 기술
- OOP프로그래밍에서 RDBMS을 연동할 때, 데이터베이스와 객체 지향 프로그래밍 언어 간의 호환되지 않는 데이터를 변환하는 프로그래밍 기법
- Django는 내장 Django ORM을 사용한다. 

<br>

| 장점                                                         | 단점                                                     |
| ------------------------------------------------------------ | -------------------------------------------------------- |
| SQL을 잘 알지 못해도 DB 조작이 가능하다.                     | ORM만으로는 완전한 서비스를 구현하기 어려운 경우가 있다. |
| SQL의 절차적 접근이 아닌 객체 지향적 접근으로 인한 높은 생산성. |                                                          |

**우리는 DB를 객체(object)로 조작하기 위해 ORM을 사용한다.** 

<br>

### 2) models.py 

```python
from django.db import models

# db테이블에 대응하는 모델 클래스를 정의 (데이터를 저장하기 위함)
class Article(models.Model):
    # 우리가 저장할 데이터 타입: model.클래스 
    title = models.CharField(max_length=10)
    content = models.TextField()
```

- 각 모델은 `django.models.Model`클래스의 서브 클래스로 표현된다. 
  - `django.db.models`모듈의 Model클래스를 상속받는다. 
- **models 모듈을 통해 어떤 타입의 DB 칼럼을 정의할 것인지 정의한다.** 
  - title과 content은 모델의 필드(열, 속성)를 나타낸다.
  - 각 필드는 클래스 속성으로 지정되어 있으며, 각 속성은 각 데이터 베이스의 열에 매핑된다.  

<br>

#### (1) 사용된 model field

- `CharField(max_length=None, **options)`
  - 길이의 제한이 있는 문자열을 넣을 때 사용한다.
  - CharField의 max_length는 필수 인자이다. 
  - 필드의 최대 길이(문자), 데이터베이스 라벨과 Django의 유효성 검사 (값을 검증하는 것)에서 활용된다. 
- `TextField(**options)`
  - 글자의 수가 많을 때 사용한다. 
  - max_length 옵션 작성시 자동 양식 필드인 textarea 위젯에 반영은 되지만 모델과 데이터베이스 수준에는 적용되지 않는다. 
  - **max_length 사용은 `CharField`에서 사용해야 한다.** 

<br>

## 3. Migrations

>  class를 정의 하면 이에 대응하여 DB에 반영 되어야 한다. (데이터 테이블이 생성 등)
>
> 이를 Migrations라고 한다.  즉, django가 model에 생긴 변화를 반영하는 방법

장고는 ORM을 사용하기 때문에 models.py와 클래스를 통해 DB 스키마를 생성하고 컨트롤 하게 되는데, 

이 때 **DB 스키마를 git처럼 버전으로 나눠서 관리 할 수 있게 해 주는 시스템**이다.

<br>

### 1) Migration 실행 및 DB 스키마를 다루는 명령어

> 클래스 정의하고 실제 db에 반영하는 것은 2단계가 걸린다.
>
> **`makemigrations`(설계도를 만든다) => ` migrate` (반영한다)** 

#### (1) `makemigrations`

```bash
$ python manage.py makemigrations [app_name]
```

- model을 변경한 것에 기반한 새로운 마이그레이션( -db 테이블 설계도-)을 만들 때 사용한다. 
- 단, 프로젝트 생성 후 처음 하는 migrate 작업을 위한 마이그레이션을 생성할 때는 app_name을 생략해야 한다. 

<br>

#### (2) `migrate`

```bash
$ python manage.py migrate [app_name] [migration_name]
```

- 마이그레이션을 DB에 반영하기 위해 사용한다. 
- 설계도를 실제 DB에 반영하는 과정이다.
- 모델에서의 변경 사항들과 DB의 스키마가 동기화를 이룬다. 
- 기본적으로 PK를 생성하지 않으면,  record가 추가 될 때마다 자동 생성된다. 

<br>

#### (4) `sqlmigrate`

```bash
$ python manage.py sqlmigrate [app name] [migration name]
```

```bash
$ python manage.py sqlmigrate atricles 0001

BEGIN;
--
-- Create model Article
--
CREATE TABLE "articles_article" ("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, 
"title" varchar(10) NOT NULL, "content" text NOT NULL);
COMMIT;
```

- 마이그레이션(설계도)에 대한 SQL 구문을 보기 위해 사용한다. 
- 마이그레이션이 SQL 문으로 어떻게 해석되어서 동작할지 미리 확인 할 수 있다. 
- 실제로 작성된 파이썬 코드가 sql문으로 어떻게 바뀌어서 작동되는 지 확인하기 위함이다. 

<br>

#### (5) `showmigrations`

```bash
$ python manage.py showmigrations [app_name]

admin
 [X] 0001_initial
 [X] 0002_logentry_remove_auto_add        
 [X] 0003_logentry_add_action_flag_choices
articles
 [X] 0001_initial
auth
 [X] 0001_initial
 [X] 0002_alter_permission_name_max_length
 [X] 0003_alter_user_email_max_length
 [X] 0004_alter_user_username_opts
 [X] 0005_alter_user_last_login_null
 [X] 0006_require_contenttypes_0002
 [X] 0007_alter_validators_add_error_messages
 [X] 0008_alter_user_username_max_length
 [X] 0009_alter_user_last_name_max_length
 [X] 0010_alter_group_name_max_length
 [X] 0011_update_proxy_permissions
 [X] 0012_alter_user_first_name_max_length
contenttypes
 [X] 0001_initial
 [X] 0002_remove_content_type_name
sessions
 [X] 0001_initial
```

- 프로젝트 전체의 마이그레이션 상태를 확인하기 위해 사용한다.
- 마이그레이션 파일들이 migrate 됐는지 안됐는지 여부를 확인 할 수 있다. 

<br>

### 2) Model Field 추가 후, Migrations

```python
class Article(models.Model):
    title = models.CharField(max_length=10)
    content = models.TextField()  
    # 새로운 record(데이터 테이블의 행)가 저장될 때의 시간을 자동으로 저장
    created_at = models.DateTimeField(auto_now_add=True)    
    # 새로 뭔가 변경돼서 내부 필드의 값이 변경될 때 시간을 자동으로 저장
    updated_at = models.DateTimeField(auto_now=True)

```

-  DB 테이블에 새로운 열을 추가 => **기존에 테이블에 저장되어 있던 데이터가 갖게 되는 `default 값`을 설정해주어야 한다.** 

#### (1) `makemigrations`

```bash
$ python manage.py makemigrations

You are trying to add the field 'created_at' with 'auto_now_add=True' to article without a default; the database needs something to populate existing rows.

 1) Provide a one-off default now (will be set on all existing rows)
 2) Quit, and let me add a default in models.py
Select an option:
```

- 기존 데이터에 대한 default 값을 지금 설정할 것인지, models.py에서 처리하고 다시 마이그레이션을 만들 것인지 물어봄

```bash
Select an option: 1
Please enter the default value now, as valid Python
You can accept the default 'timezone.now' by pressing 'Enter' or you can provide another value.
The datetime and django.utils.timezone modules are available, so you can do e.g. timezone.now
Type 'exit' to exit this prompt
[default: timezone.now] >>>
```

- 1을 선택할 경우 바로 default값을 프롬프트 창을 통해 설정할 수 있다. 

<br>

#### (2) `migrate`

```bash
$ python manage.py migrate
```

<br>

#### (3) 사용된 model field

- `auto_now_add`
  - 최초 생성 일자 
  - Django ORM이 최초 insert(테이블에 데이터 입력)시에만 현재 날짜와 시간으로 갱신한다. (테이블에 어떤 값을 최초로 넣을 때)
- `auto_now`
  - 최종 수정 일자
  - Django ORM이 save를 할 때마다 현재 날짜와 시간으로 갱신한다. 

<br>

| `models.py`                             | model 변경사항 발생 시           |
| --------------------------------------- | -------------------------------- |
| **`$ python manage.py makemigrations`** | **migrations 파일 생성**         |
| **`$ python manage.py migrate`**        | **DB 반영 (모델과 DB의 동기화)** |

<br>

## 4. Database API 

> DB를 조작하기 위한 도구이다. 

- Django가 기본적으로 ORM을 제공함에 따른 것으로 DB를 편하게 조작할 수 있도록 돕는다. 
- Model을 만들면 Django는 객체들을 만들고 읽고 수정하고 지울 수 있는 database-abstract API를 자동으로 만든다.
  - database-abstarct API 혹은 database-access API 라고도 한다. 

<br>

### 1) DB API 구문 - Making Queries

예시) **`Article. objects.all()`**

- Atricle : **Class Name**
- objects: **Manager**
  - Django 모델에 데이터 베이스 query 작업이 제공되는 인터페이스
  - 기본적으로 모든 Django 모델 클래스에 objects라는 Manager를 추가한다. 
- all(): **QuerySet API**
  - 데이터베이스로부터 전달받은 객체 목록
  - queryset 안의 객체는 0개, 1개 혹은 여러 개일 수 있다.
  - 데이터베이스로부터 조회, 필터, 정렬 등을 수행할 수 있다. 

<br>

### 2) Django shell

>  db조작과 관련된 api와 관련된것을 연습해보기 위해 사용한다. 
>
> 실제로는 view함수에 적용해야 한다. 

- 일반 Python shell을 통해서는 장고 프로젝트 환경에 접근할 수 없다. 
- 장고 프로젝트 설정이 load 된 python shell을 활용해 DB API 구문 테스트를 진행할 수 있다. 
- 기본 Django shell보다 더 많은 기능을 제공하는 shell_plus를 사용해서 진행한다. 

<br>

#### (1) 설치 

```bash
$ pip install ipython 
$ pip install django-extensions
```

<br>

#### (2) 앱 등록

```python
INSTALLED_APPS = [
    ...,
    # 3rd party apps 
    'django_extensions', 
    ...,
]
```

<br>

#### (3) shell_plus 실행

```bash
$ python manage.py shell_plus
```

<br>

## 5. CRUD

> 대부분은 컴퓨터 소프트웨어가 가지는 기본적인 데이터 처리기능
>
> Create(생성), Read(읽기), Update(갱신), Delete(삭제)

**참고로 아래의 예시들은 shell_plus에서 CRUD를 연습한 결과이다. ** 

### 1) CREATE

#### (1) 인스턴스 생성 후 인스턴스 변수를 설정한다. 

```python
# 특정 테이블에 새로운 행을 추가하여 데이터를 추가한다. 

>>> article = Article()  # Article(class)로부터 article(instance)를 생성한다. 

>>> article
<Article: Article object (None)>

# 인스턴스 변수에 값을 할당한다. 
>>> article.title = 'first' 

>>> article.content = 'django!'

# save를 하지 않으면 DB에 값이 저장되지 않는다. 
>>> article
<Article: Article object (None)>

>>> Article.objects.all()
<QuerySet []>

# 저장을 하고 확인하면 DB 테이블이 채워진 것을 볼 수 있다. 
>>> article.save()

>>>article
<Article: Article object (1)>

>>> Article.objects.all()
<QuerySet [<Article: Article object (1)>]>

# 인스턴스인 article을 활용하여 변수에 접근이 가능하다. (저장된 것을 확인하기 위함)
>>> article.title
'first'

>>> article.created_at
datetime.datetime(2022, 3, 8, 4, 10, 43, 956404, tzinfo=<UTC>)

>>> article.content
'django!'

>>> article.updated_at
datetime.datetime(2022, 3, 8, 4, 10, 43, 956404, tzinfo=<UTC>)
```
- `save()` method
  - saving objects
  - 객체를 데이터베이스에 저장한다
  - 데이터 생성 시 save()를 호출하기 전에는 객체의 ID 값이 무엇인지 알 수 없다. 
    - ID 값은 Django가 아니라 DB에서 계산되기 때문이다.
  - 단순히 모델을 인스턴스화 하는 것은 DB에 영향을 미치지 않기 때문에 반드시 save()가 필요하다. 

<br>

#### (2) 초기 값과 함께 인스턴스 생성

```python
>>> article = Article(title='second', content='django!')

# 아직 지정이 안되어 있다. 
>>> article
<Article: Article object (None)>
    
# 저장!
>>> article.save()

>>> article
<Article: Article object (2)>

>>> Article.objects.all()
<QuerySet [<Article: Article object (1)>, <Article: Article object (2)>]>

# 값 확인
>>> article.pk
2

>>> article.title
'second'

>>> article.content
'django!!!!!'
```

<br>

#### (3) QuerySet API-create()사용

```python
# 위의 2개의 방식과는 다르게 바로 쿼리 표현식을 리턴한다. 
>>> Article.objects.create(title='third', content='django!!')
<Article: Article object (3)>
```

<br>

#### (4) 기타 - str method 

```python
# models.py

class Article(models.Model):
    title = models.CharField(max_length=10)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    # 메소드 추가는 db에 반영할 필요가 없기때문에 마이그레이션 관련 명령어를 사용하지 않아도 괜찮다. 
    def __str__(self):
        return self.title 
```

- 표준 파이썬 클래스의 메소드인 `str()`을 정의하여 각각의 object가 사람이 읽을 수 있는 문자열을 반환(return)하도록 할 수 있다.
- 작성 후 반드시 shell_plus를 재시작해야 반영된다. 


```python
>>> Article.objects.all()
 <QuerySet [<Article: first>, <Article: second>, <Article: third>]>
```

<br>

### 2) READ

> QuerySet API method를 사용해 다양한 조회를 하는 것이 중요하다. 

:pencil2:참고자료: https://docs.djangoproject.com/ko/3.2/ref/models/querysets/

QuerySet API method는 크게 2가지로 분류된다. 

1. Methods that return new querysets
2. Methods that do not return querysets

<br>

#### (1) all()

현재 QuerySet의 복사본을 반환한다. 

```python 
>>> Article.objects.all()
 <QuerySet [<Article: first>, <Article: second>, <Article: third>]>
```

<br>

#### (2) get()

주어진 lookup 매개변수와 일치하는 객체를 반환한다. (쿼리셋을 리턴해주는 것이 아니라 단일의 인스턴스를 리턴)

**객체를 찾을 수 없으면 DoesNotExist 예외를 발생시키고, 둘 이상의 객체를 찾으면 MutipleObjectsReturned 예외를 발생 시킨다.**

따라서, PK와 같이 고유성이 보장하는 조회에서 사용된다. 

```python
>>> Article.objects.get(pk=100)
DoesNotExist: Article matching query does not exist.
    
>>> Article.objects.get(content='django!')
MultipleObjectsReturnes: get() returnes more than one Article -- it returned 2!
```

<br>

#### (3) filter()

주어진 lookup 매개변수와 일치하는 객체를 포함하는 새 QuerySet을 반환한다. 

filter는 없으면 그냥 빈리스트!

```python
>>>  Article.objects.filter(pk=100)
<QuerySet []>

>>> Article.objects.filter(content='django!')
<QuerySet [<Article: first>, <Article: second>]>
```

<br>

### 3) UPDATE

article 인스턴스 객체의 인스턴스 변수의 값을 변경 후 저장한다. 

```python
# UPDATE articles SET title='byebye' WHERE id=1;
>>> article = Article.objects.get(pk=1)

>>> article.title
'first'

# 값을 변경하고 저장한다. 
>>> article.title = 'byebye'

>>> article.save()

# 정상적으로 변경된 것을 확인한다. 
>>> article.title
'byebye'
```

<br>

### 4) DELETE

QuerySet의 모든 행에 대해 SQL 삭제 쿼리를 수행하고,삭제된 객체 수와 객체 유형당 삭제 수가 포함된 딕셔너리를 반환한다. 

```python
>>> article = Article.objects.get(pk=1)

# 삭제
>>>  article.delete()
(1, {'articles.Article': 1})

# 1번은 이제 찾을 수 없다. 
>>> Article.objects.get(pk=1)
DoesNotExist: Article matching query does not exist.
```

<br>

### 5) Field lookups

> 조회 시 특정 검색 조건을 지정할 수 있다. 

:pencil2: 참고자료: https://docs.djangoproject.com/en/3.2/ref/models/querysets/#field-lookups

- QuerySet 메서드 filter(), exclude() 및 get()에 대한 키워드 인수로 지정된다. 

```python
>>> Article.objects.get(pk__gt=2)
<Article: third>

>>> Article.objects.filter(content__contains='ja')
<QuerySet [<Article: second>, <Article: third>]>

>>> Article.objects.filter(content__startswith='dj')
<QuerySet [<Article: second>, <Article: third>]>

>>> Article.objects.filter(content__endswith='!')
<QuerySet [<Article: second>, <Article: third>]>
```

<br>

### 6) QuerySet Api

- 데이터베이스 조작을 위한 다양한 QuerySet API methods는 해당 공식문서를 반드시 참고하여 학습해야 한다. 

:pencil2:참고자료: https://docs.djangoproject.com/en/3.2/ref/models/querysets/#queryset-api-reference

<br>

## 6. Admin Site

> 사용자가 아닌 서버의 관리자가 활용하기 위한 페이지이다.

- model class를 `admin.py`에 등록하고 관리한다.
- django.contrib.auth 모듈에서 제공된다. 
- record 생성 여부 확인에 매우 유용하며, 직접 record를 삽입할 수도 있다. 

<br>

### 1) admin 생성

```bash
$ python manage.py createsuperuser
```

- 관리자 계정 생성 후 서버를 실행한 다음 '/admin'으로 가서 관리자 페이지 로그인
  - 계정만 만든 경우, Django 관리자 화면에서 아무 것도 보이지 않는다. 
- 내가 만든 Model을 보기 위해서는 admin.py에 작성하여 Django 서버에 등록한다.
- **[주의] auth에 관련된 기본 테이블이 생성되지 않으면 관리자 계정을 생성할 수 없다.** 

<br>

### 2) Model을 admin에 등록

```python
# admin.py

from django.contrib import admin

# 클래스 추가
from .models import Article

# admin site에 출력되는 화면을 수정할 수 있다.
# option사항이다. (없어도 무방하다.)
class ArticleAdmin(admin.ModelAdmin):
    list_display = ('pk', 'title', 'content', 'created_at', 'updated_at',)
    list_filter = ('created_at',)
    list_display_links = ('pk', 'title',)

# admin site에 Article를 register 하겠다.
admin.site.register(Article, ArticleAdmin)
```

- admin.py는 관리자 사이트에 Article 객체가 관리자 인터페이스를 가지고 있다는 것을 알려주는 것이다. 
- models.py에 정의한 `__str__`의 형태로 객체가 표현된다. 
- `list_display`
  - models.py 정의한 각각의 속성(칼럼)들의 값(레코드)를 admin 페이지에 출력하도록 설정한다. 

:pencil2:참고자료: https://docs.djangoproject.com/en/3.2/ref/contrib/admin/#modeladmin-options

<br>

## 7. CRUD with views

> url =>  view(여기서 데이터를 CRUD 한다. ) => templates 

<br>

### 1) 게시글 조회(READ)

```python
# articles/urls.py

from django.urls import path
from . import views

urlpatterns = [
    path('', views.index, name='index'),
]
```

```python
# articles/views.py

from django.shortcuts import render
from .models import Article

def index(request):
    # 클래스와 QuerySet API를 통해 저장된 DB테이블에 접근한다. 
    articles = Article.objects.all()
    context = {
        'articles' : articles, 
    }
    return render(request, 'articles/index.html', context)
```

```html
<!-- articles/templates/articles/index.html --> 

{% extends 'base.html' %}

{% block content %}
  <h1 class="text-center">Articles</h1>
  <hr>
  {% for article in articles %}
    <p>글 번호: {{ article.pk }}</p>
    <p>글 제목: {{ article.title }}</p>
    <p>글 내용: {{ article.content }}</p>
    <hr>
  {% endfor %}
{% endblock content %}

```

<br>

**:speech_balloon: 만약 게시글 정렬 순서를 변경하고 싶다면..?** 

```python
def index(request):
    # 1. DB로 부터 받은 쿼리셋을 이후에 파이썬이 변경한다.(Python이 조작)
    articles = Article.objects.all(::-1)
    
    # 2. 처음부터 내림차순 쿼리셋으로 받는다.(DB가 조작한다. )
    articles = Article.objects.order_by('-pk')
```

<br>

### 2) 게시글 작성(CREATE)

#### (1) 게시글을 작성하는 페이지 

```python
# articles/urls.py

path('new/', views.new ,name='new')
```

```python
# articles/views.py

def new(request):
    return render(request, 'articles/new.html')
```

```html
<!-- articles/templates/articles/new.html --> 

{% extends 'base.html' %}

{% block content %}
  <h1 class="text-center">NEW</h1>
  <hr>
  <form action="#" method="GET">
    <label for="title">제목:</label>
    <input type="text" name="title" id="title"><br>
    <label for="content">내용:</label>
    <textarea name="content" id="content" cols="30" rows="10"></textarea><br>
    <button class="btn btn-primary">등록</button>
  </form>
  <a href=" {% url 'articles:index' %} ">INDEX</a>
{% endblock content %}
```

- 등록 버튼을 누르면 title=입력 값, content=입력 값을 쿼리로 갖고 특정페이지로 이동한다. 
  - 현재는 이동할 페이지가 만들어지지 않았기때문에 이 페이지로 다시 돌아온다. 

<br>

#### (2) 게시글에 작성된 데이터를 받은 후 index로 리디렉션

```python
# articles/urls.py

path('create/', views.create, name='create')
```

```python
# articles/views.py

from django.shortcuts import render, redirect
from .models import Article 

def create(request):
    title = request.GET.get('title')
    content = request.GET.get('content')
    
    # DB에 저장하기
    article = Article(title = title, content = content)
    article.save()

    # return redirect('/articles/')
    return redirect('articles:index')
```

**리디렉션을 하기위해서는` django.shortcuts`에 있는 `redirect()`함수를 사용해야 한다**

> 만약 사용하지 않는다면..?
>
> ```python
> def create(request):
>     title = request.GET.get('title')
>     content = request.GET.get('content')
>     
>     article = Article(title = title, content = content)
>     article.save()
> 
>     return render(request, 'articles/index.html')
> ```
>
> 글을 작성 후 index페이지가 출력되지만 게시글이 조회되지 않는다. 
>
> 그 이유는 URL은 여전히 create에 머물러 있고 단순히 index 페이지만 render 되었을 뿐이다. 
>
> (create view함수에서 다루고 있는 데이터로 index 페이지가 render된다. )

<br>

**`redirect()`**

- 새 URL로 요청을 다시 보낸다.
- 인자에 따라 HttpResponseRedirect를 반환한다.
- 브라우저는 현재 경로에 따라 전체 URL 자체를 재구성한다. (reconstruct)
- 사용 가능한 인자
  1. model
  2. view name: **URL pattern name** or callable view object 
  3. 절대 경로 혹은 상대 경로

<br>

#### (3) 1번과 2번을 연결

```html
<!-- articles/templates/articles/new.html --> 

{% extends 'base.html' %}

{% block content %}
  <h1 class="text-center">NEW</h1>
  <hr>
  <form action="{% url 'articles:create' %}" method="GET">
    <label for="title">제목:</label>
    <input type="text" name="title" id="title"><br>
    <label for="content">내용:</label>
    <textarea name="content" id="content" cols="30" rows="10"></textarea><br>
    <button class="btn btn-primary">등록</button>
  </form>
  <a href=" {% url 'articles:index' %} ">INDEX</a>
{% endblock content %}
```

<br>

### 3) 게시글 작성시, 데이터 전송 방식을 수정 (CREATE)

| GET                                            | POST                                                         |
| ---------------------------------------------- | ------------------------------------------------------------ |
| 특정 리소스를 가져오도록 요청할 때 사용한다.   | 서버로 데이터를 전송할 때 사용한다.                          |
| 반드시 **데이터를 가져올 때**만 사용해야 한다. | 리소스를 생성 / 변경하기 위해 데이터를 HTTP body에 담아 전송한다. |
| **DB에 변화를 주지 않는다.**                   | **서버에 변경사항을 만든다.**                                |
| CRUD에서 **R 역할**을 담당한다.                | CRUD에서 **C/U/D 역할**을 담당한다.                          |

**즉, CREATE는 POST방식으로 데이터를 전송해야 한다.** 

<br>

#### (1) CSRF 토큰 생성

> Django는 CSRF이라는 보안 위협에 대항하여 middle ware와 template를 제공한다. 

- 사이트 간 요청 위조 (Cross-site request forgery)

  웹 애플리케이션 취약점 중 하나로 사용자가 자신의 의지와 무관하게 공격자가 의도한 행동을 하여 

  특정 웹페이지를 보안에 취약하게 하거나 수정, 삭제 등의 작업을 하게 만드는 공격방법이다. 

- CSRF 공격 방어

  Security Token 사용 방식 (CSRF Token) : 사용자의 데이터에 임의의 난수 값을 부여해, 매 요청마다 해당 난수 값을 포함시켜 전송 시키도록한다. 

  이후, 서버에서 요청을 받을 때마다 전달된 token 값이 유효한지 검증한다. (만약 유효하지 않으면 403forbidden에러..)

  일반적으로 데이터 변경이 가능한 POST, PARCH, DELETE Method 등에 적용된다. (GET 제외)

  Django는 CSRF token 템플릿 태그를 제공한다. 

```python
<!-- articles/templates/articles/new.html --> 

{% extends 'base.html' %}

{% block content %}
  <h1 class="text-center">NEW</h1>
  <hr>
  <form action="{% url 'articles:create' %} " method="POST">
    {% csrf_token %}
    ...
    
```

- `{% csrf_token %}`
  - CSRF 보호에 사용된다.
  - input type이 hidden으로 작성되며 value는 Django에서 생성한 hash 값으로 설정된다.
  - 해당 태그 없이 요청을 보낸다면 Django 서버는 403 forbidden을 응답한다. 
- **form태그 내부의 최상단에 기입한다.** 

<br>

:speech_balloon:**CsrfViewMiddleware**

```python
# setting.py

MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]
```

- CSRF 공격 관련 보안 설정은 settings.py에서 `MIDDLEAWARE`에 작성 되어있다.

- 실제로 요청 과정에서 urls.py 이전에 Middleware의 설정 사항들을 순차적으로 거치며 응답은 반대로 하단에서 상단으로 미들웨어를 적용시킨다. 

- MIddleware
  - 공통 서비스 및 기능을 애플리케이션에 제공하는 소프트웨어이다.
  - 데이터 관리, 애플리케이션 서비스, 메시징, 인증 및 API 관리를 주로 미들웨어를 통해 처리한다.
  - 개발자들이 애플리케이션을 보다 효율적으로 구축할 수 있도록 지원하며 애플리케이션, 데이터 및 사용자 사이를 연결하는 요소처럼 가동한다. 

<br>

#### (2) views.py 수정

```python
# articles/views.py

def create(request):
    # GET을 POST로 수정
    title = request.POST.get('title')
    content = request.POST.get('content')
 
    article = Article(title = title, content = content)
    article.save()    
    return redirect('articles:index')

```

<br>

### (3) 게시글 상세 페이지 조회 (READ)

> 개별 게시글 상세 페이지로 글의 번호(PK)를 활용하여 각각의 페이지를 따로 구현한다. 

#### (1) detail 페이지 생성

```python
# articles/urls.py

path('<int:pk>/', views.detail, name='detail')
```

```python
# articles/views.py

def detail(request, pk):
    article = Article.objects.get(pk=pk)
    context = {
        'article': article, 
    }
    return render(request, 'articles/detail.html', context)
```

- `article = Article.objects.get(pk=pk)`에서 
  - 오른쪽 pk는 variavle routinf을 통해 받은 pk!
  - 왼쪽 pk는 키워드인자!

```html
<!-- articles/templates/articles/deail.html -->

{% extends 'base.html' %}

{% block content %}
  <h1>DETAIL</h1>
  <hr>
  <p>글 번호: {{ article.pk }}</p>
  <p>글 제목: {{ article.title }}</p>
  <p>글 내용: {{ article.content }}</p>
  <p>작성 시각: {{ article.created_at }}</p>
  <p>수정 시각: {{ article.updated_at }}</p>
  <hr>
  <a href="{% url 'articles:index' %}">INDEX</a>
{% endblock content %}
```

<br>

#### (2) index와 연결

```html
<!-- articles/templates/articles/index.html -->

{% extends 'base.html' %}

{% block content %}
  <h1 class="text-center">Articles</h1>
  <hr>
  {% for article in articles %}
    <p>글번호: {{ article.pk }}</p>
    <p>글제목: {{ article.title }}</p>
    <p>글내용: {{ article.content }}</p>
    <a href="{% url 'articles:detail' article.pk %}">상세 보기</a>
    <hr>
  {% endfor %}
  <a href=" {% url 'articles:new' %} ">NEW</a>
{% endblock content %}
```

- `{% url 'articles:detail' article.pk %}`: url이 변수이기 때문에 변수의 값을 같이 보내줘야 한다. 

<br>

#### (3) 글 생성 후 리디렉션 되는 링크 수정

```python
# articles/views.py

def create(request):
    title = request.POST.get('title')
    content = request.POST.get('content')
    article = Article(title = title, content = content)
    article.save()

    # redirect 인자를 변경한다.  
    return redirect('articles:detail',article.pk)
```

- `redirect('articles:detail',article.pk)`: url이 변수이기 때문에 변수의 값도 인자로 같이 보내주어야한다. 

<br>

### 4)  게시글 삭제 (Delete)

```python
# articles/urls.py

path('<int:pk>/delete/', views.delete, name='delete'),
```

```python
# articles/views.py

def delete(request, pk):
    # url로 접근했을 때 데이터가 삭제되는 것을 막기 위함(예시. articles/3/delete 직접 url을 쳐서 접근하는 것 방지)
    # 삭제는 form submit 했을 때만 가능한 것!
    article = Articles.objects.get(pk=pk)
    if request.method == 'POST':
        article.delete()
        return redirect('articles:index')
    else:
        return redirect('articles:detail', article.pk)
```

```html
<!-- articles/templtes/articles/detail.html -->

{% extends 'base.html' %}
{% block content %}
   ...
  <form action="{% url 'articles:delete' article.pk %}" method="POST">
    {% csrf_token %}
    <input class="btn btn-danger" type="submit" value="DELETE">
  </form>
  ...
{% endblock content %}
```

- a태그가 아닌 form태그를 사용하는 이유는 데이터에 변화가 있을 때는 ,  `GET`이 아닌`POST`방식을 사용해야 하기 때문이다. 
- 그 결과 view함수에도 `POST`에 대한 조건을 생성한다. 

<br>

### 5) 게시글 수정 (UPDATE)

#### (1) Edit 페이지 생성

```python
# articles/urls.py

path('<int:pk>/edit/', views.edit, name='edit')
```

```python
# articles/views.py

def edit(request,pk):
    article = Articles.objects.get(pk=pk)
    context = {
        'article':article
    }
    return render(request, 'articles/edit.html', context)
```

```html
<!-- articles/templates/articles/edit.html -->

{% extends 'base.html' %}
{% block content %}
  <h1 class="text-center">NEW</h1>
  <hr>
  <form action="#" method="POST">
    {% csrf_token %}
    <label for="title">제목 </label>
    <input type="text" id="title" name="title" value="{{ article.title }}"><br>
    <label for="content">내용</label>
    <textarea name="content" id="content" cols="30" rows="10">{{ article.content }}</textarea><br>
    <input class="btn btn-primary" type="submit" value="수정">
  </form>
  <hr>
  <a href="{% url 'articles:detail' article.pk %}">BACK</a>
{% endblock content %}
```

- 수정은 기존에 입력 되어 있던 데이터를 보여주는 것이 좋기 때문에 html태그의 value 속성을 사용했다. 
- `textarea`태그는 value속성이 없으므로 태그 내부 값으로 설정한다. 

<br>

#### (2) 수정한 결과를 DB에 반영

```python
# articles/urls.py

path('<int:pk>/update/', views.update, name='update'),
```

```python
# articles/views.py

def update(request, pk):
    article = Articles.objects.get(pk=pk)
    article.title = request.POST.get('title')
    article.content = request.POST.get('content')
    
    article.save()
    return redirect('articles:detail', article.pk)
```

- 상세페이지로 리디렉트!

<br>
**:speech_balloon:`redirection`**

- 호출이되면 응답으로 다른 페이지(location)를 호출하도록 한다. 
- 302 응답코드 

<br>
