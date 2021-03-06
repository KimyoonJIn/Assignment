# [MIT - Introduction to Computer Science and Programming in Python](https://www.inflearn.com/course/mit-%EA%B3%B5%EA%B0%9C%EA%B0%95%EC%A2%8C-python/)
nlogn 정렬 알고리즘을 어떻게 얻는 건가요?

## 분할 정복 알고리즘
예) 이진 탐색

#### 분할정복
- 문제를 같은 유형의 작은 문제들로 쪼개라
- 각각의 작은 문제들에 대해서 독립적으로 해결하라
- 해결된 걸 합쳐라

##### 분할이란?
큰 탐색을 반을 쪼개고 또 반으로 쪼개는 것.

합치는 것 역시 쉽다.
그 특정 알고리즘을 **병합 정렬 알고리즘** 이라고 한다.

### 병합 정렬
1945년, 폰 노이만이 발명했다.  

e.g.1) 병합 정렬
```
2개의 정렬되어 있는 리스트들을 합치고 싶을 때

first_list = [3,12,17,24]
second_list = [1,2,4,30]

1<3
[1]
3>2
[1,2]
3<4
[1,2,3]
...
[1,2,3,4,12,17,24,30]

result = [1,2,3,4,12,17,24,30]
```
**POINT**
- 몇번이나 연산했는가? - 7번의 비교.   
왜냐하면 단지 각각의 작은 리스트의 첫 번째 원소만을 보고 비교했기 때문이다.

- 복잡도 - 선형 n  
n은 각각의 리스트들에 들어잇는 원소들 수의 합이다.

병합 정렬은 분할 정복의 아이디어를 취한다.
- 리스트를 반으로 나누어라
- 크기가 1개인 리스트를 가질 때까지 계속해서 반으로 나누어라
- 작은 리스트들을 합쳐라

### 분할 정복
e.g.1) 분할 정복
```
>>> def merge(left,right):
...     result =[]
...     i,j =0,0
...     while i<len(left) and j<len(right):
...             if left[i] <= right[j]:
...                     result.append(left[i])
...                     i = i + 1
...             else:
...                     result.append(right[j])
...                     j = j + 1
...     while (i< len(left)):
...             result.append(left[i])
...             i = i+1
...     while (j < len(right)):
...             result.append(right[j])
...             j = j + 1
...     return result
>>> def mergesort(L):
...     print(L)
...     if len(L) < 2:
...             return L[:]
...     else:
...             middle = int(len(L) /2)
...             left = mergesort(L[:middle])
...             right = mergesort(L[middle:])
...             together = merge(left,right)
...             print('merged', together)
...             return together
...
>>> test = [1,4,3,6,5,2,8,7]
>>> mergesort(test)
[1, 4, 3, 6, 5, 2, 8, 7]
[1, 4, 3, 6]
[1, 4]
[1]
[4]
merged [1, 4]
[3, 6]
[3]
[6]
merged [3, 6]
merged [1, 3, 4, 6]
[5, 2, 8, 7]
[5, 2]
[5]
[2]
merged [2, 5]
[8, 7]
[8]
[7]
merged [7, 8]
merged [2, 5, 7, 8]
merged [1, 2, 3, 4, 5, 6, 7, 8]
[1, 2, 3, 4, 5, 6, 7, 8]
```
교수님의 메세지  
- 분할 정복 알고리즘을 디자인할 때, 꼭 기억해 두고싶은 일반적인 것.  
나누는 것의 이점을 정말 쓰고 싶지만, 만약 합치는 단계에서 결국 엄청난 일을 해야 한다면, 결국 어떠한 것도 못 얻을 수도 있다. 그렇기 때문에 균형을 맞추는 것이 중요하다.

### 복잡도 - nlogn
**왜 nlogn인지 아래 그림과 함께 설명한다.**

  <img src="/img/nlogn.png">

크기가 n인 문제에서부터 시작한다. 사진의 트리에서 1개의 원소를 가질 때까지 계속해서 문제를 반으로 나눈다. 마지막 레벨에 도착하게 되면 병합을 시작해야 한다. 각각의 병합 연산들은 n이다.그 위 레벨로 올라가면 크기가 2인 n/2연산들이 있다. 또 그 위 레벨로 올라가면 크기가 4인 n/4연산들이 있다. 그래서 항상 n개의 원소들의 병합을 하게 된다.  

