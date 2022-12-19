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
