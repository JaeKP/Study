# Models Relationship (M:N)

[toc]

## 1. M:N 관계형 DB
### 1) 1:N 관계형 DB의 한계

#### (1) models.py

```python
#models.py
from django.db import models

class Doctor(models.Model):
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 의사 {self.name}'


class Patient(models.Model):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 환자 {self.name}'

```
<br>

`데이터 테이블` 

| Doctor       | Patient            |
| ------------ | ------------------ |
| id `integer` | id `integer`       |
| name `text`  | doctor_id `bigint` |
|              | name `text`        |

<br>

#### (2) ORM

- 만약 한 환자에게 2명이상의 의사가 진료를 예약 한다면, 어떻게 해야 할까 


```python
doctor1 = Doctor.objects.create(name='justin')

doctor2 = Doctor.objects.create(name='eric')

patient1 = Patient.objects.create(name='tony', doctor=doctor1)

patient2 = Patient.objects.create(name='harry', doctor=doctor2)

# 1:N의 한계(다른 의사에게 진료받은 것을 기록하기 위해서는 새로운 인스턴스를 또 생성해야 한다.)
# tony라는 환자가 두개의 레코드에 기록되고 있다. 
patient3 = Patient.objects.create(name='tony', doctor=doctor2)

# 그렇다면, 한 번에 기록하는 것은 불가능한 걸까?
# 오류가 발생한다. 하나의 외래키에 여러개를 추가할 수 없다. 
patient4 = Patient.objects.create(name='harry', doctor=doctor1, doctor2)
SyntaxError: positional argument follows keyword argument
```

- 위의 상황의 경우 다음과 같은 데이터 테이블이 생성되게 되는 것이다.
  - `hospitals_patient`
    tony가 2개의 레코드에 기록되는 것은 비효율적이다. (여러 의사에게 진료받은 기록을 환자 한 명에게 저장할 수 없다.)
    
    | id   | name  | doctor_id |
    | ---- | ----- | --------- |
    | 1    | tony  | 1         |
    | 2    | harry | 2         |
    | 3    | tony  | 2         |

<br>

### 2) 중개 모델 활용

앞선 경우, 새로운 예약을 생성하는 것이 불가능해 새로운 객체를 생성했어야 했다. 

그렇다면, 예약을 기록하는 중개모델(Associative Table)을 만들어서 예약을 기록하는 방법이 있다. 

<br>

#### (1) models.py

```python
#models.py
from django.db import models

class Doctor(models.Model):
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 의사 {self.name}'


# 외래키 삭제
class Patient(models.Model):
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 환자 {self.name}'

# 중개모델 작성
class Reservation(models.Model):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    patient = models.ForeignKey(Patient, on_delete=models.CASCADE)

    def __str__(self):
        return f'{self.doctor_id}번 의사의 {self.patient_id}번 환자'
```

- 중개 모델은 다른 모델들과 1:N 관계를 가진다. 
  
  - 의사: 예약 (1 : N)  
  
  - 환자: 예약 (1 : N)
  
  - 예시
    `hospitals_reservation`
  
    | id   | doctor_id | patiend_id |
    | ---- | --------- | ---------- |
    | 1    | 1         | 1          |
    | 2    | 2         | 1          |
    | 3    | 1         | 2          |

<br>

#### (2) ORM

```python
# 의사 추가
doctor1 = Doctor.objects.create(name='justin')

# 환자 추가
patient1 = Patient.objects.create(name='tony')

# 예약 추가
Reservation.objects.create(doctor=doctor1, patient=patient1)
<Reservation: 2번 의사의 1번 환자>

# 예약 내역 조회(역참조)
doctor1.reservation_set.all()
<QuerySet [<Reservation: 2번 의사의 1번 환자>]>

patient1.reservation_set.all()
<QuerySet [<Reservation: 2번 의사의 1번 환자>]>

# 환자 추가
patient2 = Patient.objects.create(name='harry')

# 예약 추가
Reservation.objects.create(doctor=doctor1, patient=patient2)
<Reservation: 2번 의사의 2번 환자>

# 예약 내역 조회(역참조)
doctor1.reservation_set.all()
<QuerySet [<Reservation: 2번 의사의 1번 환자>, <Reservation: 2번 의사의 2번 환자>]>

patient2.reservation_set.all()
<QuerySet [<Reservation: 2번 의사의 2번 환자>]>
```

<br>

### 3) ManyToMany