Q. 얼마만큼의 시간이 걸릴까?  
A. 이 트리의 각각의 레벨에서 n번의 연산을 한다.

몇 개의 레벨을 가질 것인지는 분할에 달렸다. : **logn**  
왜냐하면 문제가  n으로 시작해 각각의 단계에서 문제를 반으로 나누면 n/2가 되고, 또 반으로 나누면 n/4가 되고 n/8이 되기 때문이다.  
그래서 각 레벨마다 n번의 연산을 logn번 하게 되는 것이다.  : **n*logn번**  

시간은 좀 걸리지만 좋은 알고리즘이다.

#### 일반화   
문제가 있을 때, 이것을 해결하고자 시도해보고 이리저리 생각해보는 일반적인 도구는 문제를 더 간단하게 나누어 보는 것이다. 더 단순하다고 말해서는 안되고 같은 문제의 작은 버전이라고 말할 수 있다.

스스로 생각해보아야할 것
1. 몇번의 분할을 원하는 가이다.  

2.  기본 케이스가 무엇인가  
  언제까지 문제를 쪼개어 내려갈 것인가  
  그것이 문제를 풀기에 근본적으로 충분히 작은가를 생각해보는 것

3. 어떻게 합치는가


## Hashing



e.g.1) 정수들을 나타내고 싶다.
```
정수들을 나타내고 싶다.  
이 때, 정수는 0부터 9 범위 사이에 있는 것 외에는 어떤 것도 아니라고 약속한다.
모든 원소들을 놓을 하나의 리스트를 만든다.
그리고 나서, 어느곳에서나 정수 1을 넣을 수 있다.

>>> def create(smallest, largest):
...     inset = []
...     for i in range(smallest, largest+1):
...             intSet.append(None)
...     return intSet
...
>>> def insert(intSet, e):
...     intSet[e] = 1
...
>>> def member(intSet, e):
...     return intSet[e] == 1
...
```

#### 복잡도 - 상수
1이라는 상수

해쉬함수라고 불리는 것은 어떤 종류의 데이터도 정수들로 대응시켜주는 방법을 가지고 있기 때문이다.

e.g.2) 문자들의 집합을 가지고 싶다.
```
>>> def hashChar(c):
...     return ord(c)
...
>>> def cSetCreate():
...     cSet =[]
...     for i in range(0,255):
...             cSet.append(None)
...     return cSet
...
>>> def cSetInsert(cSet,e):
...     cSet[hashChar(e)] =1
...
>>> def cSetMember(cSet,e):
...     return cSet[hashChar(e)] == 1

이것의 아이디어는?
문자를 0부터 256사이의 정수에 대응시킨다.
그 수 만큼의 길이를 갖는 리스트를 만들어놓고 단순히 표시하면 된다.
여전히 복잡도는 상수이다.
```


##### 스트링의 집합을 나타내고싶다면?  
그렇다면 기본적으로 해쉬함수를 일반화시킨다.
스트링의 가장 대표적인 알고리즘인 Rabin-karp 알고리즘일 것이다.
- input을 정수들의 집합으로 대응해주는 것과 같다.
- 일정한 접근시간을 가진다.  

Q. 일정한 접근시간을 가짐으로써 무엇을 감수해야하는가?  
A . 메모리 감수
시간을 위해 공간을 더 사용했다.
일정한 접근시간을 가졌지만 공간을 더 사용하게 됨으로써, 너무 많은 메모리를 사용한다.

Q. 저장공간안에서 해쉬함수로 내가 가지고 있는 입력값을 어떻게 정확하게 하나의 값에 대응 시키는가입니다.  
A. 할 수 없다.
간단한 정수들의 경우에는 가능하나, 복잡한 경우에는 정확하게 하나의 경우를 입력 값으로 대응시키는 것은 어렵다.

##### Hash design
일반적으로 하는 해쉬 디자인은 여러분이 문제를 다룰 수 있는 코드를 디자인 하는 것이다.  
고르게 분포하게 하는 해쉬함수를 사용해야한다. 하지만 그 리스트를 저장하는 곳에 작은 목록이 있을 지도 모른다. 어떤 것을 찾으려 할 때, 그 리스트에 있는 것들을 통해서 선형검색을 해야한다, 이 때, 좋은 소식은 해쉬테이블에 있는 원소들을 3,4,5와 같은 작은 숫자가 될 것이다. 그러면 검색은 매우 쉽지만 그 교환은 해야한다는 나쁜 소식도 있다.

