# 운영체제 1일차-2

Process(Process in Execution)  
<br/>

PCB(Process Control Block)  
1. 프로세스 관리를 위한 모든 정적/동적 정보를 가지고 있는 구조체  
   
2. OS는 PCB를 통해 프로세스 관리  
<br/>  

PCB에 들어있는 정보  
1. Process ID: **pid(process id), ppid(parent process id)**  

2. Process State: NEW, READY, RUNNING, WAITING, HALTED, ZOMBIE, SLEEP ...  

3. Program Counter: Next Instruction Address  

4. CPU registers: r0~r31, spm lr, ....

5. CPU scheduling info: priority, time_slice, policy(**비선점**) ... 

6. I/O state: **File descriptor(파일 기술자)** ...
* **VFS(Virtual File System)(커널부 상단): 거쳐서 올라가면 응용부분에서는 파일로 보임**  
<br/>

PCB 확인 커맨드
<pre><code>cat /proce/{num}/status</code></pre>


cd

ls - al

ls

pstree

ps aux

top

# 

fork()  
1. fork()를 호출한 프로세스를 똑같이 복제한 프로세스를 만들고 pid, ppid 만 다름  

2. fork() 호출하면 리턴값이 있음

3. 부모 프로세스쪽 fork()는 복제된 자식 프로세스의 pid를 리턴하고 자식 프로세스 쪽 fork()는 0을 리턴

4. 실패하면 -1을 리턴

가상 메모리
- 주소 공간을 독립적으로 할당하고 있음: 다중 가상 공간

bash cell
- 리눅스에서 명령을 수행해줄 수 있게 해주는 기반

getpid()
- 자신의 process id를 호출(시스템 함수)

getppid()
- 부모의 process id를 호출(시스템 함수)

데몬프로세스
- 고아를 활용(백그라운드로 계속 돌림)

<pre>전역변수 초기화되면 Data 
아니라면 bss 자동으로 0 초기화  
전역변수는 fork() 할때 상속됨</pre>

**exec()**
- **완전히 새 프로그램 이미지로 교체**
- PID, PPID는 그대로

Process 종료

- 정상종료
    * main() 함수 return ...
- 비정상종료
    * **abort() 함수를 호출. 함수를 호출하면 SIGABRT 시그널이 발생됨**
    * **시그널은 프로세스 자신이 보낼 수도 있고 다르 프로세스 또는 커널이 보낼 수 있음**
<pre><code>X: char *p = 0x1004;
O: char *p = (char *)0x1004;</code></pre>

wiat() / waitpid() -> 부모가 사용
- 자식 프로세스의 종료를 기다림