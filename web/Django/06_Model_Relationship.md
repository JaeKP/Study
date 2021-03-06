# Model Relationship (1:N)

[toc]

## 1. Comment CRD

### 1) Foregin Key

`Foregin Key (외래 키)`

- 관계형 데이터 베이스에서 한 테이블의 필드 중 다른 테이블의 행을 식별할 수 있는 키이다.
- 참조하는 테이블의 필드에 해당하고 참조되는 테이블의 PK를 가르킨다. 
  - 즉, 키를 사용하여 참조되는 테이블의 유일한 값을 참조한다.**(참조 무결성)**
  - 참조 무결성: 데이터베이스 관계 모델에서 관련된 2개의 테이블 간의 일관성을 말한다.
    - 외래 키가 선언된 테이블의 외래 키 속성(열)의 값은 그 테이블의 부모가 되는 테이블의 기본 키 값으로 존재해야 한다. 

- 외래키의 값이 반드시 PK일 필요는 없지만 유일한 값이여야 한다. 



예를 들어,  한 게시글(Article)에는 여러개의 댓글(Comment)가 달릴 수 있다. 과연 어떻게 DB테이블을 연동시킬까? 

- 참조되는 테이블 (Article 테이블)

| Article_ID(PK) | Article_title | Article_content |
| -------------- | ------------- | --------------- |
| 1              | 제목 1        | 내용 1          |
| 2              | 제목 2        | 내용 2          |
| 3              | 제목 3        | 내용 3          |

- 참조하는 테이블 (Comment 테이블)

| Comment_ID(PK) | Comment_content | Article_ID                                   |
| -------------- | --------------- | -------------------------------------------- |
| 1              | 내용1           | 2 (Article_ID가 2인 게시글에 달린 댓글이다.) |
| 2              | 내용2           | 2 (Article_ID가 2인 게시글에 달린 댓글이다.) |
| 3              | 내용3           | 1 (Article_ID가 1인 게시글에 달린 댓글이다.) |

<br>

#### (1) models.ForeginKey(modelclass, on_delete)

- `ForeginKey` < `ForeginObject`< `RelatedField`

- Provide a many-to-one relation **by adding a column** to the local model to hold the remote value.

  By default ForeignKey will target the pk of the remote model but this behavior can be changed by using the `to_field` argument.

- 필수 매개변수 
  1. 참조하는 model class
  2. on_delete 옵션 
- migrate 작업 시 필드 이름에 _id를 추가하여 데이터베이스 열 이름을 만든다.
- 재귀 관계가 가능하다. (자신과 1:N 가능)
  - `models.ForeginKey('self', on_delete=models.CASCADE)`

<br>

`on_delete`

> 외리 캐가 참조하는 객체가 사라졌을 때, 외래 키를 가진 객체를 어떻게 처리할 지를 정의한다.

- 데이터 무결성을 위해서 매우 중요한 설정이다.
- on_delete옵션에 사용가능한 값:
  - CASCADE: 부모 객체(참조된 객체)가 삭제 되었을 때, 이름 참조하는 객체도 삭제한다.
  - PROTECT, SET_NULL, SET_DEFAULT, SET(), DO_NOTHING, RESTRICT

<br>

`데이터 무결성`

> 데이터의 정확성과 일관성을 유지하고 보증하는 것을 가리키며, 데이터 베이스나 RDBMS 시스템의 중요한 기능이다.

- 개체 무결성 
  - PK의 개념과 관련이 있다.
  - 모든 테이블이 PK를 가져야 하며 PK로 선택된 열은 고유한 값이어야 하고 빈 값은 허용치 않음을 규정한다.
- 참조 무결성
  - FK(외래키) 개념과 관련이 있다. 
  - FK 값이 데이터 베이스의 특정 테이블의 PK 값을 참조하는 것이다. 
    (기본 키와 참조 키 간의 관계가 항상 유지됨을 보장)
- 범위(도메인) 무결성
  - 정의된 형식(범위)에서 관계형 데이터베이스의 모든 칼럼이 선언되도록 규정한다.

<br>

#### (2) 구현

```python
# articles/models.py

class Comment(models.Model):
    article = models.ForeginKey(Article, on_delete = models.CASCADE)
    content = models.CharField(max_length=200)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    
    def __str__(self):
        return self.content
```

