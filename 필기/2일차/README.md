# OS 2일차 노트정리

**Signal 소개**
- 프로세스에게 어떤 이벤트의 발생을 알리기 위해 사용하는 소프트웨어 인터럽트  
  * Hardware exception  
  * Software condition  
  * 터미널 사용자 입력  
  <br/>
  
- 인터럽트: 비동기적으로 발생하는 사건  
- A -> B 사건 줌 / 시그널 핸들러(시스템 호출 함수)
- kill raise
- **kill sigalrm(14), sigusr1(10), sigusr2(12) (중요)**
- 터미널의 사용자 입력: ctrl + c / ctrl - \
- 프로세스는 시그널을 받으면 미리 정의된 동작을 수행  
<br/>

**Signal 전달**
- 시그널 멈춰 있으면 pending
- 멈춰있는 signal -> pending signal
- 커널이 프로세스를 감시
- **alarm 설정으로 특정 시간이 지나면 시그널 전달**  
<br/>

**Signal 처리**
- SIG_IGN: 시그널을 무시함
- SIG_DFL: 미리 정해진 default 동작을 수행
- **Signal handler : 실행할 할수를 미리 등록해 놓고 해당 시그널을 받으면 지정한 함수를 실행**  
<br/>

- signal(arg1, arg2)
- arg1: 핸들링 하고자 하는 신호
- arg2: 핸들링 방식  
  * SIGINT : ctrl + c / SIGQUIT : ctrl + \  
<br/>

- Action : Core (코어 덤프 -> 블랙박스)
- 원래 Action = Terminate -> Handler 등록 실행 => catch  
<br/>

- SIGKILL(Term, NoCatch, NoIgn) SIGSTOP(Term, NoCatch, NoIgn) / 9,19 
<br/>  

**Signal 전달2**
- raise(int signo) : 자기 자신에게 signo에 지정한 시그널을 보냄
- **alarm(unsigned int seconds) : 정해진 시간 후에 자기 자신에게 sigalrm 시그널을 보냄**
- absort() : 자기 자신에게 SIGABRT 시그널을 보냄
- **kill(pid_t pid, int signo) : pid 프로세스에게 지정한 시그널을 보냄**
- absort() 비정상적 종료
- pasue() : 신호올때까지 기다렸다가 다음 파트 실행
- 풀링: 핸들 함수에 알람 넣어주면 됨(주기적 감시)