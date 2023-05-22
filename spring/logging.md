## 로깅

실무에서는 스프링부트가 제공하는 Slf4j라는 인터페이스 구현한 Logback을 주로 사용한다. 

## 로깅을 사용하는 이유  

> System.out.println()로 출력하지 않고 로그로 출력하는 이유는 크게 4가지가 있다.  

1. 쓰레드 정보, 클래스 이름과 같은 부가 정보를 함께 볼 수 있고 출력 모양 조절 가능

<img width="950" alt="스크린샷 2022-09-18 오후 4 24 35" src="https://user-images.githubusercontent.com/84896838/190890722-e6aed337-df26-44a6-97b6-e45b331c1782.png">

  
2. 로그 레벨에 따라 개발 서버에서는 모든 로그를 출력하고 운영 서버에서는 출력하지 않는 등 로그 상황에 맞게 조절 가능
  
  
```
logging.level.hello.springmvc=debug
``` 
위와 같이 설정을 바꿀 수 있다. trace, debug, info, warn, error 순서이고 default 값은 info이다.
  
  
3. System.out.println()처럼 콘솔에만 출력하지 않고 파일이나 네트워크와 같은 곳에 로그를 별도로 남길 수 있다.  
특히 파일과 같은 경우는 일별, 특정 용량에 따라 로그를 분할 할 수 있다.  
  
4. 성능도 System.out.println()보다 좋다        
System.out.println()이 성능이 안나오는 이유를 한번 살펴보겠습니다.  
println()을 들어가보면     
```
public void println() {
    newLine();
}
```
위와 같이 newLine()이라는 메소드가 있고 newLine()을 다시 들어가보면   
```
private void newLine() {
    try {
        synchronized (this) {
            ensureOpen();
            textOut.newLine();
            textOut.flushBuffer();
            charOut.flushBuffer();
            if (autoFlush)
                out.flush();
        }
    }
    catch (InterruptedIOException x) {
        Thread.currentThread().interrupt();
    }
    catch (IOException x) {
        trouble = true;
    }
}
```
위와 같이 구현되어 있습니다. newLine()은 멀티 쓰레딩 환경에서 동기화를 보장하기 위해 synchronized 키워드를 사용하였습니다.     
synchronized 메소드나 블록은 쓰레드 간에 락을 획득하고 해제하는 과정을 거칩니다. 이러한 락 관리에는 비용이 들어가며   
여러 쓰레드가 동시에 접근해야할 때 성능 저하를 야기할 수 있습니다.      

또한 System.out.println()은 일반적으로 Blocking IO 작업을 수행합니다.   
따라서 System.out.println() 메소드의 출력 작업이 완료될 때까지 현재 쓰레드가 block 되어 대기하게 되어   
성능 저하를 야기할 수 있습니다.

    

### 따라서 실무에서는 꼭 로그를 사용하자!! 

## 사용시 주의할 점

```
log.trace("trace log =" + name);
```
다음과 같이 작성하면 trace를 출력하지 않더라도 더하기 연산이 실행된다. 의미없는 연산이 발생하는 것이다

```
log.trace("trace log={}", name);
```
다음과 같이 사용하자!
