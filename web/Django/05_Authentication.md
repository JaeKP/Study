# Django Authentication

[toc]

## 1. Authentication System

Django 인증 시스템은 `django.contrib.auth`에 Django contrib module로 제공한다. 

필수 구성은 settings.py에 이미 포함되어 있으며 INSTALLED_APPS 설정에 나열된 아래 두 항목으로 구성된다. 

- `django.contrib.auth`: 인증 프레임워크의 핵심과 기본 모델을 포함한다. 
- `django.contrib.contenttypes`: 사용자가 생성한 모델과 권한을 열견할 수 있다.



또한, Django는 Authentication(인증)과 Authorization(권한)부여를 함께 처리한다. 

- Authentication: 신원 확인, 사용자가 자신이 누구인지 확인하는 것
- Authorization: 권한 부여, 인증된 사용자가 수행할 수 있는 작업을 결정한다. 



`인증 관련 앱 등록하기`

```bash
python manage.py startapp accounts	
```

app 이름이 반드시 accounts일 필요는 없지만, auth와 관련해 Django 내부적으로 accounts라는 이름으로 사용되고 있기 떄문에 account로 지정하는 것을 권장한다.

<br>

## 2. 쿠키와 세션 

HTTP란 Hyper Text Transfer Protocol의 줄임말이다.

- HTML 문서와 같은 리소스(자원, 데이터)들을 가져올 수 있도록 해주는 프로토콜(규칙, 규약)
- 웹에서 이루어지는 모든 데이터 교환의 기초
- 클라이언트 - 서버 프로토콜이기도 하다.



`비연결지향 (connectionless)`

서버는 클라이언트의 요청에 대한 응답을 보낸 후 연결을 끊는다.



`무상태 (stateless)`

연결을 끊는 순가 클라이언트와 서버 간의 통신이 끝나며 클라이언트의 상태 정보가 유지되지 않는다.

클라이언트와 서버가 주고 받는 메시지들은 서로 완전히 독립적이다.



하지만, 클라이언트와 서버간의 관계를 지속해야 하는 경우가 있다. (자동 로그인, 장바구니 등 )

또한 연속되지 않으면 서버는 클라이언트가 누구인지 계속 인증을 해야 한다. 

**그래서 클라이언트와 서버의 지속적인 관계를 유지하기 위해 쿠키와 세션이 존재한다.**



### 1) 쿠키

> 서버가 사용자의 웹브라우저에 전송하는 작은 데이터 조각이다.

- 사용자가 웹사이트를 방문할 경우 해당 웹사이트의 서버를 통해 사용자의 컴퓨터에 설치되는 작은 기록 정보 파일이다.
  - 브라우저(클라이언트)는 쿠키를 로컬에 KEY-VALUE의 데이터 형식으로 저장한다.
  - **이렇게 쿠키를 저장해 놓았다가, 동일한 서버에 재 요청 시 저장된 쿠키를 함께 전송한다.**
- HTTP 쿠키는 상태가 있는 세션을 만들어 준다. 
- 쿠키는 두 요청이 동일한 브라우저에서 들어왔는지 아닌지를 판단할 때 주로 사용한다.
  - 이를 이용해 사용자의 로그인 상태를 유지할 수 있다.
  - 상태가 없는 HTTP 프로토콜에서 상태 정보를 기억시켜주기 때문이다.

- 쿠키는 소프트웨어가 아니기때문에 프로그램처럼 실행 될 수 없으며 악성코드를 설치할 수 없지만, 사용자의 행동을 추적하거나 쿠키를 훔쳐서 해당 사용자의 계정 접근 권한을 획득 할 수도 있다. 

**즉, 웹 페이지에 접속하면 요청한 웹페이지를 받으며 쿠키를 저장하고, 클라이언트가 같은 서버에 재 요청 시 요청과 함께 쿠키도 전송한다.** 

1. **클라이언트가 페이지를 요청** 
2. **웹 서버는 쿠키를 생성하고 응답과 함께 준다. (set-Cookie 응답 헤더)**
3. **받은 쿠키를 클라이언트가 로컬 PC에 저장한다.** 
4. **후에, 같은 서버에 재 요청할 때 쿠키가 있다면 함께 전송한다. (Cookie HTTP 헤더**)

<br>

`사용 목적`