- 참조하는 모델의 이름을 소문자로 변환하여 ForeginKey 컬럼의 이름으로 기입한다.  (article)
  - 명시적인 모델 관계 파악을 위해 참조하는 클래스 이름의 소문자(단수형)으로 작성하는 것이 바람직하다. 

- migrate하면 article_id라는 이름의 외래 키 컬럼이 생긴다. 

<br>

### 2) shell_plus로 연습

#### (1) create

```sqlite
comment = Comment() -- 인스턴스 생성 

comment.contetnt = 'first comment'

-- 오류 발생: articles_comment테이블의 ariticle_id가 누락되었기 때문이다.
comment.save()
IntegrityError: NOT NULL constraint failed: articles_comment.article_id

article = Article.objects.get(pk=1) -- 아티클 불러오기

article
<Article: title>

comment.article = article

comment.save() -- 저장!

```

<br>

#### (2) 참조 (read)

- comment 인스턴스를 통한 article 접근이 가능하다. 

```sql
comment.pk
1

comment.content
'first comment'

comment.article_id
1

comment.article.pk
1

comment.article.content
'content'
```

- **`참조` Comment(N) -> Article(1)** 

  - **댓글의 경우, 어떠한 댓글이든 반드시 자신이 참조하고 있는 게시글이 있으므로 게시글에 접근할 수 있다.**
  - **실제 ForeginKeyField 또한 Comment 클래스에서 작성된다.** 

- admin site에서 작성된 댓글을 확인할 수 있다.

  - 단, admin.py에 다음과 같이 기입

  - ```python
    from .models import Comment
    
    admin.site.register(Commnet)
    ```

<br>

**그렇다면, article에서 comment를 READ하는 방법은 없을까.**

<br>

#### (3) 역참조 (read) 

**`역참조` Article(1) -> Comment(N)**

- `article.comment`형태로는 사용할 수 없고, article.comment_set 매니저가 생성된다.
- 게시글에 몇 개의 댓글이 작성 되었는지 Django ORM이 보장할 수 없기 때문이다.
  - article은 comment가 있을 수도 있고, 없을 수도 있다.
  - 실제로 Article 클래스에는 Comment와의 어떠한 관계도 작성되어 있지 않다.

```sqlite
article = Article.objects.get(pk=1)

article
<Article: title>

In [3]: dir(article) 
[
 ...
 'comment_set',   -- 역 참조 모델 매니저
 ...
]
 
 
article.comment_set.all() -- 역참조!(쿼리셋을 반환한다.)
<QuerySet [<Comment: first commtent>, <Comment: second commemt>]>

comments = article.comment_set.all()

In [6]: for comment in comments:
   ...:     print(comment.content)
   ...: 
first comment
second comment
```

- `model_set`: 역참조시 사용하는 키워드
- 모델에서 컬럼을 정의할 때 `realated_name`을 사용하여 사용할 키워드를 수정할 수 있다.
  - `article = models.ForeignKey(Article,on_delete=models.CASCADE, related_name='comments')`
  - 수정을 하면 `article.comment_set.all()`  대신 다음과 같은 키워드를 사용한다.  `article.comments.all()`
  -  하지만 1:N에서는 권장하지 않는다. M:N에서는 불가피하게 변경을 해야하기때문에 `comment_set`키워드를 통해  1:N에서의 관계 조회임을 알 수 있다
  - 즉, 1:N, M:N 관계에서 조회인지 구분을 하기 위함이다.  

<br>

### 3) Comment Create, READ

```python
# articles/urls.py

from django.urls import path
from . import views

app_name = 'articles'

urlpatterns = [
    ...
path('<int:pk>/comments/', views.comments_create, name="comments_create" ),
]
```

<br>

```python
# articles/forms.py

from django import forms
from .models import Comment

class CommentForm(forms.ModelForm):
    class Meta:
        model = Comment
        fields = ('content',)
```

- 만약 `fields = '__all__'`를 했다면, Article_id까지 작성자가 직접 입력하는 상황이 발생한다!
- Article_id는 view함수에서 저절로 생성이 되도록 만들어야 한다. 

<br>

