# Django frame work 

[toc]



## 1. Web framework

### 1) Web Page

> web이란 world wide web의 줄임말로 인터넷에 연결된 컴퓨터를 통해 정보를 공유할 수 있는 전 세계적인 정보 공간을 뜻한다. 

**클라이언트(웹 브라우저) ====요청(메인 페이지줘!)====> 서버 (네이버)**

**클라이언트(웹 브라우저) <====응답(HTML)==== 서버 (네이버)**

<br>

- **static web page: `정적 웹페이지`, `기존에 준비한 웹사이트` , `flat page`**
  - 서버에 미리 저장된 파일이 사용자에게 그대로 전달되는 웹 페이지이다.
  - 서버가 정적 웹 페이지에 대한 요청을 받은 경우, 서버는 **추가적인 처리 과정 없이** 클라이언트에게 응답을 보낸다. 
  - 모든 상황에서 모든 사용자에게 동일한 정보를 표시한다.
  - 일반적으로 HTML, CSS, JS로 작성된다.

<br>

- **Dynamic web page: `동적 웹페이지`**
  - 웹페이지에 대한 요청을 받은 경우, 서버는 추가적인 처리 과정 이후 클라이언트에게 응답을 보낸다.
  - 동적 웹 페이지는 방문자와 상호작용하기 때문에 페이지 내용은 그때그때 다르다.
  - 서버 사이드 프로그래밍 언어(파이썬, 자바 등)이 사용되며, 파일을 처리하고 데이터 베이스와의 상호작용이 이루어진다. 

<br>

### 2) Web framework (Application framework)란

> 재 사용할 수 있는 수많은 코드를 프레임워크로 통합함으로써 
>
> 개발자가 새로운 어플리케이션을 위한 표준 코드를 다시 작성하지 않아도같이 사용할 수 있도록 도운다.

- 프로그래밍에서 특정 운영 체제를 위한 응용 프로그램 표준 구조를 구현하는 클래스와 라이브러리 모임이다. 
- 웹 페이지를 개발하는 과정에서 겪는 어려움을 줄이는 것이 주 목적이다. 
  - 데이터 베이스 연동, 템플릿 형태의 표준, 세션 관리, 코드 재사용등의 기능을 포함한다.
  - 동적인 웹 페이지나, 웹 애플리케이션, 웹 서비스 개발 보조용으로 만들어지는 Application framework의 일종이다.

<br>

### 3)  framework 구조

> 장고는 어떤 구조로 디자인 되어있는가

**MVC Design Pattern (model - view - controller)**

- 소프트웨어 공학에서 사용되는 디자인 패턴 중 하나이다.
- 사용자 인터페이스로부터 프로그램 로직을 분리!
  - 애플리 케이션의 시각적 요소나 이면에서 실행되는 부분을 서로 영향 없이 쉽게 고칠 수 있는 애플리케이션을 만들 수 있다. 

<br>

**=> 장고는 `MTV Pattern`이라고 한다.** 

- view를 template로 본다. 
- controller를 view로 본다. 

<br>

<img src="https://mdn.mozillademos.org/files/13931/basic-django.png">



| MTV PATTERN | 설명                                                         |
| ----------- | ------------------------------------------------------------ |
| Model       | 응용 프로그램의 데이터 구조를 정의하고 데이터 베이스의 기록을 관리(추가, 수정, 삭제) |
| Template    | 파일의 구조나 레이아웃을 정의한다. <br />유저에게 실제로 보여지는 부분으로 실제 내용을 보여주는 데 사용된다. (presentation) |
| View        | HTTP요청을 수신하고 HTTP 응답을 반환한다.<br />Model을 통해 요청을 충족시키는데 필요한 데이터에 접근한다.<br />template에게 응답의 서식 설정을 맡긴다. |

**.py 3대장 기억하기**

- `urls.py` : 주소(URL) 관리
- `views.py` : 페이지 관리 (페이지 하나 당, 하나의 함수)
- `models.py` : 데이터베이스 관리

<br>

**[참고]** `runserver` **Automatic reloading**

- 개발 서버는 요청이 들어올 때마다(코드가 저장될 때 마다) 자동으로 Python 코드를 다시 불러온다.
- 코드의 변경사항을 반영하기 위해서 굳이 서버를 재가동 하지 않아도 된다.
- 그러나, 파일을 추가하는 등의 몇몇의 동작(커스텀 필터, 새로운 모듈 추가 등)은 개발 서버가 자동으로 인식하지 못하기 때문에, 이런 상황에서는 서버를 재가동 해야 적용되는 경우도 있다.

<br>

## 2. Django

### 1) 기본 세팅

> 1. 가상 환경 생성 및 활성화
> 2. django 설치
> 3. 프로젝트 생성
> 4. 서버 켜서 로켓 확인하기
> 5. 앱 생성
> 6. 앱 등록

<br>

#### (1) 가상 환경 설정

- 가상환경 생성

```shell
$ python -m venv venv
```
<br>
- 활성화

```bash
$ source venv/Scripts/activate
```
<br>
- 비활성화

```bash
$ deactivate
```
<br>
- VScode에서 가상환경 설정
  - `ctrl + shift + p` 누르고 설정!


<br>

#### (2) Django 설치

```bash
$ pip install django==3.2.12
$ pip list
$ pip install -r requirements.txt
```
<br>
- 반드시 패키지를 설치하면 freeze 한다.
```bash
$ pip freeze > requirements.txt
```
<br>
- 다시 설치하기

```bash
$ pip install -r requirements.txt
```

<br>

#### (3) 프로젝트 생성

```bash
$ django-admin startproject <프로젝트이름> .
```

1. 프로젝트 이름에는 Python이나 Django에서 사용중인 키워드를 피해야 한다. 
2. `-`하이픈도 사용할 수 없다. 