- 세션 관리: 로그인, 아이디 자동 완성, 공지 하루 안보기, 팝업 체크, 장바구니 등의 정보를 관리한다.
- 개인화: 사용자 선호, 테마 등의 설정한다.
- 트래킹; 사용자 행동을 기록 및 분석한다.

<br>

`수명`

- Session cookies: 현재 세션이 종료되면 삭제 된다.
  - 브라우저가 현재 세션이 종료되는 시기를 정의한다.
  - 일부 브라우저는 다시 시작할 때 세션 복원을 사용해 세션 쿠키가 오래 지속 될 수 있도록 한다.
- Persistent cookies
  - Expires 속성에 지정된 날짜 혹은 Max-age 속성에 지정된 기간이 지나면 삭제된다. 

<br>

### 2) 세션

> 사이트와 특정 브라우저 사이의 상태(state)를 유지 시키는 것이다.
>
> (서버와 클라이언트가 연결된 상태!)

- 클라이언트가 서버에 접속하면 서버가 특정 session id를 발급하고, 클라이언트는 발급 받은 session id를 쿠키에 저장한다.
  - 클라이언트가 다시 서버에 접속하면 요청과 함께 session id가 저장된 쿠키를 서버에 전달한다.
  - 쿠키는 요청 때마다 서버에 함께 전송되므로 서버에서 session id를 확인해 알맞은 로직을 처리한다.
- ID는 세션을 구별하기 위해 필요하며, 쿠키에는 ID만 저장한다.

<br>

`쿠키와 세션`

장고 내부에 sessionid와 같은 특정 값을 저장 해서 쿠키요청하는 사람과 비교한다.

저장, 만료시기 같은 것들이 DB나 메모리에 저장된다.  

즉, 쿠키랑 비슷하지만, login같은 것들을 할 때는 쿠키가 아니라 세션에 저장을 해서 검증을 한번 더 하는 것이다.

(쿠키 자체가 오염도어 있는 경우가 있기 때문에 100%확신이 불가능하다. 따라서 검증을 위해, 만료기간이나 키를 담아두는 세션이 필요한 것이다.)

<br>

`Django`

- Django의 세션은 미들웨어를 통해 구현된다.
  - `SessionMiddleware` 요청 전반에 걸쳐 세션을 관리한다.
  - `AuthenticationMiddleware` 세션을 사용하여 사용자를 요청과 연결한다.
- Database- backend sessions 저장방식을 기본 값으로 사용한다.
  - 설정을 통해 cached, file-based, cookie-based 방식으로 변경이 가능하다.
- **Django는 특정 session id를 포함하는 쿠키를 사용해서 각각의 브라우저와 사이트가 연결된 세션을 알아낸다.**
  - **세션 정보는 Django DB의 django_session 테이블에 저장된다.**
- 모든 것을 세션으로 사용하려고 하면, 사용자가 많을 대 서버에 부하가 걸릴 수 있다.



> MIDDLEWARE?

HTTP 요청과 응답 처리 중간에서 작동하는 시스템이다. (hook)

Django는 HTTP 요청이 들어오면 미들웨어를 거쳐 해당 URL에 등록되어 있는 view로 연결해주고, HTTP 응답 역시 미들웨어를 거쳐서 내보낸다.

주로 데이터 관리, 애플리케이션 서비스, 메시징, 인증 및 API 관리를 담당한다.

<br>

## 3. 로그인과 로그아웃

**로그인과 로그아웃은 Session을 C**RU**D하는 것이다.**

로그인: Django-session table에 create!

로그아웃: Django-session table에 delete! 

<br>

### 1) 로그인

> 세션 create!

<br>

#### (1) AuthenticationForm(request)

https://docs.djangoproject.com/en/4.0/topics/auth/default/#module-django.contrib.auth.forms

- 사용자를 로그인 하기위해 빌트인 폼을 사용한다.
- 첫번째 인자로 request를 사용한다.  
  - 일반 Form이다. (forms.Form에게 상속을 받는다.)
  - 즉,  모델에게 받는 정보가 없기 때문에  request가 필수 매개 변수이다. 
  - 하위 클래스에서 사용할 수 있도록 form 인스턴스에 저장된다. 
- **DB에 데이터를 저장할 필요가 없기 때문에, model과 관련이 없는 Form이다.** 
  - **단지, login 정보를 받아서 장고 session이라는 DB에 저장을 하기위한 도구이다.**
  - **여기서만` form.get_user() `메서드를 사용할 수 있다.** 


