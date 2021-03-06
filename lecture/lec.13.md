저번 시간에 했던 피보나치를 다시 보는 것부터 시작한다.  
저번 시간에 실행했던 코드에 추가하여 코드를 실행됐을 때 무엇이 행해지고 있는 지를 볼 수 있도록 볼 수 있게하기 위해 프린트 구문을 덧붙였다.  

e.g.1) fib
```
>>> def fib(n):
...     global numCalls
...     numCalls +=1
...     print('fib called with', n)
...     if n <= 1:
...             return n
...     else:
...             return fib(n-1) + fib(n-2)
...
>>> numCalls = 0
>>> n =6
>>> res = fib(n)
fib called with 6
fib called with 5
fib called with 4
fib called with 3
fib called with 2
fib called with 1
fib called with 0
fib called with 1
fib called with 2
fib called with 1
fib called with 0
fib called with 3
fib called with 2
fib called with 1
fib called with 0
fib called with 1
fib called with 4
fib called with 3
fib called with 2
fib called with 1
fib called with 0
fib called with 1
fib called with 2
fib called with 1
fib called with 0
>>> print('fib of' , n ,'=', res, 'numCalls = ', numCalls)
fib of 6 = 8 numCalls =  25
```

결과를 보면 6의 피보나치가 8이라고 나왔다.   
잘못된 결과이다.  
무엇이 문제일까?  
"만약 n이 1보다 갖고나 작으면, n을 반환하라."가 틀렸다.
왜냐하면 0의 피보나치가 1이기 때문이다.  
그래서 수정을 하면 아래 코드와 같다.

e.g.2) corrected fib
```
>>> def fib(n):
...     global numCalls
...     numCalls +=1
...     print('fib called with', n)
...     if n == 0 or n == 1:
...             return 1
...     if n <= 1:
...             return n
...     else:
...             return fib(n-1) + fib(n-2)
...
>>> numCalls = 0
>>> n = 6
>>> res = fib(n)
fib called with 6
fib called with 5
fib called with 4
fib called with 3
fib called with 2
fib called with 1
fib called with 0
fib called with 1
fib called with 2
fib called with 1
fib called with 0
fib called with 3
fib called with 2
fib called with 1
fib called with 0
fib called with 1
fib called with 4
fib called with 3
fib called with 2
fib called with 1
fib called with 0
fib called with 1
fib called with 2
fib called with 1
fib called with 0
>>> print('fib of' , n ,'=', res, 'numCalls = ', numCalls)
fib of 6 = 13 numCalls =  25
```

6의 피보나치 결과 값이 13이란 값이 나왔고 25번의 Call이 나왔다. 같은 값을 계속해서 계산하기 때문에 25번과 같이 많이 계산되었다.  
이걸 실행할 때, 이미 답이 나온, 알고 있는 값들을 다시 계산함으로써, 많은 계산량을 갖는다는 것을 알 수 있다.
## 다이나믹 프로그래밍의 최적화 문제
### Overlapping(중첩)

많은 재귀적인 알고리즘에서, 큰 문제를 작은 문제로 나누어 해결하였다.  
중첩은 그와 다르다.  
이진 탐색과 다르게, 중첩은 각각의 인스턴스를 분류하고 어떤 인스턴스는 중첩시킨다. 그들은 같이 무언가를 공유한다.   
이 기술을 알기 위해 다음 기술을 알아야한다.

### memoization
메모란, 남아 있는 횟수만큼, 처음에 계산된 값을 기록하는 것이다.

e.g.1) memo
```
>>> def fib1(n):
...     memo  ={0:0, 1:1}
...     return fastFib(n,memo)
...
>>> def fastFib(n, memo):
...     global numCalls
...     numCalls  +=1
...     if not n in memo:
...             memo[n] = fastFib(n-1, memo) + fastFib(n-2, memo)
...     return memo[n]
...
>>> numCalls = 0
>>> n=6
>>> res = fib1(n)
>>> print('fib of', n, '=',res,'numCalls = ', numCalls)
fib of 6 = 8 numCalls =  11
```
25번의 call이 아닌, 11번의 call이 나옴을 알 수 있다.  
이 개선이 얼마나 큰 개선이었는 지를 보여주겠다.