<br>

| 프로젝트 구조 | 설명                                                         |
| ------------- | ------------------------------------------------------------ |
| `__init__.py` | python에게 이 디렉토리를 하나의 python 패키지로 다루도록 지시한다. |
| `asgi.py`     | Asynchronous Server Gateway Interface의 줄임말<br />Django 애플리케이션이 비동기식 웹 서버와 연결 및 소통하는 것을 도와준다. |
| `settings.py` | 애플리케이션의 모든 설정을 포함한다.                         |
| `urls.py`     | 사이트의 url과 적절한 views의 연결은 지정한다.               |
| `wsgi.py`     | Web Server Gateway Interface의 줄임말<br />Django 애플리케이션이 웹서버와 연결 및 소통하는 것은 도와준다. |
| `manage.py`   | Django프로젝트와 다양한 방법으로 상호작용을 하는 커맨드 라인 유틸리티이다. |

```bash
# manage.py Usage
$ python manage.py <command> [option]
```

- 프로젝트는 앱의 집합이다. (collection of apps)
- 프로젝트에는 여러 앱이 포함될 수 있다.
- 앱은 여러 프로젝트에 있을 수 있다. 

<br>

#### (4) 서버 실행해서 로켓 확인

```bash
$ python manage.py runserver 
```

종료는 `ctrl + c`이다.



**[참고사항] LTS **

- Long Term Support(장기 지원 버전)
- 일반적인 경우보다 장기간에 걸쳐 지원하도록 고안된 소프트웨어의 버전
- 컴퓨터 소프트웨어의 제품 수명주기 관리 정책
- 배포자는 LTS 확정을 통해 장기적이고 안정적인 지원을 보장한다. 

<br>

#### (5) 앱생성

- **[중요]** 앱을 생성하고 `settings.py` 등록!

```bash
$ python manage.py startapp articles
```

1. `INSTALLED_APPS ` 에 먼저 등록하고 앱을 생성하면 오류가 발생한다.

2. 일반적으로 Application 이름은 복수형으로 하는 것을 권장한다. 

<br>

| Application 구조 | 설명                                                         |
| ---------------- | ------------------------------------------------------------ |
| `__init__.py`    | python에게 이 디렉토리를 하나의 python 패키지로 다루도록 지시한다. |
| `admin.py`       | 관리자용 페이지를 설정 하는 곳이다.  (장고는 기본적으로 어드민 페이지를 제공한다. ) |
| `apps.py`        | 앱의 정보가 작성되는 곳이다.                                 |
| `models.py`      | 앱에서 사용하는 Model을 정의하는 곳이다.                     |
| `test.py`        | 프로젝트의 테스트 코드를 작성하는 곳이다.                    |
| `views.py`       | view 함수들이 정의 되는 곳이다.                              |

- 앱은 실제 요청을 처리하고 페이지를 보여주고 하는 등의 역할을 담당한다.
- 하나의 프로젝트는 여러 앱을 가진다.
- 일반적으로 앱은 하나의 기능 단위로 작성한다. 
- **MTV 중에서 M, V가 자동 생성된 것을 확인할 수 있다.** 

- **Template는 자동으로 생성되지 않는다.** 

<br>

#### (6) 앱등록

- 아직 장고 프로젝트는 이 앱이 생성된 것을 모르기 때문에 앱과 프로젝트를 연결시켜주어야 한다.

```python
# setting.py

INSTALLED_APPS = [
    # my app
    'articles',

    # 3rd-party app

    # django app 
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

1. 프로젝트에서 앱을 사용하기 위해서는 반드시 INSTALLED_APPS 리스트에 추가해야 한다. 

2. INSTALLED_APPS 리스트란 Django insallation에 활성화 된 모든 앱을 지정하는 문자열 목록이다. 

<br>

#### (7) 추가 설정

```python
# setting.py

LANGUAGE_CODE = 'ko-kr'

TIME_ZONE = 'Asia/Seoul'

USE_I18N = True

USE_L10N = True

USE_TZ = True
```

- `LAGUAGE_CODE`
  - 모든 사용자에게 제공되는 번역을 결정한다.
  - 이 설정이 적용 되려면 USE_I18N이 활성화 되어 있어야 한다.
  - http://www.i18nguy.com/unicode/language-identifiers.html
- `TIME_ZONE`
  - 데이터베이스 연결의 시간대를 나타내는 문자열을 지정한다.
  - USE_TZ가 True이고 이 옵션이 설정된 경우 데이터베이스에서 날짜 시간을 읽으면, UTC 대신 새로 설정한 시간대의 인식 날짜 & 시간이 반환 된다. 
  - USE_TZ이 False인 상태로 이 값을 설정하는 것은 error가 발생하므로 주의해야한다. 
  - https://en.wikipedia.org/wiki/List_of_tz_database_time_zones
- `USE_I18N`
  - Django의 번역 시스템을 활성화 해야 하는지 여부를 결정한다.
  -  Internationalization
- `USE_L10N`
  - 데이터의 지역화 된 형식을 기본적으로 활성화할지 여부를 지정한다.
  - True일 경우, Django는 현재 local의 형식을 사용하여 숫자와 날짜를 표시한다.
  - Localization
- `USE_TZ`
  - datetimes가 기본적으로 시간대를 인식하는지 여부를 지정한다. 
  - True일 경우 Django는 내부적으로 시간대를 인식하여 날짜/ 시간을 사용한다.  

<br>

## 3. 요청과 응답

> 코드 작성 순서: URL => View => Template

### 1) urls.py

```python
# urls.py

from django.contrib import admin
from django.urls import path
from articles import views