<br>

#### (2) login(request, user, backend=None )

- 현재 세션에 연결하려는 **인증된 사용자**가 있는 경우 login()함수가 필요하다.
- DB table에 session을 저장하고 유저에게 데이터를 넘겨준다. 
- HttpRequst 객체와 User 객체가 필요하다.
  - HttpRequst 객체: 쿠키에 대한 데이터를 받기 위함 
  - User 객체:  `get_user()`를 사용하여 유저에게 sessionid를 넘겨주기 위함. 

- **세션에 user의 ID를 저장한다 (로그인!)**

<br>

`get_user()`

https://docs.djangoproject.com/en/4.0/topics/auth/default/

- AuthenticationForm의 인스턴스 메서드이다.
- user_cache는 인스턴스 생성 시에 None으로 할당되며, 유효성 검사를 통과했을 경우 **유효성 검사를 통과한사용자 객체로 할당된다.**
- 인스턴스의 유효성을 먼저 확인하고, 인스턴스가 유효할 때만 user를 제공하려는 구조이다.

```python
class AuthenticationForm(forms.Form):
    
    def get_user(self):
        return self.user_cache
```

<br>

#### (3) is_authenticated (로그인 사용자에 대한 접근 제한)

> 로그인 사용자에 대한 엑세스를 제한한다.

https://docs.djangoproject.com/en/4.0/ref/contrib/auth/#attributes

- User model의 속성 중 하나이다.
- 모든 User 인스턴스에 대해 항상 True인 읽기 전용 속성이다.
  - AnonymousUser에 대해서는 항상 False이다.
- **사용자가 인증되었는지 여부를 알 수 있는 방법으로 일반적으로 request.user에서 이 속성을 사용한다.**
  - **미들웨어의 `django.contrib.auth.middleware.AuthenticationMiddleware`를 통과했는지 확인한다.**
- 단, 권한과는 관련이 없으며 사용자가 활성화 상태거나 유효한 세션을 가지고 있는지도 확인하지 않는다.

<br>

`request.user`

- 현재 로그인한 사용자를 나타내는 AUTH_USER_MODEL 인스턴스이다.
- 사용자가 현재 로그인하지 않은 경우 사용자는 AnonymousUser 인스턴스로 설정된다. 
  - 이는 is_authenticated로 구분할 수 있다.

<br>

---

**🙋‍♀️ 헷갈리는 개념**

