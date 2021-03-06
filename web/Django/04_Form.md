# Form

[toc]

> Django의 form기능은 유효성 검증을 편리하게 하는 기능을 제공한다. 

## 1. Django Form class

> Form은 DJango의 유효성 검사 도구 중 하나로 외부의 악의적 공격 및 데이터 손상에 대한 중요한 방어 수단이다. 
>
> HTML input요소에 매핑된다. 

`기능`

- 렌더링을 위한 데이터 준비 및 재구성
- 데이터에 대한 HTML forms 생성
- 클라이언트로부터 받은 데이터 수신 및 처리 

<br>

### 1) Form 선언하기

> forms 라이버러리에서 파생된 Form 클래스를 상속받는다. 

```python
# articles/forms.py
from django import forms
from .models import Article

class ArticleForm(forms.Form):
    REGION_A = 'sl'
    REGION_B = 'dj'
    REGION_C = 'gj'
    REGIONS_CHOICES = [
        (REGION_A, '서울'),   # (변수 값, 사용자에게 출력되는 형태)
        (REGION_B, '대전'),
        (REGION_C, '광주'),
    ]
	
    # 데이터가 전달될 때, name-value쌍으로 전달되는데 변수 명이 데이터의 name이다. 
    title = forms.CharField(max_length=10)               
    content = forms.CharField(widget=forms.Textarea)
    region = forms.ChoiceField(choices=REGIONS_CHOICES)
```

- Model을 선언하는 것과 유사하며 같은 필드타입을 사용한다. 
- 참고자료: https://docs.djangoproject.com/ko/4.0/ref/forms/fields/
- 참고자료: https://docs.djangoproject.com/ko/4.0/ref/forms/widgets/

- Form fields: input에 대한 유효성 검사 로직을 처리하며 템플릿에서 직접 사용된다. 
- widget: 웹 페이지의 HTML input 요소를 렌더링한다. 
  - GET/POST 딕셔너리에서 데이터를 추출한다. 
  - widgets은 반드시 Form fields에 할당 된다. 

<br>

#### (1) widgets

필드에는 위젯에 대한 default가 설정되어 있다. https://docs.djangoproject.com/ko/4.0/ref/forms/fields/#built-in-fields

그러나, 필드에 다른 위젯을 사용하려는 경우 필드를 정의할 때 widget을 인자로 사용하여 수정할 수 있다. 

```python
 content = forms.CharField(widget=forms.Textarea)
```

widget외에도 `required`, `label`, `initial`, `error_message`, `disabled`와 같은 필드의 공통인자가 있다. 

<br>

### 2) views.py

#### (1) 단순하게 form을 랜더링해서 보여줄 때

```python
# form을 추가
def new(request):
    form = ArticleForm()
    context = {
        'form': form
    }
    return render(request, 'articles/new.html', context)
```

<br>

#### (2) 해당 form 태그로 받은 데이터를 저장한다면? 

```python
def create(request):
    title = request.POST.get('title')        # title(name)의 데이터를 title변수에 저장
    content = request.POST.get('content')    # content(name)의 데이터를 content변수에 저장

	# DB에 저장
    article = Article(title=title, content=content)
    article.save()
    
    return redirect('articles:detail', article.pk)
```

<br>

#### (3) 유효성 검증과정을 진행한 뒤에 데이터를 저장한다면?

```python
def create(request):
    if form.is_vaild():
        # article = Article(**form.cleaned_data)
        title = form.cleaned_data['title']
        content = form.cleaned_data['content']
        article = Article(title=title, contentㅊ=content)
        article.save()
    
    return redirect('articles:detail', article.pk)
```

- Form 인스턴스에는 모든 필드에 대해서 유효성 검사를 실행하는 메서드 `is_vaild()`가 있다. 
  - 모든 필드에 유효한 데이터가 포함되어 있다면, True를 반환한다. 
  - 데이터를 해당 `cleaned_data`속성에 배치한다. 
- 즉, 유효성 검증이 끝나면 데이터는 `form.cleaned_data`에 딕셔너리형태로 저장되어 있다. 

<br>

`유효성 검사`

- 요청한 데이터가 특정 조건에 충족하는지 확인하는 작업이다.
- 데이터베이스 각 필드 조건에 올바르지 않은 데이터가 서버로 전송되거나 저장되지 않도록 한다. 

