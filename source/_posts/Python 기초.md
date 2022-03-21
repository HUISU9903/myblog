# Python 기초

# 파이썬

파이썬은 1991년에 발표된 생각보다 오래된 언어이다. 

# 주석 처리

- 코드 작업 시, 특정 코드에 대해 설명
- 사용자 정의 함수 작성 시, 클래스 작성 시, 도움말 작성 시

```python
# 한줄 주석처리

'''
여러줄 주석처리
'''
print("hello , world!")
```

# 변수(Scalar)

- 객체(Object)로 구현이 된다.
    - 하나의 자료형을 가진다.
    - 클래스로 정의가 된다.
    - 다양한 함수들이 존재 한다.

## int

- int 정수를 표현 하는데 사용한다.

```python
num_int = 1
print(num_int)

print(type(num_int))
```

## float

- 실수를 표현하는데 사용한다.

```python
num_float = 0.2
print(num_float)
print(type(num_float))
```

## bool

- True와 False로 나타내는 Boolean 값을 표현 하는데 사용한다.

```python
bool_true = True
print(bool_true)
print(type(bool_true))
```

## None

- Null을 나타내는 자료형으로 None라는 한 가지 값만 가진다.

```python
none_x = None
print(none_x)
print(type(none_x))
```

## 사칙연산

* 정수형 사칙연산

```python
a = 4
b = 4
print('a + b ', a  + b )
print('a - b ', a  - b )
print('a * b ', a  * b )
print('a / b ', a  / b )
print('a // b ',  a  // b )
print('a % b ', a  %  b )
print('a ** b ', a  ** b )
```

* 실수형 사칙연산

```python
a = 1.3
b = 2.5
print('a + b ', a  + b )
print('a - b ', a  - b )
print('a * b ', a  * b )
print('a / b ', a  / b )
print('a // b ',  a  // b )
print('a % b ', a  %  b )
print('a ** b ', a  ** b )
```

## 논리형 연산자

- Bool형은 True와 False 값으로 정의
- AND / OR

```python
x = True
y = False

print(x and x)
print(x and y)
print(y and x)
print(y and y)

print(x or x) 
print(x or y) 
print(y or x)
print(y or y)
```

## 비교 연산자

- 부등호를 의미한다
- 비교 연산자를 통해 True와 False 값을 도출

## 논리형 연산자와 비교 연산자의 응용

```python
var = input("입력")
print(type(var))
```

숫자를 입력해도 문자열인 ‘str’로 표시되는 오류가 발생한다.

- 형 변환을 해준다.
- 문자열,정수,실수 등

```python
var = int(input("숫자 입력"))
print(type(var))
```

앞에 class 이름을 붙이면 형 변환을 할 수 있다.

```python
num1= int(input("숫자 입력"))
num2 = int(input("숫자 입력"))
num3 = int(input("숫자 입력"))
num4 = int(input("숫자 입력"))

var1 = num1 >= num2 #True
var2 = num3 < num4 #True

print(var1 and var2)
print(var1 or var2) 
```

이런 식으로 응용도 가능하다.

# 변수(Non Scalar)

## Sting 연산자

덧셈 연산자를 써보자.

```python
str1 = "hello "
str2 = "World! "
num1 = 1

print(str1 + str2)
```

곱셈 연산자를 써보자.

```python
greeting = str1 + str2
print(greeting * 3 )
```

## Indexing

문자열 인덱싱은 각각의 문자열 안에서 범위를 지정하여 특정 문자를 추출한다.

```python
greeting = "Hello Kaggle!"
print(greeting[6])
```

## Slicing

범위를 지정하고 데이터를 가져온다.

```python
greeting

print(greeting[:]) # Hello Kaggle!
print(greeting[6:]) # Kaggle!
print(greeting[0:6]) # Hello 
print(greeting[3:9]) # lo Kag
print(greeting[0:9:2]) # HloKg
```

## 여러 줄인 문자열을 변수에 대입

```python
multiline= "Life is too short\n You need python" # \n 줄바꿈 코드 삽입

multiline=
"""
 Life is too short
 You need python
 """
#  큰 따옴표 3개 또는 작은 따옴표 3개로 묶기
```

