## 스레드 

스레드는 프로세스를 구성하는 실행 단위.

멀티스레드는 하나의 프로세스가 한 번에 여러 일을 동시에 처리할 수 있게 가능하게 해주는 역할.

앞 전에서 살펴보았던 프로세스에서 프로세스를 fork하여 같은 작업을 하는 동일한 프로세스 두 개를 동시에 실행하면

코드 영역, 데이터 영역, 힙 영역 등을 비롯하여 모든 자원이 복제되어 메모리에 적재됨.

즉, * fork를 세 번, 네 번하면 같은 프로세스가 통째로 세 개, 네 개 적재되기때문에 메모리 낭비가 생김.

메모리 낭비가 실행되는 이 분을 멀티스레드가 해결해줌.

### 멀티스레드

멀티쓰레드는 말 의미 그대로 여러스레드로 프로세스를 동시에 실행하는 것을 말함.

![image](https://github.com/wltnthss/learning-cs/assets/60785586/a61cf696-d577-49e2-84cb-c2b48737df39)

위의 그림은 멀티쓰레드의 장점을 가장 잘보여주는 그림.

스레드는 레지스터, 메모리, 힙, 코드 , 데이터 등 프로세스의 자원을 공유하기 때문에 협력과 통신에 유리하며 여러 프로세스를 실행하는 것보다 메모리 낭비를 막아줌.

하지만 프로세스의 자원을 공유한다는 특성은 하나의 스레드에 문제가 생겼을 때 다른 스레드도 영향을 받아 단점이 될 수도 있다는 문제점이 있음.

### 멀티스레드 문제점

- 서로 다른 스레드가 데이터, 힙 영역을 공유하기 때문에 다른 쓰레드에서 사용중인 변수나 자료 구조에 접근하여 엉뚱한 값을 읽거나 수정 할 수 있음.

- 멀티스레드 환경에서는 동기화 작업을 통해 작업 처리 순서를 컨트롤하고, 공유 자원에 대한 접근을 컨트롤 해야하면서, lock 현상을 통한 병목 현상을 줄여야함.

- 동기화를 사용할 때는 동기화 처리가 필요한 부분에만 synchronized 키워드를 통해 동기화해야함.

- 불필요한 부분까지 동기화 할 경우 lock을 획득한 스레드가 종료하기 전까지 대기해야해서 전체 성능에 영향을 미치게 됨.

## Python으로 스레드 다루기

### 코드로 스레드 만들기

```python
import os

print('this process pid is -> ', os.getpid())
```

위 소스 코드를 실행하면 프로세스가 생성됨.

````python
import threading
import os 

def f1():
    print('thread pid is -> ', threading.get_native_id())
    print('f1 process pid is -> ', os.getpid())

if __name__ == '__main__':
    print('main process pid is -> ', os.getpid())
    thread = threading.Thread(target=f1).start()
````

> 실행 결과

```
main process pid is ->  18020
thread pid is ->  14852
f1 process pid is ->  18020
```

프로세스 내에서 ID가 14852인 스레드를 만들어냄

### 동일한 작업을 하는 스레드 생성하기

```py
import threading
import os

def f1():
    print('thread pid is -> ', threading.get_native_id())
    print('f1 process pid is -> ', os.getpid())

if __name__ == '__main__':
    print('main process pid is -> ', os.getpid())
    thread1 = threading.Thread(target=f1).start()
    thread2 = threading.Thread(target=f1).start()
    thread3 = threading.Thread(target=f1).start()
```

> 실행 결과

```
main process pid is ->  19632
thread pid is ->  14688
f1 process pid is ->  19632
thread pid is ->  31072
f1 process pid is ->  19632
thread pid is ->  8144
f1 process pid is ->  19632
```

f1이라는 함수를 실행하는 세 개의 스레드(thread1, thread2, thread3)를 생성

세 개의 스레드 모두 각기 다른 스레드지만, 이들은 모두 동일한 프로세스를 공유함.

### 각기 다른 작업을 하는 스레드 생성하기

```python
import threading
import os

def f1():
    print('print thread f1')

def f2():
    print('print thread f2')

def f3():
    print('print thread f3')        

if __name__ == '__main__':
    thread1 = threading.Thread(target=f1).start()
    thread2 = threading.Thread(target=f2).start()
    thread3 = threading.Thread(target=f3).start()
```

> 실행 결과

```
print thread f1
print thread f2
print thread f3
```

이처럼 하나의 프로세스 내에 여러 개의 실행 흐름을 만들어낼 수 있음.
 