e.g.2) compare raw and memo
```
>>> def fib1(n):
...     memo  ={0:1, 1:1}
...     return fastFib(n,memo)
...
>>> def fastFib(n, memo):
...     global numCalls
...     numCalls  +=1
...     if not n in memo:
...             memo[n] = fastFib(n-1, memo) + fastFib(n-2, memo)
...     return memo[n]
...
>>> numCalls =0
>>> n =30
>>> res =fib1(n)
>>> print('fib of ',n,'=',res, 'numCalls = ', numCalls)
fib of  30 = 1346269 numCalls =  59

>>> def fib(n):
...     global numCalls
...     numCalls  +=1
...     if n == 0 or n == 1:
...             return 1
...     if n<= 1:
...             return n
...     else:
...             return fib(n-1) + fib(n-2)
...
>>> numCalls = 0
>>> n = 30
>>> res = fib(n)
>>> print('fib of ',n,'=',res, 'numCalls = ', numCalls)
fib of  30 = 1346269 numCalls =  2692537
```
위 예시를 보면 2692537번의 호출에서 59번의 호출로 훨씬 효율적으로 계산량이 줄어든 것을 확인할 수 있다.

이것이 **다이나믹 프로그래밍** 이라고 부르는 기술의 핵심이다.

#### Table lookup(테이블 조회)
메모의 특수한 경우이지만 일반적이다.  
만약 무언가를 계산할때, 답들을 저장해놨다가 이후에 답을 받을 수 있는 것처럼 말이다.  
이것은 피보나치의 가짜 허수아비이다.   


### 최적화된 하부구조
최적화된 하부구조의 개념은 작게 분리된 문제들의 glbally 최적 해법을 locally 최적 해법으로부터 얻는 것이다. 모든 문제에 일치되지는 않지만, 많은 문제에 적용할 수 있다.  

교수님의 메세지
- 만약 최적화된 하부구조와 중첩된 local 해법들을 가지고 있다면, 다이나믹 프로그래밍을 적용해서 풀어갈 수 있다.

##### 0-1 배낭문제로 돌아가서 이 개념을 설명해보자.  
물건들의 무게 합  = A  
물건의 개수 = n

이 문제에 최적화된 하부구조가 있는가?

### Decision tree

<배낭문제>  
weight = [5,3,2]  
value =[9,7,8]  
max 5에서,   
무엇을 가져야할 지 골라야한다.  

e.g.1) decision tree

<img src="/img/decision_tree.png">

이 의사결정나무 해법의 복잡도는 **지수** 이다.

e.g.2) decision tree code
```
>>> def maxVal(w,v,i,aW):
...     global numCalls
...     numCalls +=1
...     if i ==0:
...             if w[i] <= aW:
...                     return v[i]
...     without_i = maxVal(w,v,i-1, aW)
...     if w[i] > aW:
...             return without_i
...     else:
...             with_i = v[i] + maxVal(w,v,i-1, aW-w[i])
...             return max(with_i, without_i)
...
>>> weights = [1,5,3,4]
>>> values = [15.10,9,5]
>>> numCalls = 0
>>> res = maxVal(weights, vals, len(vals)-1, 9)
>>> print('max Val = ', res, 'number of calls = ', numCalls)
max Val =  29.1 number of calls =  3034
>>> w = [1,1,5,5,3,3,4,4]
>>> v =[15,15,10,10,9,9,5,5]
>>> numCalls = 0
>>> res = maxVal(weights, vals, len(vals)-1, 9)
>>> print('max Val =', res, 'number of calls = ', numCalls)
max Val = 29.1 number of calls =  1010
```
