## Python으로 프로세스 다루기

### 프로세스 개념

프로세스 메모리 영역은 크게 코드 영역, 데이터 영역, 힙 영역, 스택 영역으로 나뉘어 저장됨.

- 코드 영역은 읽기 전용인 텍스트 영역, 

- 데이터 영역은 프로그램이 실행되는 동안 유지할 데이터가 저장되는 공간 ex) 전역 변수

- 힙 영역은 프로그래머가 직접 할당할 수 있는 저장 공간

- 스택 영역은 데이터를 일시적으로 저장하는 공간 ex) 매개 변수, 지역 변수

일시적으로 저장할 데이터는 스택 영역에 PUSH 되고, 필요하지 않은 데이터는 POP으로 스택 영역에서 사라짐.

힙 영역과 스택 영역은 동적 할당 영역이라고도 부름.

### 프로세스 생성 기법

부모 프로세스는 fork를 통해 자신의 복사본은 자식 프로세스로 생성해내고, 만들어진 복사본은 exec을 통해 메모리 공간을 다른 프로그램으로 교체함.

이 때 (복사된 자식 프로세르인 경우에도 PID 값이나 저장된 메모리 위치는 다름)

즉, 부모가 자식 프로세스를 실행하면 프로세스 계층 구조를 이루는 과정은 fork와 exec을 반복하는 과정이라 볼 수 있음.

### 프로세스 만들기

```python
import os
 
print('hello os')
print('my pid is -> ', os.getpid())
```

실행 결과

```
hello, os
my pid is -> 17924
```

파이썬에서는 `os.getpid()`로 프로세스의 PID를 확인할 수 있음
PID값은 운영체제가 그때그때 부여하는 값이기 때문에 실제로 실행했을 때는 위와 다른 PID값이 출력되기도 함.
(PID는 특정 프로세스를 식별하기 위해 부여하는 고유한 번호, `os.getppid()` 는 부모프로세스의 PID를 확인가능함)

자식 프로세스 생성

```python
from multiprocessing import Process
import os

def foo():
    print('child process -> ', os.getpid())
    print('parent process is -> ', os.getppid())

if __name__ == '__main__':
    print('parent process -> ', os.getpid())
    child = Process(target=foo).start()
```

> 실행 결과

```
parent process ->  12428
child process ->  1584
parent process is ->  12428
```

### 동일한 작업을 하는 프로세스 만들기

자식 프로세스는 얼마든지 여러 개 만들 수 있음.

```python
from multiprocessing import Process
import os

def fork():
    print('hello child os')

if __name__ == '__main__':
    child1 = Process(target=fork).start()
    child2 = Process(target=fork).start()
    child3 = Process(target=fork).start()
```

> 실행 결과

```
hello child os
hello child os
hello child os
```

### 각기 다른 작업을 하는 프로세스 생성하기

```python
from multiprocessing import Process
import os

def f1():
    print('child1')

def f2():
    print('child2')

def f3():
    print('child3')

if __name__ == '__main__':
    child1 = Process(target=f1).start()
    child1 = Process(target=f2).start()
    child1 = Process(target=f3).start()
```

> 실행 결과

```
child1
child2
child3
```