1. **`Authentication Form`의 `get_user()` 메서드**
   - [공식문서](https://docs.djangoproject.com/en/4.0/topics/auth/default/)
   - 인증된 사용자 개체를 반환 
   - 단, 이 메서드는 폼 유효성 검사가 성공한 후에만 호출된다 (즉, Authentication Form에서만 사용이 가능하다. )
2. **`AUTH_USER_MODEL`의 `request.user` 인스턴스**
   - [공식문서](https://docs.djangoproject.com/en/4.0/ref/request-response/#attributes-set-by-middleware)
   - 현재 로그인한 사용자를 나타내는 AUTH_USER_MODEL 인스턴스
3. **`django.contrib.auth`의 `get_user(request)` 메서드**
   - [공식문서](https://docs.djangoproject.com/en/4.0/ref/contrib/auth/#django-contrib-auth)
   - 지정된 요청의 세션과 연결된 사용자 모델 인스턴스를 반환
   - 사용자 모델 인스턴스를 검색한 다음 사용자 모델의 `get_session_auth_hash()` 메서드를 호출하여 세션을 확인
   - get_user() 메서드에서 사용자가 반환되지 않는 경우 또는 세션 인증 해시가 유효성을 검사하지 않는 경우 AnonymousUser 인스턴스를 반환.
   - `from django.contrib.auth import get_user` 를 해야 사용 가능하다. 

<br>

**즉, 모두 인증된(로그인한) 유저 객체와 관련은 있지만 사용 방법은 다르다.**

---

<br>

#### (4) login_required decorator (로그인 사용자에 대한 접근 제한)

> 사용자가 로그인 되어 있지 않으면, settings.LOGIN_URL에 설정된 문자열 기반 절대 경로로 redirect한다.

- LOGIN_URL의 기본 값은 `/accounts/login/`이다. 
- 사용자가 로그인되어 있으면 정상적으로 view 함수를 실행한다.
- 인증 성공 시 사용자가 redirect 되어야 하는 경로는 "next"라는 쿼리 문자열 매개변수에 저장이 된다.
  - `/accounts/login/?next=/articles/create/`

<br>

`"next" query string parameter`

-  로그인이 정상적으로 진행되면 기존에 요청했던 주소로 redirect 하기 위해 주소를 keep한다.
  - 단, 별도로 처리 해주지 않으면 우리가 view에 설정한 redirect 경로로 이동하게 된다. 

<br>

#### (4) 구현

``` python
# accounts/views.py

from django.views.decorators.http import require_http_methods
from django.contrib.auth.forms import AuthenticationForm
from django.contrib.auth import login as auth_login

@require_http_methods(["GET", "POST"])
def login(request):
    # 이미 로그인되어 있으면, redirect!
    if request.user.is_authenticated:
        return redirect('articles:index')
    
    if request.method == "POST":
        # 단순 인증과정을 거치고 세션을 생성하는 과정이다. 
        form = AuthenticationForm(request, request.POST)
       
        #로그인 진행을 위한 사전 인증 절차
        if form.is_valid():
            auth_login(request, form.get_user()) 
            # 넥스트 파라미터가 있을 경우 로그인 후 해당 경로로 이동! 
            return redirect(request.GET.get('next') or 'articles:index')
    else:
        form = AuthenticationForm()
    context = {
        'form':form
    }
    return render(request, 'accounts/login.html', context)

```

<br>

```html
<!-- accouts/login.html --> 
{% extends 'base.html' %}

{% block content %}
  <h1>Login</h1>
  <hr>
  <!-- 현재 url로(next parameter가 있는) 요청을 보내기 위해 action 값을 비운다. --> 
  <form action="" method="POST">
    {% csrf_token %}
    {{ form.as_p }}
    <input type="submit">
  </form>
  <a href="{% url 'articles:index' %}">back</a>
{% endblock content %}


```

<br>

```python
# articles/views.py

from django.views.decorators.http import require_http_methods, require_POST, require_safe
from django.contrib.auth.decorators import login_required

@require_safe
def index(request):
    ...
    
@login_required  
@require_http_methods(['GET', 'POST'])
def create(request):
    ...

@require_safe
def detail(request, pk):
    ...
    
# login_required를 사용하면 405에러가 발생한다.
# 조건문을 사용하여 로그인 사용자만 접근가능하도록 한다. 
@require_POST
def delete(request, pk):
    if request.user.is_authenticated:
            article = get_object_or_404(Article, pk=pk)
            article.delete()
            return redirect('articles:index')
        
@login_required
@require_http_methods(['GET', 'POST'])
def update(request, pk):
    ...
    
```

- 405에러가 발생하는 이유
  - 로그인 이후 "next" 매개변수를 따라 해당 함수로 다시 redirect되는데, 이 때 @require_POST 때문에 405에러가 발생한다.
  - redirect과정에서 POST데이터를 손실하기 때문에 GET 방식으로 요청된다. ㅠ 
  - 비 로그인 상태로 글 삭제 요청(POST) => 로그인 검증 후  로그인 페이지로 이동 => 로그인 요청(POST) => 로그인 성공 및 next 경로로 리다이렉트(GET) 

<br>

### 2) 로그아웃

>  세션 Delete!

#### (1) logout(request)

- HttpRequest 객체를 인자로 받고 반환 값이 없다.
- 사용자가 로그인하지 않은 경우 **오류를 발생시키지 않는다.**
- 현재 요청에 대한 session data를 DB에서 완전히 삭제하고, 클라이언트 쿠키에서도 sessionid가 삭제된다.
- 다른 사람이 동일한 웹브라우저를 사용하여 로그인하고, 이전 사용자의 세션 데이터에 액세스 하는 것을 방지하기 위함이다. 

<br>

#### (2) 구현

```python
from django.views.decorators.http import require_POST
from django.contrib.auth import logout as auth_logout

@require_POST
def logout(request):
    if request.user.is_authenticated:
        auth_logout(request)
    return redirect('articles:index')

```

<br>

### 3) templates

```html
<!-- base.html --> 

    <div class="container">
      {% if request.user.is_authenticated %} 
        <h3>
          Hello,{{ user }}
        </h3>
        <form action="{% url 'accounts:logout' %}" method="POST">
          {% csrf_token %}
          <input type="submit" value="Logout">
      {% else %}
        <a href="{% url 'accounts:login' %}">Login</a>
      {% endif %}
      {% block content %}{% endblock content %}
    </div>
```

- `{{ user }}`로 현재 로그인 되어있는 유저 정보를 출력한다.
- 데이터를 전달하지 않았는데 어떻게 사용하는 걸까..?  

<br>

#### (1) context processors

- 템플릿이 렌더링 될 때 자동으로 호출 가능한 컨텍스트 데이터 목록
- 작성된 프로세서는 RequestContext에서 사용 가능한 변수로 포함된다. 

```python
# settings.py

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
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

<br>

`Users`

- 템플릿 RequestContext를 렌더링할 때, 현재 로그인한 사용자를 나타내는 auth.User 인스턴스이다.
  - (클라이언트가 로그인하지 않은 경우는 AnonymousUser 인스턴스)
- 템플릿 변수 `{{ user }}`에 저장된다.

<br>

## 3. 회원 가입, 탈퇴, 수정

**User를 CRUD 하는 것이다!**

<br>

### 1) 회원 가입

#### (1) UserCreationForm

[공식문서](https://github.com/django/django/blob/main/django/contrib/auth/forms.py#L104)

> 주어진 username과 password로 권한이 없는 새 user를 생성하는 ModelForm

3개의 필드를 가진다.

- username (from the user model)
- password1
- password2

비밀번호 field가 2개인 이유는 2개가 같은지 검증하기 위해서 독립적으로 2개 필드를 만들었다고 보면 된다.

<br>

#### (2) 구현

```python
#accouts/views.py

from django.views.decorators.http import require_http_methods, require_POST
from django.contrib.auth.forms import UserCreationForm

@require_http_methods(["GET", "POST"])
def signup(request):
    if request.user.is_authenticated:
        return redirect('articles:index')
        
    if request.method == "POST":
        # ModelForm
        form = UserCreationForm(data=request.POST)
        if form.is_valid():
            #  save된 객체로 ueser 매개변수를 전달한다.  
            user = form.save()
            auth_login(request,user)  # 회원가입 후 자동으로 로그인을 진행한다. 
            return redirect('articles:index')
    else:
        form = UserCreationForm()
    context = {
        'form':form
    }
    return render(request, 'accounts/signup.html', context)
```



`UserCreationForm의 save()`

```python
def save(self, commit=True):
     user = super().save(commit=False)
     user.set_password(self.cleaned_data["password1"])
     if commit:
            user.save()
     return user
```

=> user 객체를 반환하는 것을 확인할 수 있다. 

<br>

### 2) 회원 탈퇴

> DB에서 사용자를 삭제하는 것과 같다

```python
#accouts/views.py

from django.views.decorators.http import require_POST

@require_POST
def delete(request):
    if request.user.is_authenticated:
        # 반드시 회원 탈퇴후, 로그아웃을 해야한다.
        # 회원정보가 없어서 회원 탈퇴가 되지 않는다. 
        request.user.delete()
        auth_logout(request)
    return redirect('articles:index')
    
```

<br>

### 3) 회원 정보 수정

#### (1) UserChangeForm

> 사용자의 정보 및 권한을 변경하기 위해 admin 인터페이스에서 사용되는 ModelForm이다.

- 일반 사용자가 접근해서는 안될 field까지 노출하기 때문에 직접 사용하는 것은 위험하다.
- 그 결과 UserChangeForm을 상속받아 새로운 Form을 만든다.

<br>

```python
# accounts/forms.py

from django.contrib.auth.forms import UserChangeForm
from django.contrib.auth.models import User
from django.contrib.auth import get_user_model 

class CustonUserChangeForm(UserChangeForm):
    class Meta:
        # model = User
        model = get_user_model()
        fields = ('username', 'last_name', 'first_name',)
```

- model 속성 :  **User 모델을 직접 사용하지않고 `get_user_model()`을 사용한다.** 
  - get_user_model은 현재 사용 중인 User 클래스를 반환한다. (활성화된 사용자 모델)
  - 일반적으로 User는 재 정의(아래에서 설명) 하는 경우가 많은데,  `get_user_model()`은
    - 재 정의 하지 않았을 때는 User클래스
    - 재 정의했을 때는 새롭게 만든 User를 잡는다. 
    - 즉, 유연한 대처를 위해 `get_user_model()`을 사용한다. 

- fields 속성: User 클래스 < abstractUser 클래스 <  AbstractBaseUser 클래스

  - 아래에서 부가 설명하겠지만, User클래스는 기능이 없는 깡통 클래스이다.

  - 상속을 받는 AbstractUser 클래스 , AbstractBaseUser 클래스에서 사용할 수 있는 field를 확인할 수 있다. 

  - | **AbstractUser**     | **username, first_name, last_name, email, is_staff, is_active, date_joined** |
    | -------------------- | ------------------------------------------------------------ |
    | **AbstractBaseUser** | **password, last_login**                                     |
    | **PermissionsMixin** | **is_superuser**                                             |

<br>

---

`User 재 정의`

> [공식문서](https://docs.djangoproject.com/en/4.0/topics/auth/default/#user-objects)
>
> [링크](https://han-py.tistory.com/353)

우리는 웹을 만들 때 회원관리를 위해 user를 DB에 넣어야 한다. 이 때 기본적으로 User를 장고가 제공을 한다. 

```python
class User(AbsractUser):
    
    class Meta(AbstractUser.Meta):
        swappable = 'AUTH_USER_MODEL' # AUTH_USER_MODEL이라는 속성에 의해서 변할 수 있다. 
```

- User의 속성은 User가 상속받는 `AbsractUser`클래스에 있다.

- 즉, User자체는 기능이 없고 Django 내부에 세팅된 값이라 변경이 불가능하다.

- **그 결과, User 재정의가 필요하다.** 

  - ```python
    # accouts/models.py
    
    from django.db import models
    from django.contrib.auth.models import AbstractUser
    # Create your models here.
    
    class User(AbstractUser):
        pass
    ```

  - ```python
    # settings.py
    
    # accounts의 app의 models.py에서 내가 설정한 User를 사용하겠다고 설정한다. 
    AUTH_USER_MODEL = 'accounts.User'
    ```

<br>

**즉, 장고 내부의 User로 할 수 있는 것은 한계가 있기 때문에 내가 User를 models.py에 만든다.**

**그리고 settings.py에서 AUTH_USER_MODEL 설정을 하여 장고 내부의 User대신 내가 만든 User를 사용한다.** 

**(`get_user_model()`을 이용해서 클래스로 호출하여 사용한다. )**

---

<br>

#### (2) 구현

```python
# accounts/views.py

from .forms import CustonUserChangeForm
from django.contrib.auth.decorators import login_required
from django.views.decorators.http import require_http_methods

@login_required
@require_http_methods(['GET', 'POST'])
def update(request):
    if request.method == "POST":
        form = CustonUserChangeForm(data=request.POST, instance=request.user)
        if form.is_valid:
            # 상속받는 User는 모델폼이기 때문에 save()가 가능하다. 
            form.save()
            return redirect('articles:index')
    else:
        form = CustonUserChangeForm(instance=request.user)
    context = {
        'form': form 
    }
    return render(request, 'accounts/update.html', context)
```

<br>

### 4) 비밀 번호 수정

[django의 비밀번호 수정 로직 ](https://docs.djangoproject.com/en/4.0/topics/auth/default/#changing-passwords)

 #### (1)PasswordChangeForm

> 사용자가 비밀번호를 변경할 수 있도록 하는 Form이다.
>
> [공식문서](https://docs.djangoproject.com/en/4.0/topics/auth/default/#django.contrib.auth.forms.PasswordChangeForm)

- 이전 비밀번호를 입력하여 비밀번호를 변경할 수 있도록 한다.

 ```python
 class PasswordChangeForm(SetPasswordForm):
     """
     A form that lets a user change their password by entering their old
     password.
     """
     error_messages = {
         **SetPasswordForm.error_messages,
         'password_incorrect': _("Your old password was entered incorrectly. Please enter it again."),
     }
     old_password = forms.CharField(
         label=_("Old password"),
         strip=False,
         widget=forms.PasswordInput(attrs={'autocomplete': 'current-password', 'autofocus': True}),
     )
 
     field_order = ['old_password', 'new_password1', 'new_password2']
 
 ```

- 3개의 필드가 존재한다.
  - old_password
  -  new_password1
  - new_password2

- SetPasswordForm의 상속을 받는 서브 클래스이다.
  - SetPasswordForm은 forms.Form에게 상속을 받는다. 즉,  **모델폼은 아닌 일반 폼이다.**

<br>

#### (2) update_session_auth_hash(request, user)

- 현재 요청과 새 session hash가 파생 될 업데이트 된 사용자 객체를 가져오고, session hasg를 적절하게 업데이트한다.
- **비밀번호가 변경되면 기존 세션과의 회원 인증 정보가 일치하지 않게 되어 로그인 상태를 유지할 수 없기 때문이다.**
- 암호가 변경되어도 로그아웃되지 않도록 새로운 password hash로 session을 업데이트한다. 

<br>

#### (3) 구현

```python
# accounts/views.py
from django.contrib.auth import update_session_auth_hash
from django.contrib.auth.forms import PasswordChangeForm
from django.views.decorators.http import require_http_methods
from django.contrib.auth.decorators import login_required

@login_required
@require_http_methods(['GET', 'POST'])
def change_password(request):
    if request.method == 'POST':
        # 모델 폼이 아니기 때문에 user관련 정보가 필요하다.  
        form = PasswordChangeForm(user=request.user, data=request.POST)
        if form.is_valid():
            member = form.save()
            # 세션을 유지 시킨다. 
            # update_session_auth_hash(request, form.user)
            update_session_auth_hash(request, member)
            return redirect('articles:index')
    else:
        form = PasswordChangeForm(request.user)
    context = {
        'form': form,
    }
    return render(request, 'accounts/change_password.html', context)
```



<br>

## 4. 요약 

`로그인`

| 키워드                                 | 설명                                                         |
| -------------------------------------- | ------------------------------------------------------------ |
| `AuthenticationForm(request)`          | forms.Form의 서브 클래스<br /> modelform이 아니다. 그 결과, 모델에게 받는 정보가 없다.<br />새로운 정보를 더 넘겨줘야 하기 때문에 request를 반드시 포함 시켜 줘야한다 |
| `auth_login(request, form.get_user())` | 모델과 관련이 없고 로그인을 위한 함수가 존재한다.<br />`request`: 쿠키와 세션 중에 쿠키는 요청 객체에 담겨있기 때문에 쿠키를 받기위해서는 request는 필수!<br />`form.get_user()`: form에 있는 user정보이다. <br />(DB table에 세션을 저장하고 쿠키에 담아서 사용자에게 넘겨준다.) |



`회원 가입`

| 키워드                     | 설명                                                         |
| -------------------------- | ------------------------------------------------------------ |
| ` UserCreationForm`        | forms.ModelForm의 서브 클래스<br />username (from the user model), password1, password2 총 3개의 필드를 가지고 있다. |
| `user = form.save()`       | form.save()는 유저를 반환한다.                               |
| `auth_login(request,user)` | 회원 가입 후 자동으로 로그인이 되는게 자연스러운 로직이다.   |

<br>

`회원 정보 수정`

| 키워드                                        | 설명                                                         |
| --------------------------------------------- | ------------------------------------------------------------ |
| `CustonUserChangeForm(instance=request.user)` | UserChangeForm, AbstractUser, AbstractBaseUser 클래스의 서브 클래스로 모델폼이다.<br />form을 정의할 때, <br />models은 유연한 대처를 위해 get_user_model()를 사용한다. <br />fields는 위의 클래스들의 속성에서 가져올 수 있다.<br /><br />update이기 때문에 instance 매개변수가 필요하다. |
| `form.save()`                                 | 모델폼!                                                      |

<br>

`비밀번호 수정`

| 키워드                                         | 설명                                                         |
| ---------------------------------------------- | ------------------------------------------------------------ |
| `PasswordChangeForm(request.user)`             | SetPasswordForm의 서브 클래스로 일반 form이다.<br />매개변수user는 필수 인자이다. |
| `update_session_auth_hash(request, form.user)` | 세션을 유지시키기 위해 필요하다.                             |
| `form.save()`                                  | 모델 폼이 아님에도 save()메서드를 사용할 수 있는 이유는 SetPasswordForm에서 따로 정의를 했기 때문이다. |

<br>
