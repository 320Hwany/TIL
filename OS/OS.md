## 운영체제의 역할  

운영체제는 프로세스를 관리하고 메인 메모리, 하드디스크와 같은 저장장치를 관리한다. 또한 운영체제는 응용 프로그램과 하드웨어 간의  
인터페이스 역할을 하며 사용자가 컴퓨터를 쉽게 사용할 수 있는 환경을 제공한다. 

## interrupt

* interrupt 처리란 어떠한 일을 하고 있는데 다른 이벤트가 발생하여 그 이벤트가 우선으로 처리되어야 함을 CPU에게 알리는 것이다.  
interrupt가 없다면 어떤 일이 끝나기 전까지 다른 일을 하지 못해 CPU 효율이 떨어지게 된다.   


* interrupt는 하드웨어, 소프트웨어적으로 발생할 수 있다.
> Program Counter (PC) : 인터럽트 처리시 어디까지 처리 했었는지를 알기 위해서 레지스터에 그 위치를 저장한다.

## dual mode operation

커널, 유저모드로 나뉜다. 이렇게 두 가지 모드로 나누는 이유는 사용자가 함부로 운영체제에 접근하지 못하게 하기 위해서이다.  
커널은 자원을 컨트롤하며 사용자가 접근할 수 없는 모드이고 유저 모드는 사용자가 접근할 수 있는 모드이다. 

## System Call

* System Call은 운영체제에서 제공하는 미리 구현된 유용한 함수이다. 사용자가 GUI나 CLI를 이용하여 명령어를 입력하면 system call이라는 함수를 이용하여 
명령어 프로그램 수행에 필요한 여러가지 서비스를 제공한다. 
* System Call은 소프트웨어 인터럽트의 한 종류이다.


* 프로세스 관리를 위한 System call에는 fork(), wait(), exec() 등이 있다. 
  1. fork() : 프로세스 생성을 위한 System call로 parent 프로세스와 똑같은 child 프로세스가 복제된다.(process id만 다름)  
  2. wait() : parent 프로세스가 child 프로세스의 종료를 대기해야 하는 경우에 사용한다. 
  3. exec() : parent와 다른 프로그램을 실행할 때 사용한다. 

## 프로세스 

* 사용 설명서를 프로그램이라고 한다면 사용 설명서대로 따라하는 동작을 프로세스라고 비유할 수 있다.  

* 각 프로세스는 별도의 메모리 공간을 할당한다. 프로세스의 주소공간은 코드, 데이터, 힙, 스택이 있다.  
코드는 프로그램의 소스코드를 저장하는 곳이고 데이터는 전역변수를 저장, 힙은 동적 할당 시 사용, 스택은 함수와 지역변수를 저장한다.  

* 프로세스 상태로는 new, running, waiting, ready, terminated 가 있다.  
new : 프로세스가 새로 생성된 상태  
running : cpu를 할당 받아서 실질적으로 동작하는 상태  
waiting : 어떠한 이벤트를 기다리는 상태  
ready : wait 하던 것이 이벤트가 발생하면 ready 상태가 된다. cpu 할당을 기다리는 상태  
terminated : 종료 상태

* PCB(Process Control Block) : 프로세스를 관리하기 위한 테이블   
PCB가 필요한 이유는 CPU가 여러 개의 프로세스를 관리하기 위해 각 프로세스의 정보를 알아야 하기 때문이다.     
프로세스 테이블에는 process state, process priority, PC, registers, memory usage 와 같은 정보들이 저장되어 있다.

* Context Switching  
CPU가 이전 프로세스 정보를 PCB에 저장하고 (Context save) 또 다른 프로세스의 정보를 PCB에서 읽어 레지스터에 적재하는 과정이며
이 과정은 프로세스 상태가 바뀔 때 발생한다.  
CPU에게 프로세스를 계속 할당해주기 때문에 효율을 높일 수 있다. 하지만 Context Switching때는 
CPU가 아무런 일을 하지 못하기 때문에 너무 많은 Context Switching으로 인해 overhead가 발생할 수 있다.

* IPC(Inter Process Communication)  
프로세스들은 서로 독립되어 있는데 이러한 프로세스 간의 통신을 말한다. 
  1. Shared memmory : 같은 메모리를 공유함으로써 프로세스 간의 협력이 read, write 방식으로 이루어진다.  
  2. Message passing : 프로세스 간의 협력이 send, receive 방식으로 이루어진다. 

  pipes : 한 프로세스가 다른 프로세스에게 데이터를 전달하는 길  
  parent는 pipe를 생성하고 parent가 생성한 pipe를 child가 물려 받음  
    1. ordinary pipes : parent-child relationship이 있을 때 사용한다. 한쪽 방향으로만 통신가능  
    2. Named pipes : parent-child relationship이 없고 전혀 상관없는 프로세스끼리 통신가능. 한쪽 방향으로만 통신가능

## 쓰레드  
쓰레드는 프로세스 안에서의 실행 단위이다. 한 프로세스 안에서 쓰레드들은 메모리를 공유한다.  
프로세스의 메모리에는 코드, 데이터, 힙, 스택이 있다. 여기서 코드, 데이터, 힙 부분을 공유하고 스택 부분은  
각 쓰레드가 따로 할당 받는다.

> 멀티 프로세스 : 하나의 프로그램을 여러 개의 프로세스로 수행한다. 이때 각 프로세스는 독립적인 메모리 영역을 갖고 있어 안정성이 높다.  
하지만 메모리를 공유하지 않고 context switching 비용으로 overhead로 인해 성능이 저하될 수 있다.

> 멀티 쓰레드 : 하나의 프로그램에서 여러 쓰레드로 각 쓰레드가 각자 하나의 작업을 수행한다. 프로세스 안에서 메모리를 공유하기 때문에 IPC 통신을  
하던 멀티 프로세스 보다 성능이 좋다. 하지만 자원을 공유하기 때문에 동시성 문제에 대해서 주의해야 한다.

