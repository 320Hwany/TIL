## ThreadLocal

싱글톤 방식을 사용하면 전역변수, 멤버변수를 사용할 때 주의해야합니다.   
지역 변수는 쓰레드마다 다른 메모리에 할당되지만 멤버변수, 전역변수는 공유하기 때문입니다.     

먼저 문제가 발생하는 상황을 살펴보겠습니다.   

### BasicService

```
@Service
public class BasicService {

    private int price; // 문제가 발생하는 변수

    public void setPrice(int inputPrice) {
        this.price = inputPrice;
        sleep(1000);
        System.out.println(price);
    }

    private void sleep(int millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

@Service를 사용하여 BasicService를 스프링 빈으로 등록하였습니다. 그러므로 BasicService는 싱글톤입니다.   
그리고 setPrice 메소드는 파라미터로 들어온 inputPrice로 basicService의 price 값을 변경하고 1초의 텀을 두고   
price를 출력합니다.


### BasicServiceTest
```
@SpringBootTest
class BasicServiceTest {

    @Autowired
    private BasicService basicService;

    @Test
    void basicService() {
        Runnable runnableA = () -> {
            basicService.setPrice(1000);
        };

        Runnable runnableB = () -> {
            basicService.setPrice(2000);
        };


        Thread threadA = new Thread(runnableA);
        threadA.setName("thread-A");
        Thread threadB = new Thread(runnableB);
        threadB.setName("thread-B");

        threadA.start();
        sleep(100);
        threadB.start();

        sleep(3000); //메인 쓰레드 종료 대기
    }

    private void sleep(int millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```
basicServiceTest는 두 개의 쓰레드를 만들고 threadA를 먼저 실행하고 곧 바로 threadB를 실행합니다.    
이때 setPrice에서 1초의 텀을 두었기 때문에 다음과 같은 순서로 진행됩니다.     
threadA가 price를 1000으로 바꿈 -> threadB가 price를 2000으로 바꿈 -> threadA가 price를 출력 -> threadB가 price를 출력   

이때 결과는 2000이 두번 나오게 됩니다. threadA는 자신은 1000으로 바꾸었지만 동시성 문제로 2000을 출력하게됩니다.  

이러한 문제를 해결하기 위해서 ThreadLocal을 사용해야 합니다. 위와 같은 상황에서 int를 ThreadLocal<Integer>로 바꿔보겠습니다.   
  
### ThreadLocalSerivce
  
```
@Service
public class ThreadLocalService {

    private ThreadLocal<Integer> price = new ThreadLocal<>();  // ThreadLocal 사용

    public void setPrice(int inputPrice) {
        price.set(inputPrice);
        sleep(1000);
        System.out.println(price.get());
        price.remove();
    }

    private void sleep(int millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```
  
### ThreadLocalSerivceTest
  
```
  @SpringBootTest
class ThreadLocalServiceTest {

    @Autowired
    private ThreadLocalService threadLocalService;

    @Test
    void threadLocalService() {
        Runnable runnableA = () -> {
            threadLocalService.setPrice(1000);
        };

        Runnable runnableB = () -> {
            threadLocalService.setPrice(2000);
        };


        Thread threadA = new Thread(runnableA);
        threadA.setName("thread-A");
        Thread threadB = new Thread(runnableB);
        threadB.setName("thread-B");

        threadA.start();
        sleep(100);
        threadB.start();

        sleep(3000); //메인 쓰레드 종료 대기
    }

    private void sleep(int millis) {
        try {
            Thread.sleep(millis);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```
  
ThreadLocal을 사용하면 각 쓰레드마다 별도의 내부 저장소를 제공합니다. 따라서 threadA가 저장한 값, threadB가 저장한 값을    
따로 저장해주기 때문에 동시성 문제를 해결할 수 있습니다. 따라서 결과 값은 각각 1000, 2000이 나옵니다.  
 
### 주의할 점
  
해당 쓰레드가 ThreadLocal을 모두 사용하고 나면 ThreadLocal.remove()를 사용해서 ThreadLocal에 저장된 값을 삭제해주어야 합니다.   
저장된 값을 삭제하지않으면 다음에 같은 쓰레드가 호출 되었을 때 그 값을 불러올 수 있기 때문입니다.   

출처 : 스프링 핵심 원리 - 고급편 (인프런 - 김영한)


