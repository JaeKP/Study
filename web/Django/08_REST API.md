# REST API

[toc]

## 1. HTTP

> Hyper Text Transfer Protocol

[공식문서](https://developer.mozilla.org/ko/docs/Web/HTTP/overview)

- 웹 상에서 컨텐츠를 전송하기 위한 약속이다.
- HTML 문서와 같은 리소스들을 가져올 수 있도록 하는 프로토콜(규칙, 약속)이다.
- 웹에서 이루어지는 모든 데이터 교환의 기초
  - 요청(request): 클라이언트에 의해 전송되는 메시지
  - 응답(response): 서버에서 응답으로 전송되는 메시지
- `stateless`,  `connectionless`의 특성이 있어 쿠키와 세션을 통해 서버상태를요청과 연결하도록 함



`HTTP 요청의 예`

![A basic HTTP request](https://mdn.mozillademos.org/files/13687/HTTP_Request.png)

`HTTP 응답의 예`

![img](https://mdn.mozillademos.org/files/13691/HTTP_Response.png)





### 1) HTTP request methods

> 자원에 대한 행위(수행하고자 하는 동작)을 정의

- 주어진 리소스(자원)에 수행하길 원하는 행동을 나타낸다
  - 간혹 메서드를 "HTTP 동사"라고 부른다.
- `GET`, `POST`, `PUT`, `DELETE`



### 2) Status code

> 특정 HTTP 요청이 성공적으로 완료되었는지 여부를 나타낸다.

1. [Informational responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#information_responses) (`100`–`199`)
2. [Successful responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#successful_responses) (`200`–`299`)
3. [Redirection messages](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#redirection_messages) (`300`–`399`)
4. [Client error responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#client_error_responses) (`400`–`499`)
5. [Server error responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#server_error_responses) (`500`–`599`)



### 3) resource

> HTTP는 HTML 문서와 같은 리소스들을 가져올 수 있도록 해주는 프로토콜이라고 한다. 

- HTTP 요청의 대상을 리소스 (resource, 자원)라고 한다.
- 리소스는 문서, 사진 또는 기타 어떤 것이든 될 수 있다.
- **각 리소스는 리소스 식별을 위해 HTTP 전체에서 사용되는 URI(Uniform Resource Identifier)로 식별된다..**



#### (1) URL(Uniform Resource Locator)

- 통합 자원 위치
- 네트워크 상에 어디있는지 알려주기 위한 약속
- 관거에는 실제 자원의 위치를 나타냈지만 추상환된 의미론적인 구성
- '웹 주소', '링크'라고도 불린다.



#### (2) URN (Uniform Resource Namge)

- 통합 자원 이름
- URL과 달리 자원의 위치에 영향을 받지 않는 유일한 이름 역할을 한다.
- 예시)  ISBN(국제 표준 도서 번호)



### 4) URI(Uniform Resource Identifier)

> 통합 자원 식별자

- 인터넷의 자원을 식별하는 유일한 주소(정보의 자원을 표현한다.)
- 인터넷에서 자원을 식별하거나 이름을 지정하는데 사용되는 간단한 문자열
- 하위개념
  - URL
  - URN

즉, URI는 크게 URL과 URN으로 나눌 수 있지만, URN을 사용하는 비중이 매우 적기 때문에 일반적으로 URL은 URI와 같은 의미처럼 사용하기도 한다.



#### (1) Schema (protocol)

**http://**`www.example.com:80/path/to/myfile.html/?key=value#quict-start`

- 브라우저가 사용해야 하는 프로토콜
- 서로 데이터를 주고받을 때 어떤 메시지를 주고 응답을 받을지에 대한 약속이다.
  - **서버와 클라이언트가 서로 통신을 하기위해 요청과 응답하는 절차에 대한 규약(약속)**
- `http(s)`, `data`, `file`, `ftp`, `mailto`



#### (2) HOST (Domain name)

http://**`www.example.com`**:80/path/to/myfile.html/?key=value#quict-start



**`www.naver.com ` ====여기 어디야..?==> DNS(Domain name server)**

**`www.naver.com ` <==IP주소를 반환== DNS(Domain name server)**



- 요청받는 웹 서버의 이름
- IP address를 직접 사용할 수도 있지만, 실사용시 불편하기때문에 웹에서 그리 자주 사용되지는 않는다. 
  - **하지만, DNS를 통해 IP로 매핑이된다.** 



#### (3) Port

`http://www.example.com`**:80**`/path/to/myfile.html/?key=value#quict-start`

> 웹 서버 상의 리소스에 접근하는데 사용되는 기술적인 '문(gate)'

- 컴퓨터 안에 실제 메시지를 받아야하는 프로그램이 있다. 
  - 이러한 프로세스 마다 메시지를 받는 채널을 가지고 있고 이를 Port라고 한다. 
- **IP는 통신할 하드웨어를 지정하고, PORT는 통신할 프로세스를 지정한다.**
- 웹에서는 URI에 기입하지 않는 편이다. (이미 고정된 포트가 있음)
  - 컴퓨터가 웹 서버를 동작시킬 때는, 웹서버는 80포트를 열고 메시지를 수신하도록 기다리고 있다. 
  - `HTTP 80`, `HTTP 443`



(1)~(3)은 웹 서버까지 찾아가는 과정이다.

사실, 웹서버 입장에서는 여기서부터 중요하다. 

#### (4) Path

`http://www.example.com:80`**/path/to/myfile.html/**`?key=value#quict-start`

> 웹 서버 상의 리소스 경로

- 초기에는 실제 파일이 위치한 물리적 위치를 나타냈지만, 오늘날은 물리적인 실제 위치가 아닌 추상화 형태의 구조로 표현한다.

실제로 데이터를 주고 받는 것은 프로세스 간의 통신이다. 프로세스는 OS에 있다가 실행되면 메모리를 차지한다.



#### (5) Query (Identifier)

`http://www.example.com:80/path/to/myfile.html/`**?key=value**`#quict-start`

> Query String Parameters

- 웹 서버에 제공되는 추가적인 매개 변수이다.
- &로 구분되는 key-value의 목록이다.



#### (6) Fragment

`http://www.example.com:80/path/to/myfile.html/?key=value`**#quict-start**

> Anchor
>
> 자원 안에서의 북마크의 한 종류를 나타냄

- 브라우저에게 해당 문서 (HTML)의 특정 부분을 보여주기 위한 방법이다.
- 브라우저에게 알려주는 요소이기 떄문에 fragment identifier(부분 식별자)라고 부르며 **`#` 뒤의 부분은 요청이 서버에 보내지지 않는다.**



## 2. RESTful API

### 1) API

>  Application Prograimming Interface
>
> 프로그램들 사이에 소통하기 위한 인터페이스

-  API를 통해 프로그램들 사이에서 데이터를 주고 받는다. 
- 프로그래밍 언어가 제공하는 기능을 수행할 수 있게 만든 인터페이스이다. 
  - CLI는 명령줄, GUI는 그래픽(아이콘), API는 프로그래밍을 통해 특정한 기능을 수행한다. 



#### (1) 과거 어플리케이션들의 통신

|                                              | 설명                                                         |
| -------------------------------------------- | ------------------------------------------------------------ |
| OS와 어플리케이션의 통신                     | 어플리케이션 <=> 운영체제<br />콜백 함수를 등록한다. 어플리케이션에서 특정 이벤트가 발생하면, 이를 OS가 읽고 함수를 실행시켜주거나 데이터가 불러왔다. |
| 같은 하드웨어에 있는 어플리케이션끼리의 통신 | 프로세스간의 통신이다. 이는 같은 하드웨어에 있기 때문에 메모리를 공유하면 된다.<br />ex) 워드 프로그램에서 복사해서 파워포인트에 붙여넣기 |
| 다른 하드웨어에 있는 프로세스끼리 통신       | `RPC (Remote Procedure Call)`<br />process ========함수 호출(request) =====> process<br />process <=======함수 결과(response)====== process<br />마치 나의 로컬이 있는 함수를 호출하듯이 별도의 원격제어 없이 다른 주소 공간에 있는 함수나 프로시저를 실행할 수 있게 하는 프로세스간 통신을 의미한다. |
| 객체 지향 프로그래밍이 대두                  | `RMI(Remote Method Invocation)`<br />process안에 있는 객체 <==================> 다른 process안에 있는 객체<br />위와 같이 마치 로컬에 있는 객체를 호출 하듯이 다른 프로세스에 있는 객체를 호출해서 서로 데이터를 주고 받는다. |
| `RPC`, `RMI` 의 단점 해결 <br />             | `RPC`, `RMI` 둘다 큰 단점이 있다. 이질적인 환경(개발언어, 운영체제, 하드웨어 환경 등)에서 사용하기 어렵다는 것이다.<br />그래서 나온 대안이 `웹서비스`이다.<br /><br />http 프로토콜을 사용(웹서버) 웹서버를 두고 통신하고 싶은 프로그램을 둔다. <br />앞에 request를 받는 웹서버를 두고 정해진 규격대로 서로 통신한다. ( url로 접근)<br /><br />SOAP(프로토콜) ==> XML (XML 형태로 데이터를 보낸다.)<br />REST => json (json 형태로 데이터를 보낸다.) |



#### (2)WEB API

- 웹 애플리케이션 개발에서 다른 서비스에 요청을 보내고 응답을 받기 위해 정의된 명세이다.
- 현재 웹 개발은 모든 것을 직접 개발하기보다 여러 Open API를 활용하는 추세이다.

- 응답 데이터 타입
  - HTML, XML, JSON 등



#### (3) REST (REpresentational State Transfer)

- API Server를 개발하기 위한 일종의 소프트웨어 설계 방법론이다.
- 네트워크 구조 원리의 모음이다.
  - 자원을 정의하고 자원에 대한 주소를 지정하는 전반적인 방법
  - 설계 방법론을 지키지 않더라도 동작 여부에는 큰영향을 주지는 않는다. 
- REST 원리를 따르는 시스템을 RESTful이란 용어로 지칭한다.



**`자원`: URI**

**`행위`: HTTP Method**

**`표현`: 자원과 행위를 통해 궁극적으로 표현되는 추상화된 결과물, JSON으로 표현된 데이터를 제공**



`핵심 규칙`

- 정보(자원)은 URI로 표현한다.
- 자원에 대한 '행위'는 HTTP Method로 표현한다 (GET, POST, PUT, DELETE)

**=> 하나의 함수에 여러개를 구현할 수 있게 된다.** 

​	

즉, 데이터를 받는 입장에서 HTML과 같은 문서를 받는 것이 아니라 데이터를 받는 것이다. 그리고 받은 데이터를 가공해서 웹페이지에 반영한다.

1. 사용자가 도메인 or URL 로 요청을 보낸다 
2. 서버가 데이터를 처리해서 HTML에 넘겨준다 
3. HTMl 문서를 받는다 
4. 페이지는 유지한 체로 서버에 계속 데이터를 요청한다.
5. JSON 으로 데이터를 받아서 이를 웹페이지에 반영한다. 

만약 JSON이 아닌 HTMl을 보냈다면 웹페이지가 계속 새로고침되었을 것!



### (4) Json (Javascript Object Notation)

- Javascript의 표기법을 따른 단순 문자열
- 사람이 읽거나 쓰기 쉽고 기계가 파싱(해석, 분석)하고 만들어내기 쉽다.
- 파이썬의 dictionary, 자바스크립트의 object처럼 C계열의 언어가 갖고있는 자료구조로 쉽게 변화할 수 있는 key-value 형태의 구조를 갖고 있다. 



## 3. Response

### 1) 사전 작업

```python
# settings.py
INSTALLED_APPS = [
    ....
    'django_seed',
    'rest_framework',
    ....
    ]

```



 ```bash
 # 더미 데이터 추가
 $ python manage.py seed articles --number=20
 ```



### 2) Response 비교

#### (1) views.py

```python
from rest_framework.decorators import api_view
from rest_framework.response import Response
from django.http.response import JsonResponse, HttpResponse
from django.core import serializers
from django.shortcuts import render

from .serializers import ArticleSerializer
from .models import Article


def article_html(request):
    articles = Article.objects.all()
    context = {
        'articles': articles,
    }
    return render(request, 'articles/article.html', context)


# JsonResponse 객체를 활용한 JSON 데이터 응답
def article_json_1(request):
    articles = Article.objects.all()
    articles_json = []

    for article in articles:
        articles_json.append(
            {
                'id': article.pk,
                'title': article.title,
                'content': article.content,
                'created_at': article.created_at,
                'updated_at': article.updated_at,
            }
        )
    return JsonResponse(articles_json, safe=False)


# Django의 내장 HttpResponse를 활용한 Json 응답
# 주어진 모델 정보를 활용하기 때문에 필드를 개별적으로 직접 만들어 줄 필요가 없다. 
# 쿼리셋 => python => json
def article_json_2(request):
    articles = Article.objects.all()
    data = serializers.serialize('json', articles)  
    return HttpResponse(data, content_type='application/json')


# Django REST framework(DRF)라이브러리를 사용한 JSON 응답
@api_view()
def article_json_3(request):
    articles = Article.objects.all()
    # 옵션 many: 앞에오는 객체가 단일객체가 아닐때 True로 설정해줘야 한다.
    # articles는 쿼리셋이기 때문에 단일객체가 아니다!
    serializer = ArticleSerializer(articles, many=True)
    return Response(serializer.data) # Serialize된 Json객체 응답

```

`JsonResponse`: HttpResponse의 서브 클래스로서, 데이터를 JSON으로 serialize 한다.

- 필수 인자 : `data` ( Json으로 변환될 데이터로 기본적으로 dictionary 객체이다.)  
-  `safe` (dictionary 객체만 serialize할 수 있는지 여부를 파악한다. default는 True이다. )



`serializers.serialize`: 쿼리셋을 serialize한다. 

- 필수 인자: `format`(어떤 형태로 serialize할 것인지 ) , `queryset` (serialize할 쿼리셋)



`.data`

```python
    @property
    def data(self):
        ret = super().data
        return ReturnDict(ret, serializer=self)
```

```python
class ReturnDict(OrderedDict):
    """
    Return object from `serializer.data` for the `Serializer` class.
    Includes a backlink to the serializer instance for renderers
    to use if they need richer field information.
    """

    def __init__(self, *args, **kwargs):
        self.serializer = kwargs.pop('serializer')
        super().__init__(*args, **kwargs)

    def copy(self):
        return ReturnDict(self, serializer=self.serializer)

    def __repr__(self):
        return dict.__repr__(self)

    def __reduce__(self):
        # Pickling these objects will drop the .serializer backlink,
        # but preserve the raw data.
        return (dict, (dict(self),))
```

- [공식문서](https://www.django-rest-framework.org/api-guide/serializers/#baseserializer)
-  Returns the outgoing primitive representation. (원시적인 데이터로 변환한다.)
- 문자열 => 딕셔너리



`Response()` [공식문서](https://www.django-rest-framework.org/api-guide/responses/)

`Response(data, status=None, template_name=None, headers=None, content_type=None)`

>  Unlike regular `HttpResponse` objects, you do not instantiate `Response` objects with rendered content. Instead you pass in unrendered data, which may consist of any Python primitives.
>
> 일반 'HttpResponse' 와 달리 렌더링된 내용으로 'Response' 개체를 인스턴스화하지 않는다. Python 데이터로 구성될 수 있는 렌더링되지 않은 원시적인 데이터를 전달한다.
>
> 
>
> The renderers used by the `Response` class cannot natively handle complex datatypes such as Django model instances, so you need to serialize the data into primitive datatypes before creating the `Response` object.
>
> 'Response' 클래스에서 사용되는 renderers는 장고 모델 인스턴스와 같은 복잡한 데이터 유형을 기본적으로 처리할 수 없으므로 'Response' 개체를 만들기 전에 데이터를 원시 데이터 유형으로 serialize 해야 한다.



```python
class Response(SimpleTemplateResponse):
    """
    An HttpResponse that allows its data to be rendered into
    arbitrary media types.
    """

    def __init__(self, data=None, status=None,
                 template_name=None, headers=None,
                 exception=False, content_type=None):
        """
        Alters the init arguments slightly.
        For example, drop 'template_name', and instead use 'data'.

        Setting 'renderer' and 'media_type' will typically be deferred,
        For example being set automatically by the `APIView`.
        """
        super().__init__(None, status=status)
```

- 전달받은 데이터를 랜더하는 HttpResponse이다.



#### (2) serializer.py

[공식문서](https://www.django-rest-framework.org/api-guide/serializers/)

> Serializers allow complex data such as querysets and model instances to be converted to native Python datatypes that can then be easily rendered into `JSON`, `XML` or other content types. Serializers also provide deserialization, allowing parsed data to be converted back into complex types, after first validating the incoming data.

serialzers를 사용하면 쿼리 세트 및 모델 인스턴스와 같은 복잡한 데이터를 네이티브 Python 데이터 유형으로 변환한 다음 `JSON`, `XML`또는 다른 콘텐츠 유형으로 쉽게 렌더링할 수 있습니다. 직렬 변환기는 또한 역직렬화를 제공하여 먼저 들어오는 데이터의 유효성을 검사한 후 구문 분석된 데이터를 복합 유형으로 다시 변환할 수 있도록 합니다.

```python
# serializers.py

from rest_framework import serializers
from .models import Article

# Article 모델에 맞춰 자동으로 필드를 생성해 serialize 해주는 ModelSerializer 확인할 수 있다.
# 즉, 게시글에 대한 쿼리셋을 시리얼라이즈해주는 도구이다.
class ArticleSerializer(serializers.ModelSerializer):

    class Meta:
        model = Article
        fields = '__all__'
```

**`ModelSerializer` : 모델 필드에 해당하는 필드가 있는 serializer 클래스를 자동으로 만들 수 있는 shortcut**

`ModelSerializer` is just a regular `Serializer`  

* A set of default fields are automatically populated. (기본 필드가 자동으로 채워진다.)
* A set of default validators are automatically populated. (serializer에 대한 유효성 검사기를 자동으로 생성한다.)
* Default `.create()` and `.update()` implementations are provided.



`ModelSerialzer` > `Serializer` > `BaseSerializer`

```python
class BaseSerializer(Field):
    """
    The BaseSerializer class provides a minimal class which may be used
    for writing custom serializer implementations.

    Note that we strongly restrict the ordering of operations/properties
    that may be used on the serializer in order to enforce correct usage.

    In particular, if a `data=` argument is passed then:

    .is_valid() - Available.
    .initial_data - Available.
    .validated_data - Only available after calling `is_valid()`
    .errors - Only available after calling `is_valid()`
    .data - Only available after calling `is_valid()`

    If a `data=` argument is not passed then:

    .is_valid() - Not available.
    .initial_data - Not available.
    .validated_data - Not available.
    .errors - Not available.
    .data - Available.
    """

    def __init__(self, instance=None, data=empty, **kwargs):
        self.instance = instance
        if data is not empty:
            self.initial_data = data
        self.partial = kwargs.pop('partial', False)
        self._context = kwargs.pop('context', {})
        kwargs.pop('many', None)
        super().__init__(**kwargs)
```

**즉 인자로 instance(객체), data를 받을 수 있다. 만약, 데이터가 없다면 유효성 검사를 할 수 없다.**

- 조회 : instance가 필요하다. (현재 저장된 데이터를 serialize해야 하기 때문이다. Json을 생성)
- 생성: data가 필요하다. (현재 사용자가 입력한 data를 통해 deserialize한다. 인스턴스를 생성 )
- 수정: instance, data가 필요하다. (기존의 인스턴스와 업데이트할 데이터가 필요)

- 만약 serialize만 하는 field일 경우: read only 

  - ex) 자동으로 생성되는 pk, 자동으로 날짜를 기록하는 field의 경우 default로 `readonly`이다. 

- 만약 deserialize만 하는 field일 경우: write only

  - 만약 readonly field여야 하는데 이를 세팅하지 않았다면,  밖에서 가져온 데이터를 저장할때 (deserialize할 때) 유효성 검사에서 걸리게 된다.




#### (3) 파싱

```python
# json 파일을 파싱한다면?
from urllib import response
import requests
from pprint import pprint


# 파싱하기 전에 ArticleSerializer라는 도구가 시리얼라이즈 (유연한 데이터로 바꿔줌 (파이썬))해줬기 때문에 가능하다. 
response = requests.get('http://127.0.0.1:8000/api/v1/json-3/')
pprint(response)               # <Response [200]>
pprint(type(response))         # <class 'requests.models.Response'>
pprint(response.json())        # 파싱!  
pprint(type(response.json()))  # <class 'list'> => 파이썬으로 사용할 수 있는 데이터 타입으로 변경  

articles_list = response.json()
for article in articles_list:
    print(article.get('title'))

```



|          | DJango    | DRF        |
| -------- | --------- | ---------- |
| Response | HTML      | JSON       |
| Model    | ModelForm | Serializer |



### 3) Serialization

- "직렬화"
- 데이터 구조나 객체 상태를 동일하거나 다른 컴퓨터 환경에 저장하고, 나중에 재구성할 수 있는 포맷으로 변환하는 과정이다. 
- DJango에서는 Quryset 및 Model Instance와 같은 복잡한 데이터를 Json, XML 등의 유형으로 쉽게 변활 할 수 있는 Python 데이터 타입으로 만들어 준다.



> 객체가 데이터를 가지고 있고 데이터에 대한 행위도 객체의 메서드로 정의된다. 따라서, 모든 데이터는 객체로 표현이 된다.
>
> 어플리케이션 개발자 입장에서는 객체를 보내면 편리하다. 그런데, 객체를 보내는 것은 어렵다. 
>
> (각자의 메모리에 위치한 객체를 내용 그대로 bytestring해서 보낸다면 이질적인 환경에서는 오류가 발생할 수도 있다.)
>
> **즉, 운영체제, 개발환경 등에 따라 메모리에 표현되는 양식이 다르기 때문에 serialize를 해야한다.**



1. **메모리에 있는 객체를 연속적인 바이트 형식을 만들어서 보낸다. (시리얼라이즈)**
   - **객체(인스턴스)를 Json으로 만들 준비...** 
2. **데이터를 받아 해석(파싱)하여 나에게 맞는 객체로 변환한다. (디시니얼라이즈)**
   - **데이터를 통해 객체(인스턴스)를 생성하는.. **



## 4. single Model

> 단일 모델의 data를 serialize하여 Json으로 변환시킨다.

### 1) serializers.py

Model의 필드를 어떻게 serialize할 것인지 설정하는 것이 핵심이다.

```python
from rest_framework import serializers
from .models import Article, Comment

# article 쿼리셋을 시리얼라이즈한다.
class ArticleListSerializer(serializers.ModelSerializer):
    class Meta:
        model = Article
        # 원하는 데이터만 전송가능
        fields= ('id', 'title',)
        
# 단일 객체에 대해 조회할 경우
class ArticleSerializer(serializers.ModelSerializer):
    class Meta:
        model = Article
        fields = '__all__'
```



### 2) URL 패턴

| METHOD | URL                              | 기능         | Status code                    |
| ------ | -------------------------------- | ------------ | ------------------------------ |
| GET    | `/api/v1/articles/`              | 전체 글 조회 | 200<br />없는 경우 404         |
| POST   | `/api/v1/articles/`              | 글 작성      | valid : 201<br />invalid : 400 |
| GET    | `/api/v1/articles/<article_pk>/` | 특정 글 조회 | 200<br />없는 경우 404         |
| PUT    | `/api/v1/articles/<article_pk>/` | 특정 글 수정 | 200<br />invalid : 400         |
| DELETE | `/api/v1/articles/<article_pk>/` | 특정 글 삭제 | 204                            |



### 3) views.py

```python
# 더 정확한 상태 코드 출력으 위함
from  rest_framework import status
from rest_framework.response import Response
from rest_framework.decorators import api_view
from django.shortcuts import render, get_list_or_404, get_object_or_404
from .models import Article,
from .serializers import ArticleListSerializer, ArticleSerializer


@api_view(['GET', 'POST'])
def article_list(request):
    # 전체글 조회
    if request.method == "GET":
        articles =  get_list_or_404(Article)
        # 쿼리문을 serializier
        serializer = ArticleListSerializer(articles, many=True)
        return Response(serializer.data)
    
    # 게시글 새로 작성
    elif request.method == "POST":
        serializer = ArticleSerializer(data=request.data)
        if serializer.is_valid(raise_exception=True):
            serializer.save()
            # return Response(serializer.data, status=201)의 방식은 권장하지 않는다. 
            return Response(serializer.data, status=status.HTTP_201_CREATED)
        # return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST) 


@api_view(['GET', 'DELETE', 'PUT'])
def article_detail(request, article_pk):
    article = get_object_or_404(Article, pk=article_pk)
    if request.method == "GET":
        serializer = ArticleSerializer(article)
        return Response(serializer.data)
    
    elif request.method == "DELETE":
        article.delete()
        # 데이터가 삭제되어서 DB에 없는상태
        data = {
            # url에 있는 pk를 가져오니까 데이터를 삭제해도 사용할 수 있다. 
            'delete': f'데이터 {article_pk}번이 삭제되었습니다.'
        }
        return Response(data, status=status.HTTP_204_NO_CONTENT)
    
    elif request.method == "PUT":
        # 수정의 경우, 기존의 인스턴스와 업데이트할 데이터 둘다 필요하다. 
        serializer = ArticleSerializer(instance = article, data=request.data)
        if serializer.is_valid(raise_exception=True):
            serializer.save()
            return Response(serializer.data)
```

`request.data`

[공식문서](# https://www.django-rest-framework.org/api-guide/requests/)



`api_view decorator`

- 기본적으로 GET 메서드만 허용되며, 다른 메서드 요청에 대해서는 **405 Method Not Allowed**로 응답한다.

- View 함수가 응답해야 하는 HTTP 메서드의 목록을 리스트의 인자로 받는다. 

- DRF에서는 선택이 아닌 필수적으로 작성해야 해당 view함수가 정상적으로 동작한다. 



`raise_exception`

- is_vaild()는 유효성 검사 오류가 있는 경우 serializers.ValidationError 예외를 발생시키는 선택적 raise_exception 인자를 사용할 수 있다.
- DEF 에서 제공하는 기본 예외 처리기에 의해 자동으로 처리되며, 기본적으로 HTTP status code 400을 응답으로 반환한다. 



## 4. 1:N Relation

> 1:N 관계에서의 모델 data를 직렬화(serialization)하여 Json으로 변환하는 방법에 대한 학습

- 2개 이상의 1:N 관계를 맺는 모델을 두고 CRUD 로직을 수행 가능하도록 설계한다.

#### 1) serializers.py

```python
class CommentSerializer(serializers.ModelSerializer):
    class Meta:
        model = Comment
        fields = '__all__'
        # 클라이언트가 반환을 받아도 읽기만 가능하다
        read_only_fields = ('article',)
```

​	

#### 2) views.py

```python
# 더 정확한 상태 코드 출력으 위함
from  rest_framework import status

from rest_framework.response import Response
from rest_framework.decorators import api_view
from django.shortcuts import render, get_list_or_404, get_object_or_404
from .models import Article, Comment
from .serializers import ArticleListSerializer, ArticleSerializer, CommentSerializer


@api_view(['GET'])
def comment_list(request):
    comments = get_list_or_404(Comment)
    serializer = CommentSerializer(comments, many=True)
    return Response(serializer.data)

@api_view(['GET', 'DELETE', 'PUT'])
def comment_detail(request, comment_pk):
    comment = get_object_or_404(Comment, pk=comment_pk)
    if request.method == "GET":
        serializer = CommentSerializer(comment)
        return Response(serializer.data)

    elif request.method == "DELETE":
        comment.delete()
        data = {
            'delete': f'데이터 {comment_pk}번이 삭제되었습니다.'
        }
        return Response(data, status=status.HTTP_204_NO_CONTENT)

    elif request.method == "PUT":
        serializer = CommentSerializer(instance = comment, data=request.data)
        if serializer.is_valid(raise_exception=True):
            serializer.save()
            return Response(serializer.data)

@api_view(['POST'])
def comment_create(request, article_pk):
    article = get_object_or_404(Article, pk=article_pk)
    serializer = CommentSerializer(data=request.data)
    # 400 request: is_vauld를 통과하지 못했다. => 몇번 게시글에 작성하는 코멘트인지에 대한 정보를 기입해야 한다. 
    # 유효성 검사를 진행할때 시리얼라이즈에 정의된 필드에 대해서 검사를 하게된다. 그래서 추가로 넣기전에 유효성 검사에서 걸린다. 
    # 그래서 유효성 검사에서 article fk 필드는 유효성 검사에서 빼줘야 한다. 읽기 전용필드여야 하는 이유다.  
    if serializer.is_valid(raise_exception=True):
        serializer.save(article=article)
        return Response(serializer.data, status=status.HTTP_201_CREATED)
```



### 3) 기존 필드 override

> Serializer는 기존 필드를 override하거나 추가 필드를 구성할 수 있다. 



#### (1) PrimaryKeyRealatedField

- pk를 사용하여 관계된 대상을 나타내는 데 사용할 수 있다.
- 필드가 to many relationships(N)을 나타내는데 사용되는 경우 many=True 속성이 필요하다.



`특정 게시글에 작성된 댓글 목록 출력하기 `

```python
from rest_framework import serializers
from .models import Article, Comment

class ArticleSerializer(serializers.ModelSerializer):
    # 역참조를 한다는 것은 1인쪽은 DB에 변화가 없기 때문에 이렇게 override한다.
    comment_set = serializers.PrimaryKeyRelatedField(many=True, read_only=True)
    
    class Meta:
        model = Article
        fields = '__all__'

```

- comment_set 필드 값을 form-data로 받지 않으므로 read_only =True 설정이 필요하다. 
- pk에 대한 정보만 보여준다. 



#### (2) Nested relationships

> 모델 관계상 참조된 대상은 대상의 표현(응답)에 포함되거나 중첩(nested)될 수 있다.
>
> 이러한 중첩된 관계는 serializers를 필드로 사용하여 표현할 수 있다.



`특정 게시글에 작성된 댓글 목록 출력하기 `

```python
from rest_framework import serializers
from .models import Article, Comment

class ArticleSerializer(serializers.ModelSerializer):

    # 나를 참조하고 있는 시리얼라이즈를 호출한다. 단, 호출하는 시리얼라이즈가 먼저 정의되어야 함
    comment_set = CommentSerializer(many=True, read_only=True)

    class Meta:
        model = Article
        fields = '__all__'
```

- CommentSerializer에서 정의된 fields를 다 보여준다.



### 4) 새로운 필드 추가하기

```python
from rest_framework import serializers
from .models import Article, Comment

class ArticleSerializer(serializers.ModelSerializer):
    comment_set = CommentSerializer(many=True, read_only=True)
    comment_count = serializers.IntegerField(source="comment_set.count", read_only=True)

    class Meta:
        model = Article
        fields = '__all__'

```

comment_set 매니저는 모델 관계로 이해 자동으로 구성되기 때문에 커스텀 필드를 구성하지 않아도 comment_set이라는 필드명을 fields 옵션에 작성만 해도 사용할 수 있다. 하지만, 지금처럼 별도의 값을 위한 필드를 사용하려는 경우 자동으로 구성되는 매니저가 아니기 때문에 직접 필드를 작성해야 한다. 



`source`

- 필드를 채우는 데 사용할 속성의 이름이다.
- 점 표기범을 사용하여 속성을 참색할 수 있다.



<br>