##### 어렵다
마지막으로 해쉬에 대해서 말하고 싶은것은 만들기가 어렵다는 것이다. 겹치지 않고 고르게 잘 대응된 좋은 해쉬함수를 원하기 때문에 많은 노력이 필요하다.

## Exception
- nameerror  
처리할 수 없는 어떤 것의 이름을 적었다.  
특별한 종류의 예외이다.
- indexerror  
지정된 인덱스보다 오버된 인덱스에 지정할 때 일어나는 에러이다.

에러를 고치기 위해 탑 레벨로 올라는 것은 짜증나는 일일 수 있다.
많은 예외의 경우에는 사실 프로그램 디자이너로서 처리할 수 있다.

#### handled exceptions
e.g.1) try와 except
```
>>> def readFloat(requestMsg, errorMsg):
...     while True:
...             val = input(requestMsg)
...             try:
...                     val = float(val)
...                     return val
...             except:
...                     print(errorMsg)
...
>>> print(readFloat("Enter float: ", "Not a float."))
Enter float: 3
3.0
>>> print(readFloat("Enter float: ", "Not a float."))
Enter float: 0
0.0
>>> print(readFloat("Enter float: ", "Not a float."))
Enter float: s
Not a float.
Enter float: a
Not a float.
Enter float: 2
2.0
```

#### exception 구체화

e.g.1)
```
파일을 입력하는 코드를 쓰고 있다.
파일 이름을 말해주었던 것을 확실히 기억한다.
파일 이름을 가지고 무언가를 하고 싶다.
그 이름으로 파일이 나타난다는 것을 장담할 수는 없지만
그것이 일어날 수 있다는 것은 알고 있다.
```
이럴 때 좋은 방법은 **exception** 으로 쓰는 것이다.

exception을 쓴다면, 저 상황을 다음과 같이 대처할 수 있다.
```
여기 파일을 찾을 때 하고 싶은 것이 있다.
그러나 파일이름이 없는 경우에는 그것을 처리하기 위한 무언가가있다.
```
e.g.2) 예외 일반화
```
>>> def readVal(valType, requestMsg, errorMsg):
...     while True:
...             val = input(requestMsg)
...             try:
...                     val = valType(val)
...                     return val
...             except:
...                     print(errorMsg)
...
>>> readVal(int, 'Enter int: ', 'Not an int.')
Enter int: 1
1
>>> readVal(int, 'Enter int: ', 'Not an int.')
Enter int: 10
10
>>> readVal(int, 'Enter int: ', 'Not an int.')
Enter int: 10.5
Not an int.
Enter int: d
Not an int.
Enter int: 46
46
```
e.g.3) 실행 안되는 강의 코드  
원래 코드는 아래와 같다. 하지만 GetGradesError를 exception 클래스로 따로 생성해주지 않았기에 GetGradesError를 정의하지 않았다는 에러와 함께 우리가 기대하는 결과를 가져오지 못한다.
```
>>> def getGrades(fname):
...     try:
...             gradesFile = open(fname, 'r')
...     except IOError:
...             print('Could not open', fname)
...             raise GetGradesError
...     grades = []
...     for line in gradesFile:
...             grades.append(float(line))
...     return grades
...
>>> try:
...     grades = getGrades('qlgrades.txt')
...     grades.sort()
...     median = grades(len(grades)/2)
...     print('Median grade is', median)
... except GetGradesError:
...     print('Whoops!')
...
Could not open qlgrades.txt
Traceback (most recent call last):
  File "<stdin>", line 3, in getGrades
FileNotFoundError: [Errno 2] No such file or directory: 'qlgrades.txt'

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "<stdin>", line 2, in <module>
  File "<stdin>", line 6, in getGrades
NameError: name 'GetGradesError' is not defined

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "<stdin>", line 6, in <module>
NameError: name 'GetGradesError' is not defined
```
e.g.4) 내가 수정한 강의 코드 1   
GetGradesError라는 클래스를 생성해주는 방법이다.
```
>>> class GetGradesError(Exception):
...     def __str__(self):
...             pass
...
>>> def getGrades(fname):
...     try:
...             gradesFile = open(fname, 'r')
...     except IOError:
...             print('Could not open', fname)
...             raise GetGradesError
...     grades = []
...     for line in gradesFile:
...             grades.append(float(line))
...     return grades
...
>>>
>>> try:
...     grades = getGrades('qlgrades.txt')
...     grades.sort()
...     median = grades(len(grades)/2)
...     print('Median grade is', median)
... except GetGradesError:
...     print('Whoops')
...
Could not open qlgrades.txt
Whoops
```
아니면 이러한 방법도 있다.  