<br>

### 3) html(랜더링)

```html
{% extends 'base.html' %} 
{% block content %}
<form action="{% url 'articles:create' %}" method="POST">
  {% csrf_token %} 
  {{ form.as_p }} 
  <input type="submit" />
</form>
{% endblock content %}
```

#### (1) Form rendering options

참고자료: https://docs.djangoproject.com/ko/4.0/topics/forms/#working-with-form-templates

- `{{ form.as_table }}`: 각 필드가 테이플 (<tr>태그)행으로 감싸져서 랜더링 된다. 
- `{{ form.as_p }}`: 각 필드가 단락 (<p>태그)으로 감싸져서 랜더링 된다. 
- `{{ form.as_ul }}`: 각 필드가 목록 항목 (<li>태그)행으로 감싸져서 랜더링 된다. 

주변 요소 `<table>`또는 `<ul>` 요소는 직접 기입해야 한다. 

```html
{% extends 'base.html' %} 
{% block content %}
<table>
  <form action="{% url 'articles:create' %}" method="POST">
    {% csrf_token %} 
     {{ form.as_table }} 
  <input type="submit" />
</table>
{% endblock content %}
```

<br>

#### (2) Rendering fields manually

> 수동으로 필드를 재정렬할 수 있다. 각 필드를 사용하여 양식의 속성으로 사용할 수 있으며, Django 템플릿에서 적절하게 렌더링 된다. 

참고자료: https://docs.djangoproject.com/ko/4.0/topics/forms/#rendering-fields-manually

```python
 <h2>1. Rendering fields manually</h2>
  <form action="{% url 'articles:create' %}" method="POST">
    {% csrf_token %}
    <div>
      {{ form.title.errors }}
      {{ form.title.label_tag }}
      {{ form.title }}
    </div>
    <div>
      {{ form.content.errors }}
      {{ form.content.label_tag }}
      {{ form.content }}
    </div>
    <input type="submit">
  </form>
  <hr>
    
  <h2>2. Looping over the form’s fields</h2>
  <form action="{% url 'articles:create' %}" method="POST">
    {% csrf_token %}
    {% for field in form %}
      {% if field.errors %}
        {% for error in field.errors %}
          <div class="alert alert-warning">{{ error }}</div>
        {% endfor %}
      {% endif %}
      {{ field.label_tag }}
      {{ field }}
    {% endfor %}
    <input type="submit">
  </form>
```

<br>

## 2. ModelForm

> Model을 통해 Form Class를 만든다. 

- 모델 폼을 사용한다는 것은 데이터베이스에 저장한다는 의미이다. ex) 회원가입
- 로그인과 같은 폼에서는 데이터베이스에 저장을 하기 위한 것이 아니기때문에 일반적인 form을 사용한다. 

- 참고자료: https://docs.djangoproject.com/en/4.0/topics/forms/modelforms/

<br>

`모델폼이 쉽게 해주는 것`

- 모델 필드 속성에 맞는 html element를 만둘어준다.
- 이릍 통해 받은 데이터를 view 함수에서 유효성 검사를 할 수 있도록 한다. 

<br>

### 1) Form 선언하기

```python
# articles/forms.py
from django import forms
from .models import Article

class ArticleForm(forms.ModelForm):
    title = forms.CharField(
        widget=forms.TextInput(
            attrs={
                'class': 'my-class',
                'placeholder': 'Enter the title',
                'maxlength': 10,
            }
        )
    )
    content = forms.CharField(
        widget=forms.Textarea(
            attrs={
                'class': 'my-content',
                'row': 5,
                'cols': 50,
            }
        ),
        error_messages={
            'required': 'ㅈ[대로 콘ㅌ[ㄴ츠 값을 넣으ㅅ[요!!!!!'
        }
    )

    # 모델의 정보를 작성하는 곳
    # 메타 데이터: 데이터에 대한 데이터. ex) 사진 : 데이터/ 사진의 메타데이터: 날짜, 장소 등
    class Meta:
        model = Article
        
        # 어떤 필드를 출력할 것인지에 대해 기입하고 싶다면 다음과 같은 키워드를 사용한다. 
        # fields = ['title, 'content']
        fields = '__all__'

        # 필드가 여러가지 일 때, 특정 필드만 출력을 제외하고 싶다면 다음과 같이 키워드를 사용할 수 있다. 
        # fields 속성과 동시에 사용할 수 없다.
        exclude = ('title', )
```