> 그러나 DJango 에는 ManyToMany라는 필드가 있다. 
>
> [공식문서](https://docs.djangoproject.com/en/4.0/topics/db/examples/many_to_many/)

`M:N관계에서 ManyToManyField를 사용하는 이유`

- 중개모델 사용시, 관계가 얽히면 얽힐 수록 테이블이 점점 늘어난다.
- Django의 ManyToMany 필드를 활용하면 RelatedManager를 통해 ORM 문법을 더 간단하게 사용할 수 있다.
  - `RelatedManager`: 일대다 또는 다대다 관련 컨텍스트에서 사용되는 manager로 1:N에서는 tartget 모델 인스턴스만 사용이 가능하다.
    M:N 관계에서는 관련된 두 객체에서 모두 사용이 가능하다.
    예시) `add()`, `remove()`, `create()`, `clear()`, `set()` 등 


<br>

#### (1) models.py

```python
#models.py

from django.db import models

class Doctor(models.Model):
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 의사 {self.name}'


class Patient(models.Model):
    # ManyToManyField 작성
    # 다대다 관계 설정 시 사용하는 모델 필드
    doctors = models.ManyToManyField(Doctor)
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 환자 {self.name}'
```

- 위에서 생성한 중개테이블과 같은 column을 가진 테이블이 자동으로 생성된다. 
  - `hospital_patient_doctors`
  
  - 중개테이블에 작성한다는 것은 두 테이블의 관계를 생성하는 것이다.
    | hospital_doctor | hospital_patient | hospital_patient_doctors |
    | --------------- | ---------------- | ------------------------ |
    | id `integer`    | id `integer`     | id `integer`             |
    | name `text`     | name `text`      | patient_id `bigint`      |
    |                 |                  | doctor_id `bigint`       |

<br>


- `models.ManyToManyField`는 Doctor 클래스에서 정의해도 상관없다.
  - M:N은 1:N과 달리 종속된 관계가 아니다.
  - 대신 참조와 역참조의 관계는 달라진다. 

<br>

#### (2) ORM

```python
# 의사 추가
doctor1 = Doctor.objects.create(name='justin')

# 환자 추가
patient1 = Patient.objects.create(name='tony')

patient2 = Patient.objects.create(name='harry')

# 예약 추가
patient1.doctors.add(doctor1)

# 예약 내역 조회
patient1.doctors.all()
<QuerySet [<Doctor: 1번 의사 justin>]>

doctor1.patient_set.all()
<QuerySet [<Patient: 1번 환자 tony>]>

# 예약 추가
doctor1.patient_set.add(patient2)

# 예약 내역 조희
doctor1.patient_set.all()
<QuerySet [<Patient: 1번 환자 tony>, <Patient: 2번 환자 harry>]>

patient2.doctors.all()
<QuerySet [<Doctor: 1번 의사 justin>]>

patient1.doctors.all()
<QuerySet [<Doctor: 1번 의사 justin>]>


# 예약 취소
doctor1.patient_set.remove(patient1)

patient2.doctors.remove(doctor1)

```
`add()`

- `patient1.doctors.add(doctor1)`

- 지정된 객체(doctor1) 를 관련 객체 집합(patient.doctors)에 추가한다.
- 이미 존재하는 관계에 사용하면 관계가 복제되지 않는다.
- **모델 인스턴스, 필드 값(PK)을 인자로 허용한다.**  

<br>
`remove()`

- 관련 객체 집합에서 지정된 모델 객체를 제거한다.
- 내부적으로 Query.Set.delete()를 사용하여 관계가 삭제된다.
- **모델 인스턴스, 필드 값(pk)를 인자로 허용한다.** 

<br>

`hospital_patient_doctors` (생성된 중개 테이블)

| id   | patient_id | doctor_id |
| ---- | ---------- | --------- |
| 1    | 1          | 1         |
| 2    | 2          | 1         |

<br>

#### (3) related_name 속성

1:N과 구분되기 위해서 ManyToManyField는

- **복수형의 변수**를 사용한다. 
- **realated_name** 속성을 사용한다. 
  - target model(관계 필드를 가지지 않은 모델이) source model(관계 필드를 가진 모델)을 역참조 할 때, 사용할 manager의 이름을 설정한다.
  - 즉, 역참조 시에 사용하는 manager의 이름을 설정한다.
  - ForeginKey의 related_name과 동일하다.

```python
from django.db import models

class Doctor(models.Model):
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 의사 {self.name}'


class Patient(models.Model):    
    # ManyToManyField - related_name 작성
    doctors = models.ManyToManyField(Doctor, related_name='patients')
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 환자 {self.name}'
```

<br>

