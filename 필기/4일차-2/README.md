# OS 4일차 - Synchronizaiton

동기화
- 멀티 프로세서 등등등... 공유자원(전역변수 ...)에 접근하려고 할 때 순서보장과 동시성을 배재하기 위한 목적으로 설계된 개념
- 동기화는 2개 이상의 프로세스/스레드가 충돌 없이 정상적으로 실행하기 위한 규칙 -> **위반시 중대한 오류 발생할 수 있음**
- 멀티 프로세스/스레드는 실행되는 순서와 속도가 매번 다를 수 있기 때문에 동기화 부분을 제대로 처리하지 않으면 **어떤 경우는 정상 실행되고 또 다른 경우는 에러가 발생하는식**의 간혈적 현상이 나타날 가능성이 큼  
<br/>

동기화의 필요성
- 공유자원에 동시 접근을 배제(Mutual Exclusion)(mutex)
- 프로그램의 일관된 순서 보장  
<br/>

동기화의 종류
- Signal
- Mutex
- Semaphore
- Conditional Variable
- RW Lock  
<br/>

Signal
- 실행순서 보장만 가능하고 공유 자원 동시접근 배제엔는 사용할 수 없음  

Mutex
- 스레드간에 임게영역(critical section)(race condition)을 정의하여 임계영역에 대한 상오 배제(mutual exclusion)를 제공
- Mutex 공용변수를 사용
- **Mutex lock에는 소유권이 있음**  
<br/>

Semaphore
- Binary semaphore 와 counting semaphore 가 있음
- 내부적으로 카운터를 가지고 있고 이 값을 + 또는 - 하면서 자원 접근 여부 결정
- 소유자 개념 x  
<br/>


## Mutex

소개
- 처음 만들어 지면 자물쇠는 열려 있음  
<br/>

특징
- Atomicity: 잠금/해제는 최소단위의 동작. 즉 하나의 스레드가 lock/unlock 동작을 시작하면 다른 스레드 x
- Non-Busy Waiting : Mutex 가 이미 lock 이 되어 있는데 다른 스레드가 lock 을 시도하면 그 스레드는 mutex 가 unlock 되어 자신이 lock을 할 수 있을 때 까지 실행을 멈추고 어떠한 cpu 자원도 소비하지 않음  
<br/>

동작
- **여러 스레드가 실행 중단되어 있는 경우 깨어나는 순서**
    * 리눅스 커널 2.6.22부터 가장 우선순위가 높은 리얼타임 스레드가 깨어남. 단 리얼타임 스레드가 없으면 이전과 마찬가지로 가장 **오래 기다린 스레드가 깨어남**  
<br/>

공유자원 보호
- 공유자원을 액세스 하는 경우 atomicity를 보호하기 위해 mutex 사용

생성
<pre><code>int pthread_mutex_init(pthread_mutex_t *mutex, const pthread_mutexattr_t *attr);</code></pre>

<pre><code>pthread_mutex_t [mutex variable name] = PTHREAD_MUTEX_INITIALIZER;</code></pre>

제거
<pre><code>int pthread_mutex_destroy(pthread_mutex_t *mutex);</code></pre>

Recursive Mutex
- 스레드가 자신이 이미 lock을 한 mute를 lock하려고 하면 deadlock이 발생
- Recursive MUTEX를 만들 때는 static initializer를 사용하는 방법과 pthread_mutexattr_t 구조체에 RECURSIVE 속성을 설정하여 MUTEX를 초기화 하는 방법이 있음


share 파일 retry 뭔지 알아오기