```python
# articles/views.py

from django.views.decorators.http import require_POST, require_safe
from .models import Article, Comment
from .forms import ArticleForm, CommentForm

@require_safe
def detail(request, pk):
    article = get_object_or_404(Article, pk=pk)
    comment_form = CommentForm()          # CREATE
    comments = article.comment_set.all()  # READ
    context = {
        'article': article,
        'comment_form': comment_form,
        'comments': comments,
    }
    return render(request, 'articles/detail.html', context)

@require_POST
def comments_create(request, pk):
    if request.user.is_authenticated:
        article = get_object_or_404(Article, pk=pk)
        comment_form = CommentForm(request.POST)
        if comment_form.is_valid():
            # 세이브를 안하고 객체만 반환하기 위함
            comment = comment_form.save(commit=False) 
            # article_id 컬럼의 값을 따로 추가한다. 
            comment.article = article
            # 저장!
            comment.save()
        return redirect('articles:detail', article.pk)
    return redirect('acoounts:login')
```

- `.save(commit=False)`
  - Create, but don't save the new instance
  - 아직 데이터베이스에 저장되지 않은 인스턴스를 반환한다.
  - 저장하기 전에 객체에 대한 사용자 지정 처리를 수행할 때 유용하게 사용한다. 

<br>

### 4) Comment Delete

```python
# articles/urls.py

from django.urls import path
from . import views

app_name = 'articles'

urlpatterns = [
    path('', views.index, name='index'),
    path('create/', views.create, name='create'),
    path('<int:pk>/', views.detail, name='detail'),
    path('<int:pk>/delete/', views.delete, name='delete'),
    path('<int:pk>/update/', views.update, name='update'),
    path('<int:pk>/comments/', views.comments_create, name="comments_create" ),
    
    # url의 통일성을 위해서 다음과 같이 생성한다. 
    path('<int:article_pk>/comments/<int:comment_pk>/delete/', views.comment_delete, name="comment_delete" ),
]
```

- 삭제해야하는 댓글의 pk를 알아야 하기 때문에 `<int:comment_id>`는 꼭 필요하다.

<br>

```python
# articles/views.py

@require_POST
def comment_delete(request, article_pk, comment_pk):
    if request.user.is_authenticated:
        comment =  get_object_or_404(Comment, pk=comment_pk)
        comment.delete()        
    return redirect('articles:detail', article_pk)
```

<br>

## 2. User Model

> Django에서는 기본 사용자 모델이 충분하더라도 커스텀 유저 모델을 설정하는 것을 강력하게 권장한다.

<br>

### 1) AUTH_USER_MODEL

```python
class User(AbstractUser):
    """
    Users within the Django authentication system are represented by this
    model.

    Username and password are required. Other fields are optional.
    """
    class Meta(AbstractUser.Meta):
        swappable = 'AUTH_USER_MODEL'  # AUTH_USER_MODEL이라는 속성에 의해서 변할 수 있다.
```

- User를 나타내는 데 사용하는 모델이다.
  - User를 참조하는 데 사용하여 default User Model을 재정의(오버라이드)할 수 있도록 한다.
  - default: `'auth.User'`
- 프로젝트가 진행되는 동안 변경할 수 없다.
  - 프로젝트 중간에 변경할 수는 있지만, 추가 작업이 필요하다.(이는 장고에서 권하지 않는다.)
- 프로젝트 시작 시 설정하기 위한 것이며, 참조하는 모델은 첫번째 마이그레이션에서 사용할 수 있어야 한다.

<br>

#### (1) Custom User Model