# path(): 사용자가 url을 통해 첫번째 인자를 호출(요청)하면 두번째 인자로 응답을 한다.  
# 장고에서는 엔드 슬래시를 붙여야 한다. 
# 장고에서는 trailing comma를 권장한다. 
# url에서는 _를 권장하지 않는다. 
urlpatterns = [
    path('admin/', admin.site.urls),
    path('index/', views.index),
    path('greeting/', views.greeting),
]
```

- **HTTP요청 (request)을 알맞은 view로 전달한다.** 
- 장고 서버로 요청(request)이 들어오면, 그 요청이 어디로 가야하는지 인식하고 관련된 함수(view)로 넘겨준다.
- `views.py` 에서 만든 함수를 연결시켜준다.

<br>

### 2) views.py

```python
# views.py

from django.shortcuts import render
import random


# view함수의 규칙 : requset는 필수 인자이다. 
# HTTP request 객체
def index(request):
    # render(): 호출되었을 때, 템플릿을 랜더링해서 HttpResponse를 return 한다. 
    # 템플릿은 자동으로 장고가 만들어 주지 않기 때문에 앱에서 생성한다. 
    # 앱이 MTV를 관장함
    return render(request, 'index.html')


def greeting(request):
    foods = ['apple', 'chicken', 'banana',]
    info = {
        'name': 'Alice',
    }
    context = { 
        'foods': foods,
        'info' : info,
    }
    return render(request, 'greeting.html', context)
```

- HTTP 요청을 수신하고 HTTP 응답을 반환하는 함수를 작성한다. 
- Model을 통해 요청에 맞는 필요 데이터(DB, context) 에 접근한다. 
- Template에게 HTTP 응답 서식을 맡긴다. 

<br>

### 3) Template

```html
<!-- articles/templates/index.html -->

 <h1>안녕하세요 ㅎㅎ</h1>

```

- `views.py`에서 지정한 `index.html` 파일을 만든다.
- 실제 내용을 보여주는데 사용되는 파일이다.
  - 데이터 표현을 제어하는 도구이자 표현에 관련된 로직이다. 
  - **즉, 유저에게 보여지는 화면, 표현이다. ( 조작과 연산은 view함수가 한다)**
- 파일 구조나 레이아웃을 정의한다.
- **Template 파일 경로의 기본 값은 app 폴더 안의 templates 폴더로 지정되어 있다.** 

<br>

#### (1) DTL

> Django Template Language
>
> Django template에서 사용하는 built-in template system

- **조건, 반복, 변수 치환, 필터 등의 기능을 제공한다.**
- 단순한 python이 HTML에 포함된 것이 아니며, 프로그래밍적 로직이 아니라 프레젠테이션을 표현하기 위한 것이다.
- python처럼 일부 프로그래밍 구조 (if, for 등) 를 사용할 수 있지만, 이것은 python 코드로 실행되는 것이 아니다.

<br>

#### (2) DTL Sytax: Variable

> render()를 사용하여 views.py에서 정의한 변수를 template 파일로 넘겨 사용하는 것이다. 

```html
{{ variable  }}
```

- 변수명은 영어, 숫자와 밑줄(`_`)의 조합으로 구성될 수 있으나 밑줄로는 시작할 수 없다. 
  - **공백이나 구두점 문자 또한 사용할 수 없다.**
- dot(.)를 사용하여 변수 속성에 접근할 수 있다.
- **view함수에서** render()의 세번째 인자로 {'key':value}와 같이 딕셔너리 형태로 넘겨주며, 여기서 정의한 key에 해당하는 문자열이 template에서 사용가능한 변수명이 된다.  

<br>

#### (3) DTL Sytax: Filters

:pencil2: 참고자료: https://docs.djangoproject.com/en/4.0/ref/templates/builtins/

```html
{{ variable|filter}}

<!-- chained가 가능하다 --> 
{{ variable|filter|filter }}

<!-- 일부 필터는 인자를 받기도 한다 --> 
{{ variable|truncatewords:30 }}
```

- 표시할 변수를 수정할 때 사용한다. 
- 60개의 built-in template filters를 제공한다
- chained가 가능하며, 일부 필터는 인자를 받기도 한다. 

<br>

#### (4) DTL Sytax: Tags

```html
{% tag %}

