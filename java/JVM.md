# JVM (Java Virtual Machine)

JVM은 자바 바이트코드(.class)를 실행하는 주체입니다. JVM이 .class를 해당 OS에 맞는 기계어로 변환해주는 것입니다.      
따라서 JVM 자체는 플랫폼에 종속적이지만 JVM만 있다면 개발자가 작성한 .java 자바 코드는 타겟 플랫폼과 상관없이 같은 동작을 할 수 있습니다.        

자바의 컴파일 과정은 다음과 같습니다.   

개발자가 작성한 .java 파일이 자바 컴파일러(javac)를 통해 바이트코드인 .class 파일로 변환됩니다.    
그 후 Class Loader를 통해 JVM의 메모리인 Runtime Data Area로 올라갑니다.    
JVM의 실행엔진을 통해 바이트코드가 기계어로 바뀌어 실행됩니다.  

## JVM 구조

JVM 구조는 크게 4가지로 나눠볼 수 있습니다.   

1. 클래스 로더 (Class Loader)

   JVM 메모리에 .class 파일을 로드하는 역할을 합니다.

   크게 3가지 단계로 진행됩니다.
   
   (1) 로딩        
   클래스 로더가 .class 파일을 읽고 바이너리 데이터를 만들어 메소드 영역에 저장합니다.         
   로딩이 끝나면 Class 객체를 생성하여 힙 영역에 저장합니다.        
   이때 클래스 로더는 계층 구조로 되어있으며 부트스트랩 > 플랫폼 > 애플리케이션 클래스 로더로 구성됩니다.           

   (2) 링크       
   링크 과정은 레퍼런스를 연결하는 과정입니다.         
   이 과정은 Verify, Prepare, Resolve 3단계로 이루어집니다.        
   Verify 단계에서는 .class 바이트 코드 형식이 유효한지 확인합니다.       
   Prepare 단계에서는 클래스에 필요한 변수와 기본값에 필요한 메모리를 준비하는 과정입니다.       
   Resolve 단계에서는 논리적인 레퍼런스(실제 레퍼런스 X)를 힙에 존재하는 레퍼런스를 가리키도록 교체합니다. (이 과정은 선택)              

   (3) 초기화         
   링크 과정에서 Prepare 단계에서 준비해둔 메모리에 클래스 변수의 값을 할당합니다.        
   
3. Runtime Data Area         

   JVM의 메모리 영역으로 5가지 영역으로 나뉩니다.   

4. 실행 엔진 (Execution Engine)

   클래스 로더를 통해 JVM 메모리에 로드된 바이트코드를 기계어로 번역합니다.  
   이때 기계어로 번역할 때 JIT Compiler, Interpreter 2가지 방법이 있습니다.    
   [자바 컴파일 과정](https://github.com/320Hwany/TIL/blob/main/java/%EC%9E%90%EB%B0%94%20%EC%BB%B4%ED%8C%8C%EC%9D%BC%20%EA%B3%BC%EC%A0%95.md)    
   
5. 네이티브 메소드 인터페이스 (JNI)

   자바 애플리케이션에서 C, C++, 어셈블리어로 작성된 함수를 사용할 수 있도록 인터페이스를 제공합니다.    
   
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

출처 : [우아한 테크 [10분 테코톡] JVM Stack & Heap](https://steady-coding.tistory.com/305)               
      더 자바, 코드를 조작하는 다양한 방법 - 인프런 (백기선)   
