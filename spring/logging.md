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
