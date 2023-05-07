# JVM (Java Virtual Machine)

C, C++ 언어는 컴파일 플랫폼과 타켓 플랫폼이 다를 경우 프로그램이 동작하지 않습니다.  
자바 바이트코드는 JVM이 설치된 플랫폼이라면 타켓 플랫폼과 상관 없이 동작할 수 있습니다.  

자바의 컴파일 과정은 다음과 같습니다.   

개발자가 작성한 .java 파일이 자바 컴파일러를 통해 바이트코드인 .class 파일로 변환됩니다.    
그 후 Class Loader를 통해 JVM의 메모리인 Runtime Data Area로 올라갑니다.    
JVM의 실행엔진을 통해 바이트코드가 기계어로 바뀌어 실행됩니다.  

## JVM 구조

JVM 구조는 크게 4가지로 나눠볼 수 있습니다.   

1. 클래스 로더 (Class Loader)

   JVM 메모리에 .class 파일을 로드하는 역할을 합니다.  

2. 실행 엔진 (Execution Engine)

   클래스 로더를 통해 JVM 메모리에 로드된 바이트코드를 기계어로 번역합니다.  
   이때 기계어로 번역할 때 JIT Compiler, Interpreter 2가지 방법이 있습니다.    
   [자바 컴파일 과정](https://github.com/320Hwany/TIL/blob/main/java/%EC%9E%90%EB%B0%94%20%EC%BB%B4%ED%8C%8C%EC%9D%BC%20%EA%B3%BC%EC%A0%95.md)    
   
   
3. 가비지 컬렉터 (Garbage Collector)    

   힙 메모리 영역에 생성된 객체들 중에서 참조되지 않은 객체들을 탐색 후 제거하는 역할을 합니다.
   
4. Runtime Data Area         

   JVM의 메모리 영역으로 5가지 영역으로 나뉩니다.    
   
## Runtime Data Area

Runtime Data Area는 Method Area, Heap, Stack, PC register, Native Method Stack으로 나뉩니다.  
Method Area, Heap은 모든 쓰레드가 공유하고 Stack, PC register, Native Method Stack은 각 쓰레드마다 1개씩 가지고 있습니다.   

- Method Area

Method 영역은 JVM이 시작될 때 생성되는 공간으로 바이트코드가 저장됩니다.    
이 영역에 클래스 정보, static으로 선언한 변수가 저장됩니다.

- Heap

Heap 영역은 동적으로 생성된 객체가 저장되는 영역으로 GC의 대상이 되는 공간입니다.  
Heap 영역은 효율적인 GC를 위하여 Young Generation(eden, s0, s1), Old Generation, Permanent Generation으로 나뉩니다.   

- Stack

Stack 영역은 지역변수나 메소드의 매개변수, 임시적으로 사용되는 변수, 메소드의 정보가 저장되는 영역입니다.    

- PC register

쓰레드가 시작될 때 생성되며 현재 수행 중인 JVM의 명령어 주소를 저장하는 공간입니다.   
쓰레드가 어떤 부분을 어떤 명령어로 수행할 지를 저장하는 공간입니다.   

- Native Method Stack

Native Method Stack은 Java가 아닌 다른 언어로 작성된 코드를 위한 공간입니다.     

## 특징

JVM은 연산을 할 때 레지스터를 이용하는 C, C++ 언어와 다르게 Stack을 이용합니다.   
CPU의 레지스터는 CPU 아키텍쳐마다 다를 수 있습니다. 하지만 Stack을 이용하기 때문에   
타켓 플랫폼과 상관없이 JVM만 있다면 동작할 수 있도록 설계하였습니다.   

출처 : 우아한 테크 [10분 테코톡] JVM Stack & Heap,    
      https://steady-coding.tistory.com/305

