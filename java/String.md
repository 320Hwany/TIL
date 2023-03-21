## String    

String은 CharSequence 인터페이스를 구현한 구현체이다.  

```
String a = new String("hello");
```  
위와 같이 new 키워드로 객체를 만들어서 사용할 수 있지만 아래와 같은 리터럴 방식을 더 많이 사용한다.  

```
String a = "hello";
```

두 방식의 차이점을 테스트해보자

### StringTest

```
public class StringTest {

    String a = "hello";
    String b = "hello";
    String c = "hi";

    String aa = new String("hello");
    String bb = new String("hello");
    String cc = new String("hi");

    @Test
    void test1() {
        System.out.println(a == a); // true
        System.out.println(a == b); // true
        System.out.println(a == c); // false
    }

    @Test
    void test2() {
        System.out.println(aa == aa); // true
        System.out.println(aa == bb); // false
        System.out.println(aa == cc); // false
    }
}
```
  
따라서 new String은 값이 같아도 객체를 계속 생성하고 리터럴 방식은 값이 같으면 재사용한다는 것을 확인할 수 있다.   
String의 특징에 대해 더 자세히 알아보자.   

* String 자료형은 리터럴 방식으로 표기 가능하지만 primitive 자료형이 아니다.  
* String은 불변객체이다. 
* 가비지 컬렉터에 의해서 제거된다.

```
String a = "hello";
a = "hi";
``` 
위와 같은 코드에서 a의 값 자체가 변했다고 생각할 수 있지만 "hello"가 들어간 메모리 말고 또 다른 영역의 메모리에  
"hi"라는 값이 들어가고 "hello"는 가비지 컬렉터에 의해서 삭제될 수 있다.   

## String이 불변 객체인 이유 

1. 성능   
 불변이기 때문에 위와 같이 String은 heap 영역 안에 있는 String pool에 저장되어 같은 문자열은 새로 생성하지 않고  
 재사용할 수 있다.
 
2. 동기화    
 불변 객체는 thread-safe하기 때문에 멀티 쓰레드 환경에서 사용해도 된다.   

3. 캐싱   
 String의 hashcode()를 사용할 때마다 호출하지 않고 처음 한 번만 사용하는데 이것은 String이 불변이기 때문에   
 캐싱하여 사용할 수 있다.  
 
4. 보안   
 문자열은 서비스에서 아이디, 비밀번호와 같이 개인적인 정보를 저장하기 위해 많이 사용한다.  
 해커가 임의적으로 문자열을 변경할 수 없으므로 보안상 더 안전하다.   

## String의 문제점   

String은 불변성이다. 따라서 사용 빈도가 많다면 메모리가 부족해질 수 있다. 따라서 애플리케이션 성능에 영향을 미칠 수 있다.  
이를 해결하기 위하여 불변성이 아닌 가변성의 특징을 가지는 StringBuffer, StringBuilder를 도입하였다.  

## StringBuffer VS StringBuilder

둘다 가변성을 가지는 클래스이지만 동기화 지원 유무에서 차이가 있다. StringBuffer는 동기화를 지원하여 멀티쓰레드 환경에서 안전성을 가진다.  
StringBuilder는 동기화를 지원하지 않아서 멀티쓰레드 환경에서 안전성을 가지지 않는다.  

마지막으로 문자열 클래스 3가지를 어떤 환경에서 사용하면 좋을지를 정리해보자.  
* String : 문자열 연산이 적고 멀티쓰레드 환경  
* StringBuffer : 문자열 연산이 많고 멀티쓰레드 환경  
* StringBuilder : 문자열 연산이 많고 싱글쓰레드 환경, 동기화를 고려하지 않아도 되는 환경  
이때 싱글쓰레드 환경에서는 StringBuilder가 StringBuffer보다 성능이 좋다

