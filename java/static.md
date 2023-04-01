## static 변수

> static 변수를 사용하는 목적에는 2가지가 있다.

1. 메모리의 효율  
2. 공유의 목적

```
class Counter  {
    static int count = 0;
    Counter() {
        count++;
        System.out.println(this.count);
    }
}

public class Sample {
    public static void main(String[] args) {
        Counter c1 = new Counter();
        Counter c2 = new Counter();
    }
}
```
위와 같은 코드를 생각해보자. static 변수를 사용하면 count를 공유할 수 있다. static 변수가 없었으면 new Counter()로 객체를 생성할 때  
count가 1 늘어나지만 공유되지는 않아 계속 만들어도 count == 1이다. 이렇게 공유의 목적으로 static을 사용할 수 있다.  
또한 static 키워드를 붙여 메모리 할당을 한번만 하게해서 메모리 사용에 이점도 있지만 공유가 주된 목적이다.

## static 메소드

```
class Counter  {
    static int count = 0;
    Counter() {
        count++;
        System.out.println(count);
    }

    public static int getCount() {
        return count;
    }
}

public class Sample {
    public static void main(String[] args) {
        Counter c1 = new Counter();
        Counter c2 = new Counter();

        System.out.println(Counter.getCount()); 
    }
}
```

보통은 메소드를 만들면 객체를 생성해서 그 객체를 이용하여 메소드를 호출한다.  
하지만 static 메소드는 위와 같이 Counter.getCount()로 클래스를 이용하여 메소드를 호출할 수 있다.  

## static block

static block은 클래스가 로드될 때 한번만 실행됩니다. 생성자가 실행되기 전에 실행됩니다.  
```
@Getter
public class StaticTest {

    private final int value = 10;

    public StaticTest() {
        System.out.println(value);
    }

    static {
        System.out.println("hello");
    }

    static {
        System.out.println("world");
    }

    public static void main(String[] args) {
        StaticTest staticTest = new StaticTest();
    }
}
```
위와 같이 코드를 실행했을 때 결과는 다음과 같습니다.     
<img width="150" alt="image" src="https://user-images.githubusercontent.com/84896838/229268835-fe05a5c9-2824-4ade-8d7b-f04498c60617.png">    
따라서 생성자가 생성되기 전에 static block이 실행된다는 것을 확인할 수 있습니다.    
static 필드를 초기화할 때 사용할 수 있습니다.   

출처 : https://wikidocs.net/228