<!-- 일부태그는 시작과 종료 태그가 필요하다 -->
{% if %}{% endif %}
{% for %}{% endfor %}
```

- 출력 텍스트를 만들거나, 반복 또는 논리를 수행하여 제어 흐름을 만드는 등 변수보다 복잡한 일들을 수행한다. 
- 일부태그는 시작과 종료태그가 필요하다.
- 약 24개의 built-in-template tags를 제공한다.

<br>

#### (5) DTL Sytax: Comments

```html
{# 한 줄 주석 #}

{% comment %}
  여러 줄 주석
  여러 줄주석
{% endcomment %}

<!-- 주석 -->
```

- Django template에서 라인의 주석을 표현하기 위해 사용한다.

<br>

```python
# views.py

from django.shortcuts import render

def greeting(request):
    foods = ['apple', 'chicken', 'banana',]
    info = {
        'name': 'Alice',
    }
    context = { 
        'foods': foods,
        'info' : info,
    }
    return render(request, 'greeting.html', context)
```

```html
<!-- articles/templates/greeting.html -->
<p>안녕하세요 저는 {{ info.name|lower }}입니다.</p>
<p>제가 가장 좋아하는 음식은 {{ foods }}입니다.</p>
<p>저는 사실 {{ foods.0 }}을 가장 좋아합니다.</p>
```

- `.`을 이용하여 변수의 속성에 접근할 수 있다. 
  - 변수의 값이 리스트인 경우 인덱스로 접근할 수 있다. 

<br>

```python
# views.py

from django.shortcuts import render
import random

def dinner(request):
    foods = ['족발', '햄버거', '치킨', '초밥']
    pick = random.choice(foods)
    context = {
        'pick' : pick,
        'foods' : foods,
    }
    return render(request, 'dinner.html', context)
```

```html
<!-- articles/templates/dinner.html -->


<h1>오늘 저녁은 {{ pick }} 이다! </h1>
<p>{{ pick }}는 {{ pick|length }} 글자 이다.</p>
<p>{{ foods|join:','}}</p>

<p>메뉴판</p>
<ul>
  {% for food in foods %}
    <li>{{ food }}</li>
  {% endfor %}
</ul>

<!-- 주석은 여러개 -->

{# 주석은 여러개. #}

{% comment %} 
주석은 여러개 
{% endcomment %}
```

- 모듈을 추가할 수 있다. 

<br>

```python
# views.py

from django.shortcuts import render

def dtl_practice(request):
    foods = ['짜장면', '탕수육', '짬뽕', '양장피']
    fruits = ['apple', 'banana', 'cucumber', 'mango']
    user_list = []
    context = {
        'foods': foods,
        'fruits': fruits,
        'user_list': user_list,
    }
    return render(request, 'dtl_practice.html', context)
```

```html
<!-- articles/templates/dtl_practice.html -->

  <h3>1. for</h3>
  {% for food in foods %}
    <p>{{ food }}</p>
  {% endfor %}
  <hr>

<!-- forloop.counter : 루프가 입력된 횟수를 표시한다. 마다 인덱스를 생성한다.(1부터 시작) -->
  {% for food in foods %}
    <p>{{ forloop.counter }} {{ food }}</p> 
  {% endfor %}
  <hr>

  {% for user in empty_list %}
    <p>{{ user }}</p>
  {% empty %}
    <p>지금 가입한 유저가 없습니다.</p>
  {% endfor %}
  <hr>


  <h3>2. if</h3>

  {% if not empty_list %}
    <p>지금 가입한 유저가 없습니다.</p>
  {% else %}
    <p>지금 가입한 유저가 있습니다.</p>
  {% endif %}
  <hr>

  {% if '짜장면' in foods %}
    <p>짜장면엔 고추가루지 !</p>
  {% endif %}
  <hr>

<!-- forloop.first: 루프 처음 실행 시 True로 설정 -->
<!-- forloop.last: 루프 마지막 통과 시 True로 설정 -->
  {% for food in foods %}
    {% if forloop.first %}
      <p>짜장면+고추가루</p>
    {% else %}
      <p>{{ food }}</p>
    {% endif %}
  {% endfor %}
  <hr>

  <p>3. length filter 활용</p>
  {% for fruit in fruits %}
    {% if fruit|length > 5 %}
      <p>이름이 너무 길어요.</p>
    {% else %}
      <p>{{ fruit }}, {{ fruit|length }}</p>
    {% endif %}
  {% endfor %}
  <hr>

  <h3>4. lorem ipsum</h3>
  <!-- {% lorem [count] [method] [random] %} --> 
  {% lorem %}
  <hr>
  {% lorem 3 w %}
  <hr>
  {% lorem 4 w random %}
  <hr>
  {% lorem 2 p %}
  <hr>

  <h3>5. 글자 관련 필터</h3>
  <p>{{ 'ABC'|lower }}</p>          <!-- 소 문자 --> 
  <p>{{ my_sentence|title }}</p>    <!-- 단어가 시작되는 첫번째 글자만 대문자 -->
  <p>{{ foods|random }}</p>         <!-- 변수가 리스트이면 random으로 하나만 뽑는다. -->
  <hr>

  <h3>6. 연산</h3>
  <p>{{ 4|add:6 }}</p>
  <hr>

  <h3>7. 다양한 날짜 표현</h3>
  {% now "DATETIME_FORMAT" %}<br>
  {% now "SHORT_DATETIME_FORMAT" %}<br>
  {% now "DATE_FORMAT" %}<br>
  {% now "SHORT_DATE_FORMAT" %}
  <hr>
  {% now "Y년 m월 d일 (D) h:i" %}
  <hr>
  {% now "Y" as current_year %}
  Copyright {{ current_year }}
  <hr>

  <a href="/index/">back</a>
```

<br>

### 4) Template inheritance (템플릿 상속)

> 템플릿 상속을 사용하면 사이트의 모든 공통 요소를 포함하고, 
>
> 하위 템플릿이 재정의(override) 할 수 있는 블록을 정의하는 기본 'skeleton' 템플릿을 만들 수 있다. 

- 템플릿 상속은 기본적으로 코드의 재사용성에 초점을 맞췄다. 

#### (1) base template 만들기

- `app_name> templates`에 만들지 않는다. (구분을 위해)
- Django 프로젝트를 가지고 있는 가장 최상위 폴더에 `templates` 디렉토리를 생성한 후 , `템플릿 이름.html`을 작성한다. 

<br>

#### (2) template 경로를 `settings.py`에 기입

>  프로젝트에게 templates이 위치한 경로를 제공해주어야 한다. 
>
> (articles> templates는 기본이기 때문에 따로 설정할 필요가 없었음)

```python
# settings.py

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        
        # 이 경로에도 템플릿 파일이 있을 거야! (추가 경로를 알려준 것이다.)
        # 운영체제마다 경로시스템이 다르기때문에 운영체제 상관없이 맞추기 위해...
        # python object oriented filesystem path
        'DIRS': [BASE_DIR / 'templates',],
       
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```
- `'DIRS'`속성에 `[BASE_DIR / '폴더 이름',]` 을 기입한다.  

<br>

```python
# settings.py

# Build paths inside the project like this: BASE_DIR / 'subdir'.
# 장고 프로젝트를 가지고 있는 최상단 폴더
# 경로 추가할 때 시작점을 변수로 미리 잡아준 것!
BASE_DIR = Path(__file__).resolve().parent.parent
```

- `path(__file__)`은 해당 파일의 위치를 알려 준다. 
- `resolve()`물리적인 경로가 숨겨져 있을 경우, 진짜 물리 경로를 찾아준다. (ex. 리눅스의 심볼릭 링크, 윈도우의 바로가기)

**=> 즉,  `settings.py`의 절대경로를 뽑아낸다.** 

**=> `settings.py`의 절대경로의 `.parent.parent`(부모의 부모)는 Django프로젝트를 가지고 있는 최상단 폴더 이다.** 

<br>

#### (3) skeleton template 기입 

- 원하는 공통 요소를 기입할 수 있다. 

- 하위 템플릿에서 재지정(오버라이드)할 수 있는 블록을 정의 할 수 있다.

  ```html
    <!--자식 템플릿이 가져다가 사용하는 블럭이다. (블럭에 이름이 있어야한다) -->
    <!-- 즉, 하위 템플릿이 채울수 있는 공간이다. -->
    {% block 이름 %}
    {% endblock 이름 %}
  ```

- 템플릿을 로드하고 현재 페이지로 렌더링 할 수 있다. (base. html도 충분히 무거워질 수 있기 때문에 이를 방지하기 위함)

  ```html
    <!-- 템플릿 내에 다른 템플릿을 포함하는 방법이다.-->
    <!-- 템플릿을 퍼즐처럼 사용한다. -->
    <!-- 일반적으로 include되는 템플릿은 앞에 _를 붙여서 일반 템플릿과 구분이 쉽게 한다. -->
    {% include '템플릿 이름' %}
  ```

<br>

#### (4) skeleton template 적용

```html
{% extends 'skeleton template 이름' %}
```

- 자식(하위)템플릿이 부모 템플릿을 확장한다는 것을 알린다.
- 반드시 템플릿 최상단에 작성되어야 한다. 

<br>

```html
<!-- templates > base.html -->

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-1BmE4kWBq78iYhFldvKuhfTAU6auU8tT94WrHftjDbrCEXSU1oBoqyl2QvZ6jIW3" crossorigin="anonymous">
  <title>Document</title>
</head>
<body>

  <!-- _를 앞에 붙이는 템플릿은 인클루드 되는 템플릿이구나! -->
  {% include '_nav.html' %}

  <!--자식 템플릿이 가져다가 사용하는 블락-->
  {% block content %}
  {% endblock content %}

  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-ka7Sk0Gln4gmtz2MlQnikT1wXgYsOg+OMhuP+IlRH9sENBO0LRn5q+8nbTov4+1p" crossorigin="anonymous"></script>
</body>
</html>
```

```html
<!-- articles > templates > _nav.html -->

<ul class="nav">
  <li class="nav-item">
    <a class="nav-link active" aria-current="page" href="#">Active</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" href="#">Link</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" href="#">Link</a>
  </li>
  <li class="nav-item">
    <a class="nav-link disabled">Disabled</a>
  </li>
</ul>
```

```html
<!-- articles > templates > greeing.html -->

{% extends 'base.html' %}

{% block content %}
<p>안녕하세요 저는 {{ info.name|lower }}입니다.</p>
<p>제가 가장 좋아하는 음식은 {{ foods }}입니다.</p>
<p>저는 사실 {{ foods.0 }}을 가장 좋아합니다.</p>
{% endblock content %}
```

<br>

> Django template system (feat. Django 설계 철학)

- 표현과 로직(viesw)을 분리한다.
  - 템플릿 시스템은 표현을 제어하는 도구이자 표현에 관련된 로직일 뿐이라고 생각한다. 
  - 즉, 템플릿 시스템은 이러한 기본 목표를 넘어서는 기능을 지원하지 말아야 한다.
- 중복을 배제한다.
  - 대다수의 동적 웹사이트는 공통 header, footer, navbar 같은 사이트 공통 디자인을 갖는다.
  - Django 템플릿 시스템은 이러한 요소를 한 곳에 저장하기 쉽게 하여 중복 코드를 없애야 한다.
  - 이것이 템플릿 상속의 기초가 되는 철학이다.

<br>

## 3. form

> 웹에서 사용자 정보를 입력하는 여러 방식을 제공하고 사용자로부터 할당된 데이터를 서버로 전송하는 역할을 담당한다.
>
> text, button, checkbox, file, hidden, image, password, radio, reset, submit

### 1) 주요 속성

| attribute | value      | 설명                                     |
| --------- | ---------- | ---------------------------------------- |
| action    | URL        | 입력 데이터(form data)가 전송될 url 지정 |
| method    | get / post | 입력 데이터(form data) 전달 방식 지정    |

<br>

#### (1) HTTP

- HyperText Transfer Protocol
- 웹에서 이루어지는 모든 데이터 교환의 기초
- 주어진 리소스가 수행 할 작업을 나타내는 request methods를 정의한다.
- HTTP request method 종류
  - GET, POST, PUT, DELETE ...

<br>

#### (2) HTTP request method 

`GET` 

- **서버로부터 정보를 조회하는 데 사용한다. 데이터를 가져올 때만 사용해야 한다.**  
- 데이터를 서버로 전송할 때, 전송 URL에 입력 데이터를 쿼리스트링으로 보내는 방식이다.
  - 우리는 서버에 요청을 하면 HTML 문서 파일 한 장을 받는데, 이 때 사용하는 요청의 방식이 GET이다.
  - ex) http://jsonplaceholder.typicode.com/posts?userId=1&id=1

- **전송 URL 바로 뒤에 ‘?’를 통해 데이터의 시작을 알려주고, key-value형태의 데이터를 추가한다. 1개 이상의 전송 데이터는 ‘&’로 구분한다.**
- URL에 전송 데이터가 모두 노출되기 때문에 보안에 문제가 있으며 전송할 수 있는 데이터의 한계가 있다. (최대 255자).
- REST API에서 GET 메소드는 모든 또는 특정 리소스의 조회를 요청한다

<br>

`POST`

- 데이터를 서버로 전송할 때,  Request Body에 담아 보내는 방식이다.
  - ex) http://jsonplaceholder.typicode.com/posts

- URL에 전송 데이터가 모두 노출되지 않지만 GET에 비해 속도가 느리다.
- REST API에서 POST 메소드는 특정 리소스의 생성을 요청한다.

<br>

### 2) input 태그

>  **form을 입력하는 태그 중에서 가장 중요한 태그로 사용자로부터 데이터를 입력받기 위해 사용된다.**  
>
> **input 태그에 입력된 데이터가 form 태그의 method 어트리뷰트에 지정된 방식으로 action 어트리뷰트에 지정된 서버측의 처리 로직에 전달된다.**

<br>

- type 속성에 따라 동작 방식이 달라진다.
  - input 태그는 다양한 종류가 있는데 type 속성에 의해 구분된다.
  - type 속성에 따라 동장 방식이 달라진다. 
- **서버에 전송되는 데이터는 name 어트리뷰트를 키로, value 어트리뷰트를 값으로하여 key=value의 형태로 전송된다.** 
  - 사용자 입력값이 그냥 value인 경우도 있다. (value속성은 필수가 아님!)

| 핵심 속성 (attribute) | 설명                                                         |
| --------------------- | ------------------------------------------------------------ |
| `name`                | 중복이 가능하다, 양식을 제출 했을 때 **name**이라는 이름에 설정된 값을 넘겨서 값을 가져올 수 있다. <br />주요 용도는 GET/POST 방식으로 서버에 전달되는 파라미터 (name은 key, value는 value)로 매핑하는 것이다.<br />GET 방식에서 URL에서 ?key=value&key=value 형식으로 데이터를 전달한다. |

:pencil2: 참고자료: https://developer.mozilla.org/ko/docs/Web/HTML/Element/Input 

<br>

### 3) label

> 사용자 인터페이스 항목에 대한 설명(caption)을 나타낸다. 

- label을 클릭하여 input자체의 초점을 맞추거나 활성화 시킬 수 있다.
- 사용자는 선택할 수 있는 영역이 늘어나 웹/ 모바일 (터치) 환경에서 편하게 사용할 수 있음.
- label과 input 입력의 관계가 시각적 뿐만 아니라 화면 리더기에서도 label을 읽어 쉽게 내용을 확인할 수 있도록 함.

<br>

#### (1) label을 input 요소와 연결

- input에 `id 속성`을 부여한다. 
- label에는 input의 id와 동일한 값의 `for 속성`이 필요하다.  
  - for 속성의 값과 일치하는 id를 가진 문서의 첫 번째 요소를 제어한다. 
  - 연결된 요소가 lablel able elements인 경우 이 요소에 대한 labele control이 된다. 

<br>

`label able elements`

label 요소와 연결할 수 있는 요소이다.

예시) button, input(not hidden type), select, textarea 등

<br>

```python
# urls.py

from django.urls import path
from . import views

urlpatterns = [
    path('throw/', views.throw),
    path('catch/', views.catch),
]
```

```python
# articles > views.py

from django.shortcuts import render

def throw(request):
    return render(request, 'throw.html')

def catch(request):
    # messeage를 키를 갖는 데이터를 호출하면, 함수가 실행된다
    message = request.GET.get('message')
    context = {
        'message': message
    }
    return render(request, 'catch.html', context)
```

```html
<!--articles > templates > throw.html -->

{% extends 'base.html' %}

{% block content %}
  <h1>Throw</h1>
  <!-- 쿼리스트링: /?message=입력값-->
  <!-- 즉, form을 제출하면, name 속성의 값인 messeage를 key로 입력 값을 value로 하는 한 쌍의 데이터를 서버에 보낸다 -->
  <form action="/catch/" method="GET">
    <label for="message">Throw</label>
    <input type="text" id="message" name="message">
    <input type="submit" value="던질게요!!">
  </form>
{% endblock content %}
```

```html
<!--articles > templates > catch.html -->

{% extends 'base.html' %}

{% block content %}
  <h1>Catch!</h1>
  <h2>그만 던져!! {{ message }}를 받았어!!</h2>
  <a href="/throw/">한번 더!</a>
{% endblock content %}
```

<br>

## 4. URL

> Dispatcher(발송자, 운항 관리자)로서의 URL
>
> 웹 애플리케이션은 URL을 통한 클라이언트의 요청에서부터 시작된다. 

### 1) Variable Routing

- url 주소를 변수로 사용하는 것
- url의 일부를 변수로 지정하여 view 함수의 인자로 넘길 수 있다. 
- 즉, 변수 값에 따라 하나의 path()에 여러 페이지를 연결 시킬 수 있다. 

<br>

#### (1) URL Path converters

```python
# urls.py

from django.urls import path
from articles import views

urlpatterns = [
    path('hello/<str:name>/', views.hello),
]
```

- **str**
  - **`/`를 제외하고 비어 있지 않은 모든 문자열과 매치한다**
  - 작성하지 않을 경우 기본 값이다. 
- **int**
  - 0 또는 양의 정수와 매치 
- **slug**
  - ASCII 문자 또는 숫자, 하이픈 및 밑줄 문자로 구성된 모든 슬러그 문자열과 매치한다. 
  - ex) 'building-your-1st-django-site'

<br>

#### (2) 실습

```python
# articles > views.py

from django.shortcuts import render

# /hello/str으로 호출되면 실행되는 함수
# /hello만 호출하면 실행되지 않는다. 
def hello(request, name):
    context = {
        'name': name, 
    }
    return render(request, 'hello.html', context)
```

```html
<!-- articles > templates > hello.html -->

{% extends 'base.html' %}

{% block content %}
  <p>만나서 반갑습니다. {{ name }}님!!</p>
{% endblock content %}
```

<br>

### 2) App URL mapping

> app의 view 함수가 많아지면서 사용하는 path() 또한 많아지고, 
>
> app 또한 더 많이 작성되기 때문에 프로젝트의 urls.py에서 모두 관리하는 것은 프로젝트 유지보수에 좋지 않다. 

**이제는 각 app에 urls.py를 작성하자**

```python
# project > urls.py

from django.contrib import admin
from django.urls import path, include

# ~ dinner에서 ~/articles/dinner로 처리한다. 
# app별로 관리한다.
urlpatterns = [
    path('admin/', admin.site.urls),
    path('articles/', include('articles.urls')),
    path('pages/', include('pages.urls')),
]
```

```python
# articles > urls.py

from django.urls import path

#django는 명시적 상대경로를 권장한다! 
from . import views

urlpatterns = [
    path('index/', views.index),
    path('greeting/', views.greeting),
    path('dinner/', views.dinner),
    path('dtl-practice/', views.dtl_practice),
    path('throw/', views.throw ),
    path('catch/', views.catch ),
    path('hello/<str:name>/', views.hello),
]
```

- `include()`
  - 다른 URLconf(app1/urls.py)들을 참조할 수 있도록 도운다.
  - 함수 include()를 만나게 되면, URL의 그 시점까진 일치하는 부분을 잘라내고, 남은 문자열 부분을 후속 처리를 위해 include된 URLconf로 전달한다. 

<br>

<img src="https://user-images.githubusercontent.com/72687619/156505992-c8e1073e-0935-43b2-9285-0638adb90ef7.png" style="zoom: 50%;" >



<br>

### 3) Naming URL patterns

> 이제는 링크에 url을 직접 작성하는 것이 아니라 path ()함수의 name인자를 정의해서 사용한다. 
>
> 이는 Django Template Tag 중 하나인 url 태그를 사용해서 path() 함수에 작성한 name을 사용할 수 있다. 
>
> 그리고 app이 여러개일 경우를 대비해 app_name 변수를 활용한다. 

```python
# articles > ulrs.py

from django.urls import path
from . import views

app_name = 'atricles'
urlpatterns = [
    path('index/', views.index, name='index'),
    path('greeting/', views.greeting, name= 'greeting'),
    path('dinner/', views.dinner, name= 'dinner'),
    path('dtl-practice/', views.dtl_practice, name='dtl_practice'),
    path('throw/', views.throw, name='throw'),
    path('catch/', views.catch, name='catch'),
    path('hello/<str:name>/', views.hello),
]
```

```html
<!-- articles > templates > articles > index -->

{% extends 'base.html' %}

{% block content %}
<h1>안녕하세요 ㅎㅎ</h1>
<a href="/articles/greeting/">Greeting</a>
<a href="{% url 'articles:dinner' %}">Dinner</a>
<a href="{% url 'articles:dtl_practice' %}">Dtl-practice</a>
<a href="{% url 'articles:throw' %}">Throw</a>

<!-- 만약 다른 앱으로 이동하고 싶다면...?-->
<a href= "{% url 'pages:index' %}">Pages Index</a>
{% endblock content %}
```

```html
<!-- articles > templates > articles > throw -->

{% extends 'base.html' %}

{% block content %}
  <h1>Throw</h1>
  <form action="{% url 'articles:catch' %}" method="GET">
    <label for="message">Throw</label>
    <input type="text" id="message" name="message">
    <input type="submit" value="던질게요!!">
  </form>
{% endblock content %}
```

- url 설정에 정의된 특정한 경로들의 의존성을 제거할 수 있다. 
- `{% url '' %}`
  - 주어진 url 패턴 이름 및 선택적 매개 변수와 일치하는 절대 경로 주소를 반환한다.
  - 템플릿에 url을 하드 코딩하지 않고도 DRY원칙을 위반하지 않으면서 링크를 출력하는 방법이다.

<br>

### 4) name space

**`그런데 만약 앱이 여러개라면...?`**

- `아티클 앱 `: articles > templates >  index 

- `페이지 앱`: pages > templages > index

**views.py에서 index.html을 가져올때 혼란스럽지 않을까...?**

```python
from django.shortcuts import render

def index(request):
    return render(request, 'pages/index.html')  # articles 앱의 index 페이지를 가져옴!
```

- 앱이 여러개 생기면 발생하는 문제이다. (이름 규칙)
- django가 template를 찾을 때, settings.py 에 추가로 설정한 템플릿 경로를 찾는다.
- 그 다음에 app폴더에서 찾는다. (settings.py에 등록되어 있는 앱순서대로..)
- **현재 article앱이 pages보다 위에 기입되어 있기 때문에  aritcles의 template에서 읽어오게 되는 것이다.** 

<br>



#### (1) 정의

`namspace` : 이름공간은 객체를 구분할 수 있는 범위를 나타내는 말로 일반적으로 하나의 이름공간에는 하나의 이름이 단 하나의 객체를 가르키게된다.

- 프로그래밍을 하다 보면 모든 변수명과 함수명 등 이들 모두를 겹치지 않게 정의 하는 것은 매우 어렵다.
- **서로 다른 app의 같은 이름을 가진 url name은 이름공간을 설정해서 구분한다.**
- **templates, static등 django는 정해진 경로 하나로 모아서 보기 때문에 중간에 폴더를 임의로 만들어 줌으로써 이름공간을 설정한다.** 

<br>

#### (2) 활용

>  Djang는 기본적으로 `app_name/templates`경로에 있는 templates 파일들만 찾을 수 있으며, 
>
> `INSRALLED_APPS`에 작성된 pp순서로 template을 검색 후 랜더링 한다. 

<br>

**임의로 templates의 폴더 구조를 `app_name/templates/app_name`형태로 변경해 임의로 이름 공간을 생성 후 변경된 추가 경로로 수정한다.** 

<br>

[이전] pages > templages > index. html

[개선] pages > templates > pages > index.html

```python
from django.shortcuts import render

def index(request):
    return render(request, 'pages/index.html')

```

<br>

## 5. static

### 1) 웹서버와 정적 파일

#### (1) 웹서버와 정적 파일

- 웹 서버는 특정위치(URL)에 있는 자원(resource)을 요청(HTTP request)받아서 제공(serving)하는 응답(HTTP response)을 처리하는 것을 기본 동작으로 한다.
- 이는 자원과 접근 가능한 주소가 정적으로 연결된 관계 이다.
  - 예를 들어, 사진 파일은 자원이고 파일 경로는 웹주소라 한다.
- 즉, 웹 서버는 요청 받은 URL로 서버에 존재하는 정적 자원(static resource)를 제공한다. 

<br>

#### (2) Static files

- 정적 파일
- 응답할 때 별도의 처리 없이 파일 내용을 그대로 보여주면 되는 파일
- 사용자의 요청에 따라 내용이 바뀌는 것이 아니라 요청한 것을 그대로 보여주는 파일
- 예를 들어, 웹 사이트는 일반적으로 이미지, 자바 스크립트 또는 CSS와 같은 미리 준비된 추가 파일(움직이지 않는)을 제공해야 함
- Django에서는 이러한 파일들을 “static file”이라 함
- https://docs.djangoproject.com/en/3.1/howto/static-files/#managing-static-files-e-g-images-javascript-css

<br>

### 2) Static files 구성

1. django.contrib.staticfiles 앱이 `INSTALLED_APPS`에 있는지 확인
2. setting.py에 `STATIC_URL` 정의
3. 템플릿에서 static 템플릿 태그를 사용하여 static file이 있는 상대경로를 빌드
4. 앱에 static file 저장하기 (`my_app/static/my_app/sample.jpg`)

<br>

#### (1) Django template tag

- `load`
  - 사용자 정의 템플릿 태그 세트를 로드
  - 로드하는 라이브러리, 패키지에 등록된 모든 태그와 필터를 로드
  - 빌트인 태그가 아니기때문에 로드해야 한다. 
- `static`
  - STATIC_ROOT에 저장된 정적 파일에 연결

- 이미지 파일 위치 - `articles/static/articles/images/`
- static file 기본 경로
  - `app_name/static/`

<br>

### 2) The staticfiles app

> https://docs.djangoproject.com/en/3.1/ref/contrib/staticfiles/#module-django.contrib.staticfiles

**`STATICFILES_DIRS`**

```python
STATICFILES_DIRS = [
    BASE_DIR / 'static',
]
```

- app/static/ 디렉토리 경로를 사용하는 것(기본 경로) 외에 추가적인 정적 파일 경로 목록을 정의하는 리스트
- 추가 파일 디렉토리에 대한 전체 경로를 포함하는 문자열 목록으로 작성되어야 함
- 즉, static의 추가경로이다.

<br>

**`STATIC_URL`**

```python
STATIC_URL = '/static/'
```

- STATIC_ROOT에 있는 정적 파일을 참조 할 때 사용할 URL
- 개발 단계에서는 실제 정적 파일들이 저장되어 있는 app/static/ 경로 (기본 경로) 및 STATICFILES_DIRS에 정의된 추가 경로들을 탐색함
- 실제 파일이나 디렉토리가 아니며, URL로만 존재 비어 있지 않은 값으로 설정 한다면 반드시 slash(/)로 끝나야 함

<br>

**`STATIC_ROOT`**

- collectstatic이 배포를 위해 정적 파일을 수집하는 디렉토리의 절대 경로
- django 프로젝트에서 사용하는 모든 정적 파일을 한 곳에 모아 넣는 경로
- 개발 과정에서 setting.py의 DEBUG 값이 True로 설정되어 있으면 해당 값은 작용되지 않음
- 직접 작성하지 않으면 django 프로젝트에서는 setting.py에 작성되어 있지 않음
- 실 서비스 환경(배포 환경)에서 django의 모든 정적 파일을 다른 웹 서버가 직접 제공하기 위함

> [참고] **collectstatic**
>
> - 프로젝트 배포 시 흩어져있는 정적 파일들을 모아 특정 디렉토리로 옮기는 작업
>
> ```python
> # settings.py 예시
> 
> STATIC_ROOT = BASE_DIR / 'staticfiles'
> $ python manage.py collectstatic
> ```

<br>

### 3) static file 사용하기

#### (1) 기본경로

- `article/static/articles/` 경로에 이미지 파일 위치

  ```jinja2
  <!-- articles/index.html -->
  
  {% extends 'base.html' %}
  {% load static %}
  
  {% block content %}
    <img src="{% static 'articles/sample.png' %}" alt="sample">
    ...
  {% endblock %}
  ```

- 이미지 파일 위치 - `articles/static/articles/`

- static file 기본 경로

  - `app_name/static/`

<br>

#### (2) 추가 경로

- `static/` 경로에 CSS 파일 위치

```jinja2
<!-- base.html -->

<head>
  {% block css %}{% endblock %}
</head>


# settings.py

STATICFILES_DIRS = [
    BASE_DIR / 'static',
]


<!-- articles/index.html -->

{% extends 'base.html' %}
{% load static %}

{% block css %}
  <link rel="stylesheet" href="{% static 'style.css' %}">
{% endblock %}
/* static/style.css */

h1 {
    color: crimson;
}
```

<br>