| 코드  | 설명 |
| --- | --- |
| \n | 문자열 안에서 줄을 바꿀 때 사용 |
| \t | 문자열 사이에 탭 간격을 줄 때 사용 |
| \\ | \ 을 그대로 표현할 때 사용 |
| \’ | 작은따옴표를 그대로 표현할 때 사용 |
| \” | 큰따옴표를 그대로 표현할 때 사용 |
| \r | 캐리지 리턴(줄 바꿈 문자, 현재 커서를 가장 앞으로 이동 |
| \f | 폼 피드 ( 줄 바꿈 문자, 현재 커서를 다음 줄로 이동) |
| \a | 벨소리 ( 출력할 때 PC 스피커에서 삑하는 소리가 난다) |
| \b | 백 스페이스 |
| \000 | 널 문자 |

## 문자열의 연산

### 문자열 연결하기

```python
a = "Python"
b = " Good!" 
print(a + b)
'Python Good'
```

‘+’  연산자를 이용하면 문자열을 합쳐서 출력할 수 있다.

### 문자열 곱하기

```python
a = "Python"
a * 2
'PythonPython'
```

문자열이 2번 출력되는 것을 확인 할 수 있다.

### 문자열의 길이 구하기

```python
a = "Life is strange"
len(a)
```

실행 시 15라고 나오는데, 공백을 포함한 수이다.

### 문자열 인덱싱

```python
a = "Life is strange"
a[3]
'e'
```

실행 시 4번째 문자인 e를 출력한다. 

파이썬은 0부터 숫자를 세기 때문에 0이 ‘L’이고 1이 ‘i’ 2가 ‘f’ 3이 ‘e’가 된다.

### 문자열 슬라이싱

```python
a = "Life is strange"
a[0:3]
'Life'
```

문자열 인덱싱은 한 문자만을 추출하지만, 슬라이싱은 **원하는 단어나 문장 단위**로 추출할 수 있다.

### 문자열 포맷팅

```python
"I have %d apples" % 3
'I have 3 apples'
```

문자열 안에 임의의 원하는 정수를 삽입하는 방법이다 .

```python
"I have %s apples" % "three"
'I have three apples'
```

 문자열 안에 임의의 원하는 문자열을 삽입하는 방법이다.

```python
number = 3
day = "monday"
"I have %d apples, and today is %s" % (number,day)
'I have 3 apples, and today is monday'
```

변수를 이용해서 삽입할 수도 있다, 또한 2개 이상의 값을 넣을 수도 있다.

| 코드  | 설명 |
| --- | --- |
| %s | 문자열(String) |
| %c | 문자 1개(character) |
| %d | 정수(Integer) |
| %f | 부동소수(floating-point) |
| %o | 8진수 |
| %x | 16진수 |
| %% | Literal % (문자 % 자체) |

### .format

```python
"I have {0} apples" .format("five")
'I have five apples'
```

```python
"I have {0} apples" .format(3)
'I have  apples'
```

```python
number = 3
day = "monday"
"I have {0} apples, and today is {1}" .format (number,day)
'I have 3 apples, and today is monday'
```

```python
"I have {0} apples, and today is {1}" .format (number = 3,day = "monday")
'I have 3 apples, and today is monday'
```

%d,%s 안쓰고 .format 쓰는 방법도 있다.

## 문자열 관련 함수들

### count

```python 
a = "apple"
a.count('p')
2
```
문자 개수를 세는 함수이다 .문자열 중 특정 문자의 갯수를 반환한다.

### find
```python 
a = "apple is good"
a.find('d')
14
```
문자열 중 특정 문자가 처음으로 나온 위치를 반환한다. 만약 없다면 -1 을 반환한다.

### index

```python 
a = "apple is good"
a.index('d')
14
```
문자열 중 특정 문자가 처음으로 나온 위치를 반환한다. 만약 없다면 오류가 발생한다.

### join

```python 
",".join('kokomong')
'k,o,k,o,m,o,n,g'
```
문자열의 각각 문자 사이에 ',' 를 삽입한다.


### replace

``` a = "Life is strange"
 a.replace("strange", "Good")
'YLife is Good'
```

문자열 안의 특정한 값을 다른 값으로 치환한다.

### split

```python 
a = "Life is strange"
a.split()
['Life', 'is', 'strange']
```

괄호 안에 아무것도 없을 때에는 공백을 기준으로 문자열을 나눈다, 괄호안에 특정 값을 넣으면 특정 값을 기준으로 나눈다.