[공식문서](https://docs.djangoproject.com/en/4.0/topics/auth/customizing/#substituting-a-custom-user-model)

`모델 생성`

```python
# accounts/models.py

from django.db import models
from django.contrib.auth.models import AbstractUser

class User(AbstractUser):
    pass
```

- 위의 Django Default User 모델을 보면 알 수 있듯이, User자체는 기능이 없고 상속을 하는 `AbsractUser`클래스에 많은 속성이 존재한다.
  그래서 `AbstracUser(AbstractBaseUser, PermissionsMixin)`에게 상속을 받는다.

- 만약 비밀번호만 상속받고 싶을 경우에는 AbstractBaseUser에게 상속 받는다.  

  - | **AbstractUser**     | **username, first_name, last_name, email, is_staff, is_active, date_joined** |
    | -------------------- | ------------------------------------------------------------ |
    | **AbstractBaseUser** | **password, last_login**                                     |
    | **PermissionsMixin** | **is_superuser**                                             |

<br>

`default로 설정`

```python
# settings.py

AUTH_USER_MODEL = 'accounts.User'
```

- 기존에 Django가 사용하는 User모델이었던 auth 앱의 User 모델을 acoounts앱의 User 모델을 사용하도록 변경한다.

<br>

`admin site에 등록`

```python
# accounts/adim.py

from django.contrib import admin
from django.contrib.auth.admin import UserAdmin
from .models import User


admin.site.register(User, UserAdmin)
```

<br>

#### (2) 만약, 프로젝트 중간에 변경했다면?

> 데이터베이스를 초기화 한 후 마이그레이션을 진행해야 한다.

1. db.sqlite3 파일 삭제

2. migrations 파일 모두 삭제 (파일명에 숫자가 붙은 파일만 삭제한다.)

3. 마이그레이션!

   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```

<br>

### 2) 변경된 유저 모델을 반영

#### (1) accounts

`UserCreationForm`과 `UserChangeForm`은 기존 내장 User 모델을 사용한 ModelForm이기 때문에 Custom User Model로 대체해야 한다.

```python
# accounts/forms. py

from django.contrib.auth.forms import UserChangeForm, UserCreationForm
from django.contrib.auth import get_user_model


class CustomUserChangeForm(UserChangeForm):

    class Meta:
        model = get_user_model() # 현재 장고프로젝트에 활성화된 유저모델을 리턴한다. 
        fields = ('email', 'first_name', 'last_name',)


class CustomUserCreationForm(UserCreationForm):
    class Meta(UserCreationForm.Meta):
        model = get_user_model()  
        fields = UserCreationForm.Meta.fields + ('email',)
```

- get_user_model()은 현재 활성화된 UserModel을 반환한다.

  - 커스텀을 하지 않았다면 defalut인 User Model을 반환
  - 커스텀을 했다면, 커스텀한 User Model을 반환

- ` UserCreationForm.Meta.fields`는 UserCreationForm에서 정의된 필드를 의미한다. 

  - ```python
    class UserCreationForm(forms.ModelForm):
  	...
      
      password1 = forms.CharField(
          label=_("Password"),
          strip=False,
          widget=forms.PasswordInput(attrs={'autocomplete': 'new-password'}),
          help_text=password_validation.password_validators_help_text_html(),
      )
      password2 = forms.CharField(
          label=_("Password confirmation"),
          widget=forms.PasswordInput(attrs={'autocomplete': 'new-password'}),
          strip=False,
          help_text=_("Enter the same password as before, for verification."),
      )
    
      class Meta:
          model = User
          fields = ("username",)
          field_classes = {'username': UsernameField}
    
    ...
    ```
    
    **현재 UserCreationForm에 있는 fields는  `username`필드를 의미한다.**
    
    **뒤에 추가하길 원하는 필드를 작성할 수도 있다.** 


- view함수도 form을 수정한다. 

<br>

### 3) 1: N 관계

|         | 관계 | table            |
| ------- | ---- | ---------------- |
| User    | 1    | auth_user        |
| Article | N    | articles_article |
| Comment | N    | articles_comment |

앞에서 보았듯이, 게시글 하나당 여러개의 댓글이 작성될 수있었다. 마찬가지로, 유저 하나당 여러개의 게시글과 댓글을 작성할 수 있다. 

<br>

#### (1) models.py 

```python
# models.py

from django.db import models
from django.conf import settings

class Article(models.Model):
    # 유저 모델을 참조하는 방법은 크게 두가지이다.
    # 문자열: settings.AUTH_USER_MODEL => models.py에서만 사용
    # 객체: get_user_model()
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    title = models.CharField(max_length=10)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    def __str__(self):
        return self.title


class Comment(models.Model):
    article = models.ForeignKey(Article,on_delete=models.CASCADE)
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    content = models.CharField(max_length=200)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    def __str__(self):
        return self.content

```

- **ForeginKey(modelclass, on_delete)**
- 만약, 프로젝트 중간에 마이그레이션을 한다면 user_id 필드는 null값을 허용하지 않기 때문에 기존 테이블 필드에 부여할 값을 설정해야 한다. 

<br>

`django.contrib.auth.get_user_model()`과 `settings.AUTH_USER_MODEL`의 차이점

둘 다 현재 활성화된 유저 모델을 의미한다. 

- `get_user_model`: 클래스 (객체)이다.
- `AUTH_USER_MODEL`: 문자열이다.

일반적으로 `AUTH_USER_MODEL`은 models.py에서만 사용된다. 그외의 모든 곳에서는  `get_user_model`이 사용된다. 

models.py에서만 AUTH_USER_MODEL을 사용하는 것은 앱 등록 순서와 관련이 있다. 
앱을 등록한 순서대로 models.py에 접근하는데 account앱의 Custom User Model을 모르는 상황에서 앞에 있는 앱에서 User Model을 참조하면 제대로 작동하지 않을 수 있다. 

<br>

#### (2) forms.py

```python
from django import forms
from .models import Article, Comment

class ArticleForm(forms.ModelForm):

    class Meta:
        model = Article
        exclude = ('user',)

class CommentForm(forms.ModelForm):
    class Meta:
        model = Comment
        fields = ('content',)
```

- user_id 필드가 노출되기 때문에 수정

<br>

#### (3) views.py

- 글이나 코멘트 작성시 use_id를 자동으로 부여해주어야 한다. 

```python
@login_required
@require_http_methods(['GET', 'POST'])
def create(request):
    if request.method == 'POST':
        form = ArticleForm(request.POST)
        if form.is_valid():
            # 유저 정보 추가
            article = form.save(commit=False)
            article.user = request.user
            article.save()
            return redirect('articles:detail', article.pk)
    else:
        form = ArticleForm()
    context = {
        'form': form,
    }
    return render(request, 'articles/create.html', context)

@require_POST
def comments_create(request, pk):
    if request.user.is_authenticated:
        article = get_object_or_404(Article, pk=pk)
        comment_form = CommentForm(request.POST)
        if comment_form.is_valid():
            comment = comment_form.save(commit=False) 
            comment.article = article
             # 유저 정보 추가
            comment.user = request.user
            comment.save()
        return redirect('articles:detail', article.pk)
    return redirect('acoounts:login')
```



- 글이나 코멘트 삭제시, 작성한 유저만 삭제할 수 있어야 한다.

```python
@require_POST
def delete(request, pk):
    article = get_object_or_404(Article, pk=pk)
    if request.user.is_authenticated:
        # 글 작성자만 삭제할 수 있도록 기능을 추가한다.
        if request.user == article.user:
            article.delete()
    return redirect('articles:index')


@require_POST
def comment_delete(request, article_pk, comment_pk):
    if request.user.is_authenticated:
        comment =  get_object_or_404(Comment, pk=comment_pk)
        # 댓글 작성자만 삭제할 수 있다. 
        if request.user == comment.user:
            comment.delete()        
    return redirect('articles:detail', article_pk)
```

<br>

- 작성한 유저만 글을 수정할 수 있도록 설정한다. 

```python
@login_required
@require_http_methods(['GET', 'POST'])
def update(request, pk):
    article = get_object_or_404(Article, pk=pk)
    
    #작성자만 수정이 가능하다. 
    if request.user == article.user:
        if request.method == 'POST':
            form = ArticleForm(request.POST, instance=article)
            if form.is_valid():
                article = form.save()
                return redirect('articles:detail', article.pk)
        else:
            form = ArticleForm(instance=article)
    else:
        return redirect('articles:index')
    context = {
        'article': article,
        'form': form,
    }
    return render(request, 'articles/update.html', context)

```

<br>

- detail 페이지에서 코멘트가 볼 수있도록 관련 데이터를 전달한다.

```python
@require_safe
def detail(request, pk):
    article = get_object_or_404(Article, pk=pk)
    comment_form = CommentForm() # 인스턴스 생성
    comments = article.comment_set.all()
    context = {
        'article': article,
        'comment_form': comment_form,
        'comments': comments,
    }
    return render(request, 'articles/detail.html', context)

```

<br>

#### (4) templates

- 작성한 유저만 수정, 삭제 버튼이 보이도록 수정한다. 

```html
{% extends 'base.html' %}
{% block content %}
  <h1>DETAIL</h1>
   ...
<!-- 작성한 유저만 수정과 삭제가 보인다. --> 
  {% if request.user == article.user %}
    <a href="{% url 'articles:update' article.pk %}">수정</a>
    <form action="{% url 'articles:delete' article.pk %}" method="POST">
      {% csrf_token %}
      <input type="submit" value="삭제">
    </form>
  {% endif %}

  ...
  <hr>
  <h4>댓글 목록</h4>
  {% for comment in comments %}
    <li>
      {{ comment.user }}
      {{ comment.content }}
        
<!-- 작성한 유저만 삭제가 보인다. --> 
      {% if request.user == comment.user %}
        <form action="{% url 'articles:comment_delete' article.pk comment.pk %}" method="POST">
          {% csrf_token %}
          <input type="submit" value="삭제">
        </form>
      {% endif %}
    </li>
  {% endfor %}
  ...
{% endblock content %}
```

<br>