- forms 라이브러리에서 파생된 ModelForm 클래스를 상속받는다.
- 정의한 클래스 안에 Meta 클래스를 선언하고, 어떤 모델을 기반으로 Form을 작성할 것인지에 대한 정보를 Meta클래스에 지정한다. 
- 클래스 변수 fields와 exclude는 동시에 사용할 수 없다. 

<br>

#### (1) Meta class

- Model의 정보를 작성하는 곳이다. 
- ModelForm을 사용할 경우 사용할 모델이 있어야 하는데 Meta Class가 이를 구성한다. 
- 해당 Model에 정의한 field 정보를 Form에 적용하기 위함이다.

<br>

#### (2) widget

> Django의 HTML input element를 표현한다. 
>
> model field 에 따라 form field 가 결정되고 form field 에 따라 Widget 이 결정된다.

위의 방식 처럼 메타클래스와 따로 위젯을 기입하는 것을 추천한다. 

`다른 방식(비추천)`

```python
class ArticleForm(forms.ModelForm):
    
    class Meta:
        model = Ariticle
        fields = '__all__'
        widgets = {
            'title': forms.TextInput(arrts={
                'class': 'my-title',
                'placeholder': 'Enter the title'
                'maxlength': 10,
            }
          )
        }
```

<br>

### 2) view함수

`new 함수 + create 함수`

```python
def create(request):
    if request.method == 'POST':
        # create
        form = ArticleForm(request.POST)
        if form.is_valid():
            article = form.save()
            return redirect('articles:detail', article.pk)
    # new
    else:
        form = ArticleForm()

    # get 호출: form에는 else에서 form변수에 정의한 데이터가 있다. 
    # 유효성 검사 실패: form에는 에러메시지가 담겨 있다.(form.error)
    context = {
        'form': form
    }
    return render(request, 'articles/create.html', context)
```

<br>

#### (1) save()

> Form 바인딩 된 데이터에서 데이터베이스 객체를 만들고 저장한다. 

- ModelForm의 하위 클래스는 기존 모델 인스턴스 키워드 인자 `instance`로 받아 들일 수 있다. 

  - 이것이 제공되면 save()는 해당 인스턴스를 수정한다 (UPDATE)

  - 제공되지 않은 경우 save()는 지정된 모델의 새 인스턴스를 만든다.(CREATE)

  - ```python
    def update(request, pk):
        article = Article.objects.get(pk=pk)
        if request.method == "POST":
            # update (새로운 게시글 작성이 아닌 수정이다.)
            form = ArticleForm(request.POST, instance=article)
            if form.is_valid():
                article = form.save()   
            return redirect('articles:detail', article.pk)
    
        else:
            # edit
            form = ArticleForm(instance=article) 
        context = {
            'article': article,
            'form': form,
        }
        return render(request, 'articles/update.html', context)
    ```

- Form의 유효성이 확인되지 않은 경우 save()를 호출하면 form.errors를 확인하여 에러 확인이 가능하다.

<br>

## 비교, 기타

| Form                                                         | ModelForm                                                    |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 어떤 Model에서 저장해야 하는지 알 수 없으므로 유효성 검사 이후 cleaned_data 딕셔너리를 생성한다. | Django가 해당 model에서 양식에 필요한 대부분의 정보를 이미 정의한다. |
| Cleaned_data 딕셔너리에서 데이터를 가져온 후 `.save()`를 호출 해야 한다. | 어떤 레코드를 만들어야 할 지 알고 있으므로 바로 `.save()`호출이 가능하다. |
| Model에 연관되지 않은 데이터를 받을 때 사용한다.             |                                                              |

<br>

`부트스트랩`

```python
<!DOCTYPE html>
{% load bootstrap5 %}
<html lang="en">

  <head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    {% bootstrap_css %}
    <title>Document</title>
  </head>

  <body>
    <div class="container">
      {% block content %}{% endblock content %}
    </div>
    {% bootstrap_javascript%}
  </body>

</html>
```

```python
def delete(request, pk):
    article = Article.objects.get(pk=pk)
    if request.method == 'POST':
        article.delete()
        return redirect('articles:index')
    return redirect('articles:detail', article.pk)

```

