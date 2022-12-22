# Os 4일차

Thread 소개
- 쓰레드 전역변수 공유가능
- Light-Weight 프로세스
- .text(코드 영역)(명령어) 섹션, .data 섹션 등은 공유 됨 -> 메모리 소비 작음
- IPC 사용 가능(but 전역변수 주로 사용)
- 부모 쓰레드 / 자식 쓰레드 ( pthread_create())
- 부모 프로세스 의존적 / 부모 프로세스 종료되면 삭제 됨
- 프로세스 내의 모든 스레드 메모리 공간 공유 , 한 스레드 잘못 모든 스레드 영향 -> 디버깅 어렵  

<br/>

<pre><code>gcc -o thread1 thread1.c -lpthrea</code></pre>

<pre><code>pthread_create(pthread_t  *  thread, pthread_attr_t *
attr, void * (*start_routine)(void *), void * arg);</code></pre>

<pre><code>pthread_create(* 쓰레드id, 속성, 쓰레드함수, 인자)</code></pre>

