# fuction_args_kwargs

## 팩킹과 언패킹

> 모든 시퀀스형 데이터(문자열, 튜플, 리스트, range()등 )은 패킹/ 언패킹을 할 수 있다. 

- packing

  여러 개의 값을 하나의 컨테이너(자료구조)로 묶는 것. (여러 개의 인자 => 하나의 **리스트형태의 객체**) 

  하나로 압축시키는 개념. 

- unpacking
  여러 개의 값을 하나로 묶은 컨테이너를 푸는 것. (하나의 객체 => 여러 개의 **튜플 형태의 인자**)
  여러개로 풀어버리는 개념. 

<br>

```python
#packing을 통해 변수에 여러 개의 데이터를 한번에 저장할 수 있다. 
x, *y, z = 0, 1, 2, 3, 4, 5, 6, 7, 8, 9
 
print(x, type(x)) # 0 <class 'int'>
print(y, type(y)) # [1, 2, 3, 4, 5, 6, 7, 8] <class 'list'>
print(z, type(z)) # 9 <class 'int'>

```

```python
# unpacking을 통해 하나의 변수에 저장되어 있는 여러 인자를 풀어서 참조할 수 있다. 
x = [1, 2, 3, 4, 5]
y = 0, *x

print(y, type(y)) # (0, 1, 2, 3, 4, 5) <class 'tuple'>
```

<br>

## 가변 인자 

> 인자 (argument)란, 함수의 입력값으로 함수를 호출할 때 매개변수(parameter)로 전달되는 값이다.  
>
> 가변 인자는 함수의 입력값으로 임의의 개수의 인자를 매개변수로 전달하는 것을 의미한다.  (즉, 인자의 개수가 정해지지 않음)

<br>

### 위치 가변 인자 `*args`

- 임의의 개수의 위치 인자를 매개변수로 전달한다. 
- **`tuple`** 형태로 처리가 되며 매개변수 앞에 `*`로 표시를 한다. 
- 가변형 매개변수는 하나만 지정할 수 있으며, 가장 마지막 매개변수로 지정해야 부작용 없이 사용할 수 있다. 

```python
def my_print(*args):
    print(args, type(args), len(args))

a = 1, 2, 3

my_print(1, 2, 3) # (1, 2, 3) <class 'tuple'> 3
my_print(*a) # (1, 2, 3) <class 'tuple'> 3


# 위와 달리 튜플형태의 단일 인자를 매개변수로 전달한 것. 
my_print(a) # ((1, 2, 3),) <class 'tuple'> 1
 
```

 ```python
 # 활용
 def my_sum(*args):
     result = 0 
     for arg in args:
         result += arg
     return result
 
 print(my_sum(1, 2, 3, 4, 5)) # 15
 ```

<br>

### 키워드 가변 인자 `**kwargs`	

- 임의의 개수의 키워드 인자를 매개변수로 전달한다.

- **`dictionnary`** 형태로 처리가 되며 매개면수 앞에 `**`로 표시를 한다. 

- 딕셔너리의 key는 문자열 형태여야 한다. 

```python
def my_print(**kwargs):
    print(kwargs, type(kwargs), len(kwargs))
    
a = {'one': 1, 'two': 2, 'three': 3 }

my_print(one = 1, two = 2, three = 3) # {'one': 1, 'two': 2, 'three': 3} <class 'dict'> 3
my_print(**a) # {'one': 1, 'two': 2, 'three': 3} <class 'dict'> 3

my_print(a) # TypeError: my_print() takes 0 positional arguments but 1 was given
```

```python
#활용
def info(**kwargs):
    for key, value in kwargs.items():
        print('{0} : {1}'.format(key, value) )

x = {'name': '홍길동', 'age': 20 } 

info(**x)  # x = {'name': '홍길동', 'age': 20 } 
```

<br>

<br>