```python
# related_name설정 후 기존의 _set manager는 더이상 사용할 수 없다. 
doctor1.patient_set.all()
AttributeError: 'Doctor' object has no attribute 'patient_set'

doctor1.patients.all()
<QuerySet []>
```

- 명령어를 통일 시켰다.
  - 역참조: `doctor객체. patients.all()`
  - 참조: `patiens객체.dotors.all()`


<br>

### 4) through 옵션을 활용한 중개 테이블

현재 `ManyToManyField`는 사람에 대한 정보만 들어가 있다.
- 예약 정보가 더 필요할 수도 있다. (시간, 진료 목적 등)
- 만약 중개테이블에 추가 칼럼이 필요할 때는 직접 중개 테이블을 생성해야 한다.

즉, 중개 테이블에 추가 데이터를 사용해 다대다 관계로 연결하는 경우, 중개 테이블을 직접 작성한다.

이때,  `through`옵션을 사용하여, 중개 테이블을 나타내는 DJango 모델을 지정할 수 있다. 



#### (1) models.py

```python
# models.py

from django.db import models


class Doctor(models.Model):
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 의사 {self.name}'


class Patient(models.Model):
	# through 속성으로 모델 관계를 설정한다. 
    doctors = models.ManyToManyField(Doctor, through='Reservation')
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 환자 {self.name}'


class Reservation(models.Model):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    patient = models.ForeignKey(Patient, on_delete=models.CASCADE)
    symptom = models.TextField()
    reserved_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return f'{self.doctor.pk}번 의사의 {self.patient.pk}번 환자'
```

`through 속성`

중개 테이블을 직접 작성하는 경우, through 옵션을 사용하여 중개 테이블을 나타내는 DJango 모델을 지정할 수 있다.

일반적으로 중개 테이블에 추가 데이터를 사용하는 다대다 관계와 연결하려는 경우에 주로 사용된다. 

<br>

`hospitals_reservation` (생성된 중개 테이블)

| id        | symptom | reserved_at | doctor_id | patient_id |
| --------- | ------- | ----------- | --------- | ---------- |
| `integer` | `text`  | `datetime`  | `bigint`  | `bigint`   |

<br>

#### (2)ORM 

```python
# 의사 추가
doctor1 = Doctor.objects.create(name='justin')

# 환자 추가
patient1 = Patient.objects.create(name='tony')

patient2 = Patient.objects.create(name='harry')

# 예약 추가 (예약 저장을 해야 한다.)
reservation1 = Reservation(doctor=doctor1, patient=patient1, symptom='headache')

reservation1.save()

# 아까처럼 환자나 의사 객체로 예약을 추가할 수 있을까...?
# 가능하다 대신 through_defaults를 딕셔너리 형태로 인자에 넣어서 전달해야 한다. 
patient2.doctors.add(doctor1, through_defaults={'symptom': 'flu'})


#예약 내역 조회 
doctor1.patient_set.all()
<QuerySet [<Patient: 1번 환자 tony>, <Patient: 2번 환자 harry>]>

patient1.doctors.all()
<QuerySet [<Doctor: 1번 의사 justin>]>

patient2.doctors.all()
<QuerySet [<Doctor: 1번 의사 justin>]>


# 예약 취소
doctor1.patient_set.remove(patient1)
patient2.doctors.remove(doctor1)
```

<br>

## 2. 좋아요

> 게시글에는 여러 유저가 좋아요를 누를 수 있고,  유저는 여러 게시글에 좋아요를 누를 수 있다. 

#### (1) models.py

```python
# articles/models.py
from django.db import models
from django.conf import settings

class Article(models.Model):
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)

    # 이글에 좋아요를 누른 유저
    like_user = models.ManyToManyField(settings.AUTH_USER_MODEL, related_name = 'like_articles')

    title = models.CharField(max_length=10)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    def __str__(self):
        return self.title


class Comment(models.Model):
    article = models.ForeignKey(Article, on_delete=models.CASCADE)
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    content = models.CharField(max_length=200)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    def __str__(self):
        return self.content

```

`현재 User-Article간 사용가능한 DB API`

1. article.user: 게시글을 작성한 유저 (1:N)
2. article.like_users: 게시글을 좋아요한 유저 (M:N)
3. user.article_set: 유저가 작성한 게시글 (1:N) 
4. user.like_articles: 유저가 좋아요한 게시글 (M:N) 



####  (2) related_name 속성을 사용하지 않았다면, 오류가 발생한다. 

like_users 필드 생성 시 자동으로 역참조는 .article_set 매니저를 생성한다. 