e.g.5) 내가 수정한 강의 코드 2   
클래스에 아예 whoops를 넣어주고 except에서 에러를 찍어주면 바로 whoops가 나온다.
```
>>> class GetGradesError(Exception):
...     def __str__(self):
...             return ("whoops")
>>> try:
...     grades = getGrades('qlgrades.txt')
...     grades.sort()
...     median = grades(len(grades)/2)
...     print('Median grade is', median)
... except GetGradesError as e:
...     print(e)
...
Could not open qlgrades.txt
whoops
```

## assert과 exception의 차이점

### assert
- 목적  
함수가 이런 종류의 결과를 주는 지 확인할 수 있었다.
특정한 제약들을 주는 인풋을 주면

- assert는 여기에 테스트를 할 조건들이 있다는 것을 말한다.  
만약 그것이 True이면, 코드의 나머지 부분을 실행한다.  
사실이 아니라면, 에러를 띄운다.

- 기본적으로 몇몇의 pre-condition들을 가지고 있다는 것을 뜻한다.  
pre-condition들은 True이어야하는 asset안의 조항들을 말한다.그리고 post-condition이 있다.  
본질적으로 asset가 말하는 것 다음과 같다.  
만약 pre-condition들을 만족시키는 입력값을 준다면, 코드가 pre-condtion을 만족함을 보장한다는 것을 뜻한다.
결과적으로 만약 pre-condtion들이 사실이 아니라면, 에러가 나오고, 탑 레벨로 거슬러 올라가서 즉시 작동을 멈출 것이다.  

- 디버깅 시간과 테스팅 시간에서 조건을 확인할 수 있게 해준다는 점에서 좋다.  
그래서 사실 코드가 어떻게 돌아가고 있는 것인지 볼 때 그것들을 사용할 수 있다.

- **요약**   
사용자에게 말하기 위해서 여러분이 넣은 것.  
예로 들어, "이런 타입의 입력 값을 넣었는지 확인하세요."와 같은 것이 있다.  
그러나 그 나머지 코드들은 올바르게 잘 작동할 것이다.  

### exception
- 예외처리는 일반적인 것들 외에도 그것 자체가 조건들을 처리하기 위해 노력할 것이다.  
함수를 가지고 여러분이 원하는 것을 할 수 있다. 어떤 것이 잘못되어 가고 있는지 여러분에게 말해줄 수 있다. 대부분의 경우에 스스로 그것을 처리할 수 있다. 그래서 가능한 한 많은 exception들은 기대되지 않은 것들을 처리하려고 할 것이다.  

- 만약 처리할 수 없다면, 다른 누군가에게 그것을 처리하라고 할 것이고, 기대되지 않은 조건들에 대해서는 처리하는 사람이 없을 떄에만 탑 레벨로 올라갈 것이다.

- **요약**   
이 부분에 이상한 경우(에러)가 발생했고, 그것들을 처리하기 위해 무엇인가 하고 싶은 것이 있다는 것을 말한다.


Q. 마지막으로 왜 우리는 exception을 가지고 싶어할까?  
A. 단순한 부동소수점을 입력했던 때로 돌아가보자.  
  만약 숫자들이 주로 안에 있기를 기대했었다면, try 해보고 강제로 변환했을 수도 있다.  
  그러나 문제는 사실 내가 기대하는 형태를 가졌는지 가지지 못했는지 알고 싶다는 것이다.  
  따라서, exception은 사용가능한 지에 대해 말할 때 매우 유용하다.  

교수님의 메세지
- 결과적으로  exception을 사용하는 것은 좋은 것이다.
