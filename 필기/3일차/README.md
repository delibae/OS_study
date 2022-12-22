# OS 3일차 - IPC

IPC(Inter Process Communication) 종류
- Signal : 프로세스간 매우 간단한 신호 전달
- Pipe : 단방향 데이터 전송
- Message Queue : 쌍방향 데이터 전송 가능 공용 큐 사용
- Shared Memory : 여러 프로세스가 같이 사용할 별도 메모리 공간 프로세스 저장소
- Semaphore(SysV) : 커널에 lock을 만들어 프로세스간 동기화 사용
- Unix Domain Socket : 파일 형식의 소켓을 만들어 로컬 머신내 프로세스 통신 간 사용
- Inet Socket : 네트워크 통신을 위한 소켓  
<br/>

IPC 식별자
- Key 
    * IPC 자원에 접근하기 위한 식별번호로 사용
    * 프로세스간 동일한 키값이 필요한 경우 파일을 이용하여 생성(ftok()) 
- Id
    * key를 이용해 만든 IPC 자원의 고유 id  
<br/>

/etc 시스템 각종 설정 파일 존재  
<br/>

ipcs (명령어): IPC, 공유메모리, 세마포어를 현황을 파악  
<br/>

IPC 자원의 생성 및 제거  
- IPC 자원은 커널에 만들어 짐
- 일단 만들어 진 IPC 자원은 따로 제거하기 전에는 시스템에 남아 있음 -> 관리 필요
- system call API or shell prompt  
<br/>

**Pipe**  
- 가장 오래된 Unix IPC
- 단방향 통신 매커니즘
- 같은 조상을 가진 프로세스간에만 pipe를 사용해 통신 가능
- 파이프의 한쪽으로 데이터를 쓰면 반대쪽으로 쓰여진 데이터가 읽혀짐  
<br/>

Pipe의 종류
- unnamed pipe
    * Fiel descriptor 를 사용하기는 하나 가상파일을 사용하기 때문에 이름이 없음.
- named pipe(FIFO 특수파일)
    * **pipe를 위한 별도의 전용 파일을 생성** / 프로세스는 이 파일을 일반 파일 처럼 조작
    * 두 프로세스가 하나의 파일 공유 / 한 프로세스는 기록 / 다른 프로세스는 읽음  
<br/>

Pipe 생성
- 쉘 프롬포트에서 파이프 사용

<pre><code>write(file descrpitor, buf, strlen(buf)+1)</code></pre>
<pre>standardin = 0 / standardout = 1</pre>

- pipe() 함수 사용
<pre><code>int pipe(int filedes[2]);</code></pre>  
<br/>

FIFO(Named pipe)
- Unnamed pipe는 같은 조상 둔 프로세스간 통신 가능 / fd 상속 때문
- 상속관계에 있지 않은 프로세스간에 통신 위해 FIFO 사용
- FIFO의 이름을 아는 모든 프로세스 간에 통신 가능
<pre><code>mkfifo, mknod (shell)</code></pre>
<pre><code>mkfifo()</code></pre>

<pre><code>unlink() -> 파일 지우기</code></pre>

- owner-group-others
<pre><code>drwxr-xr-x</code></pre>

  * owner: rwx, group: r-x, others: -x  
<br/>

**Shared Memory**  
<br/>

SHM 소개

- 메모리 복사 오버헤드가 없어 다른 IPC 메커니즘에 비해 속도가 빠름  

- 동시에 여러 프로세스가 메모리에 접근해서 데이터 훼손이 있을 수 있기 때문에 동기화 기법이 필요  


<br/>



ftok: 유일한 키값을 만들어 주는 함수  
<pre><code> shmKey = ftok("./shmview.c",'R'); ftok(const char *pathname,int proj_id);</code></pre>  
<br/>

shmget : 공유 메모리 생성 함수
<pre><code>shmId = shmget(shmKey, SHM_SIZE, 0644 | IPC_CREAT);</code></pre>  
<br/>

shmat : 공유 메모리 연결
<pre><code>char *ptr1 = shmat(shmId, 0, 0) / shmat(int shmid, void *shmaddr, int shmflg)</code></pre>  
<br/>  

공유자원 -> 동기화 필요
* 뮤택스(상호배제) / 세마포어 로 구현  
<br/>  


**Message Queue**  

- 동작 멈춤 blocking