이전에 생성했던 1:N 관계에서 이미 해당 매니저의 이름을 사용 중이기 때문에 

1. User와 관계된 ForeignKey 
2. 또는 ManyToManyField 중 하나에related_name이 추가해서 필요하다

```python
# 역참조 매니저 이름이 겹친다.
SystemCheckError: System check identified some issues:

ERRORS:
articles.Article.like_user: (fields.E304) Reverse accessor for 'articles.Article.like_user' clashes with reverse accessor for 'articles.Article.user'.
        HINT: Add or change a related_name argument to the definition for 'articles.Article.like_user' or 'articles.Article.user'.
articles.Article.user: (fields.E304) Reverse accessor for 'articles.Article.user' clashes with reverse accessor for 'articles.Article.like_user'.     
        HINT: Add or change a related_name argument to the definition for 'articles.Article.user' or 'articles.Article.like_user'.

```

<br>

#### (3) 구현

- url 생성

```python
# articles/urls.py

from django.urls import path
from . import views


app_name = 'articles'
urlpatterns = [
    ...
    path('<int:article_pk>/likes/', views.likes, name='likes'),
]

```

<br>

- 좋아요를 누르면 DB에 기록하는 view 함수

```python
# articles/views.py

@require_POST
def likes(request, article_pk):
    if request.user.is_authenticated:
        article = get_object_or_404(Article, pk=article_pk)
		
        # 게시글에 이미 좋아요를 누른유저가 다시 누르면 좋아요를 취소시킨다. 
        # if request.user in article.like_users.all():exists에 비해 효율성이 떨어진다.
        if article.like_users.filter(pk=request.user.pk).exists(): 
            article.like_users.remove(request.user) 
        else:
            article.like_users.add(request.user)
        return redirect('articles:index')
    return redirect('acoounts:login')

```

- QuerySetAPI `exists()`
  - QuerySet에 결과가 포함되어 있으면 True를 반환하고 그렇지 않으면 False를 반환한다.
  - 특히 규모가 큰 QuerySet의 컨텍스트에서 특정 개체 존재 여부와 관련된 검색에 유용하다.
  - 고유한 필드(예: primary key)가 있는 모델이 QuerySet의 구성원인지 여부를 찾는 가장 효율적인 방법이다. 

<br>

- index.html에 좋아요 버튼을 추가한다. (내가 작성한 게시글에는 좋아요를 누르는 버튼이 보이지 않는다.) 

```django
<!-- articles/index.html --> 

.. 
    <div>
      <form action="{% url 'articles:likes' article.pk %}" method="POST">
        {% csrf_token %}
        {% if user in article.like_users.all %}
          <input type="submit" value="좋아요 취소">
        {% else %}
          <input type="submit" value="좋아요">
        {% endif %}
      </form>
    </div>
...
```

<br>

## 3. 프로필 페이지

#### (1) 구현

- url 생성

```python
# accounts/urls.py

from django.urls import path
from . import views


app_name = 'accounts'
urlpatterns = [
	...
    # 문자열을 사용할 때 주의사항!!!
    # 해당 url을 가장 맨 위에 올렸을 때의 문제: 유저의 경로가 accounts/문자열 이면 무조건 이 url에 걸린다.
    path('<username>/', views.profile, name='profile'),
]
```

<br>

- 유저의 프로필화면을 render시키는 view함수 

```python
# accounts/views.py
from django.shortcuts import render, redirect, get_object_or_404
from django.contrib.auth import get_user_model

def profile(request, username):
    User = get_user_model()

    # 어차피 username은 유일한 값이기 때문에 꼭 pk를 사용하지 않아도 괜찮다.
    person = get_object_or_404(User, username=username)
    context = {
        'person': person,
    }

    return render(request, 'accounts/profile.html', context)
```

<br>

- 프로필 페이지 생성

```html
<!-- accounts/profile.html-->

{% extends 'base.html' %}
{% block content %}
  <h1>{{ person.username }}님의 프로필</h1>

  <hr>
  <!-- 이 사람이 작성한 게시글 목록 -->
  <h2>{{ person.username }}이 작성한 게시글</h2>
  {% for article in person.article_set.all %}
  <p>{{ article.title }}</p>
  {% endfor %}

  <hr>
  <!--  이 사람이 작성한 댓글 목록 -->
  <h2>{{ person.username }}이 작성한 댓글</h2>
  {% for comment in person.comment_set.all  %}
  <p>{{ comment.content }}</p>
  {% endfor %}

  <hr>
  <!-- 이 사람이 좋아요를 누른 게시글 목록 --> 
  <h2>{{ person.username }}이 좋아요를 누른 게시글</h2>
  {% for article in person.like_articles.all  %}
  <p> {{ article.title }}</p>
  {% endfor %}

  <hr>

  <a href="{% url 'articles:index' %}">Back</a>
{% endblock content %}

```

<br>

- index.html에서 작성자의 프로필에 바로 방문할 수 있도록 링크를 생성한다.

```html
<!-- articles/index.html --> 

 {% for article in articles %}
    <p>작성자:
      <!-- 작성한 사람의 프로필에 방문 해야 한다. --> 
      <a href="{% url 'accounts:profile' article.user.username %}">
        {{ article.user }}</a>
    </p>
    <p>글 번호:
      {{ article.pk }}</p>
    <p>글 제목:
      {{ article.title }}</p>
    <p>글 내용:
      {{ article.content }}</p>
    <div>
      <form action="{% url 'articles:likes' article.pk %}" method="POST">
        {% csrf_token %}
        {% if user in article.like_users.all %}
          <input type="submit" value="좋아요 취소">
        {% else %}
          <input type="submit" value="좋아요">
        {% endif %}
      </form>
    </div>
    <a href="{% url 'articles:detail' article.pk %}">DETAIL</a>
    <hr>
  {% endfor %}
```

<br>

## 4. 팔로우

>  User와 User간의 M:N 관계이다. (재귀)

#### (1) models.py

```python
# accouts/models.py

from django.db import models
from django.contrib.auth.models import AbstractUser

# Create your models here.

class User(AbstractUser):
    followings = models.ManyToManyField('self', symmetrical=False, related_name='followers')
```

`symmetrical`

- ManyToManyField가 동일한 모델(on self)을 가리키는 정의에서만 사용한다.
- symmetrical=True(기본 값)일 경우 Django는 models_set 매니저를 추가하지 않는다. 
  - Source model의 인스턴스가 target 모델의 인스턴스를 참조하면, target 모델 인스턴스도 source 모델 인스턴스를 자동으로 참조하도록 한다.
  - 즉, 내가 당신의 친구라면 당신도 내 친구가 되는 것이다.
- **대칭을 원하지 않는 경우 False로 설정해야 한다.** 

<br>

`accounts_user_followings` (생성된 중개 테이블)

| id        | from_user_id | to_user_id |
| --------- | ------------ | ---------- |
| `integer` | `bigint`     | `bigint`   |

<br>

#### (2) 구현

- url 생성

```python
# accounts/urls.py

from django.urls import path
from . import views


app_name = 'accounts'
urlpatterns = [
	...
    path('<int:user_pk>/follow/', views.follow, name='follow'),
]

```

<br>

- 팔로우한 뒤, 이를 DB에 저장하는 함수 생성

```python
@require_POST
def follow(request, user_pk):
    if request.user.is_authenticated:
        you = get_object_or_404(get_user_model(), pk=user_pk)
        me = request.user

        # 내가 나 스스로를 팔로우하는 것을 막는다.
        if me != you:
            # 언팔로우
            # if me in you.followers.all():
            if you.followers.filter(pk=me.pk).exists():
                you.followers.remove(me)
            # 팔로우
            else:
                you.followers.add(request.user)

        return redirect('accounts:profile', you.username)
    return redirect('accounts:login')
```

<br>

- profile 페이지에 팔로우와 언팔로우 버튼 추가 & 팔로잉 수, 팔로워 수 출력 

```html
 <!-- accounts/profile.html --> 

  {% with followers=person.followers.all followings=person.followings.all  %}
    <div>
      팔로워 : {{ followers|length }} / 팔로우: {{ followings|length}}
    </div>
	
	<!--팔로우 언팔로우 버튼은 타인만 볼 수 있도록 설계 -->
    <div>
      {% if user != person %}
        <form action="{% url 'accounts:follow' person.pk %}" method="POST">
          {% csrf_token %}
          {% if user in followers %}
          <input type="submit" value="언팔로우">
          {% else %}
          <input type="submit" value="팔로우">
          {% endif %}
        </form>
      {% endif %}
    </div>
  {% endwith %}
```

<br>

---

**중개 테이블의 필드 생성 규칙**

- source model 및 target model 모델이 다른 경우
  - `id`
  - `<containing_model>_id`
  - `<other_model>_id`
- ManyToManyField가 동일한 모델을 가르키는 경우
  - `id`
  - `from_<model>_id`
  - `to_<model>_id`
