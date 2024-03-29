## ==, equals, hashcode 비교

1.  == (동일성 비교)

두 객체가 있을 때 == 비교는 두 객체가 완전히 같다는 것이다. 즉 단순히 값뿐만 아니라 주소 값 자체가 같아야 true를 반환한다.  

2. equals (동등성 비교), hashcode

equals, hashcode는 모든 객체의 부모 클래스인 object 클래스에 정의되어 있다.   
```
public boolean equals(Object obj) {
        return (this == obj);
}
```
기본적으로 equals는 동일성 비교이다. 동일성 비교가 아니라 동등성 비교가 필요할 때 재정의해서 사용한다.   
예를들어 Integer, String 처럼 값을 표현할 때 객체가 같은지 비교하는 것이 아니라 값 자체가 같은 지  
비교하고 싶을 때 재정의 한다. Hashcode는 객체를 식별하는 Integer 값이다.  

두 객체가 equals()에 의해서 같으면 hashcode()도 같다.  
-> 두 객체가 hashcode()에 의해 다르면 equals()도 다르고 서로 다른 객체다.  

두 객체가 equals()에 의해서 다르더라도 hashcode()는 같을 수 있다. 단 hashcode()가 다른 값을 반환해야 성능이 좋다.   
-> 두 객체가 hashcode()에 의해 같으면 equals()는 같을 수도 있고 다를 수도 있다. 서로 같은 객체인지 아닌지 확인하지 못한다. 

코드로 확인해보자. 먼저 Human 클래스를 만들고 equals, hashcode를 재정의 하였다.

```
static class Human {
    private String name;
    private int age;

    public Human(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Human human = (Human) o;
        return age == human.age && Objects.equals(name, human.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }
}
```

test1()에서는 서로 다른 두 객체이지만 name, age가 같은 객체가 있다.  
결과는 hashcode()가 같고, equals()도 true지만 == 비교로 서로 다른 객체라는 것을 확인할 수 있다.  
```
@Test
void test1() {
    Human human1 = new Human("aaa", 10);
    Human human2 = new Human("aaa", 10);
    System.out.println(human1.hashCode()); // 2986922
    System.out.println(human2.hashCode()); // 2986922
    System.out.println(human1.equals(human2)); // true
    System.out.println(human1 == human2); // false
}
```

test2()에서는 서로 다른 두 객체이고 name, age도 서로 다른 객체가 있다.  
결과는 hashcode()가 다르고, equals()도 false이고 == 비교로 서로 다른 객체라는 것을 확인할 수 있다.
```
@Test
void test2() {
    Human human1 = new Human("aaa", 10);
    Human human2 = new Human("bbb", 20);
    System.out.println(human1.hashCode()); // 2986922
    System.out.println(human2.hashCode()); // 3017715
    System.out.println(human1.equals(human2)); // false
    System.out.println(human1 == human2); // false
}
```

## hashcode 사용하는 이유

그렇다면 equals()만 있으면 될 것 같은데 hashcode를 사용하는 이유는 뭘까?    

바로 객체를 비교할 때 드는 비용을 낮추기 위해서이다. equals는 hashcode보다 많은 시간이 소요된다.   
따라서 객체를 비교할 때 우선 hashcode를 비교한다. hashcode가 다르면 equals를 사용하지 않고 서로 다른 객체임을 알 수 있다.   
또한 hashcode가 같아도 다른 객체일 수 있다. 이는 비교를 하는데 비용을 더 들이기 때문에  
서로 다른 객체라면 다른 hashcode를 갖도록 하는 것이 효율적임을 알 수 있다.   

hashcode는 이렇게 equals와 관련이 있기 때문에 equals를 override하는 경우라면 hashcode도 반드시 같이 override하자. 

## hashcode 구현

override한 hashcode를 살펴보자  
```
@Override
public int hashCode() {
    return Objects.hash(name, age);
}
```
Objects.hash를 들어가보자 
```
public static int hash(Object... values) {
    return Arrays.hashCode(values);
}
```
다시 Arrays.hashCode를 들어가보자 
```
public static int hashCode(Object a[]) {
    if (a == null)
        return 0;

    int result = 1;

    for (Object element : a)
        result = 31 * result + (element == null ? 0 : element.hashCode());

    return result;
}
```

hashcode가 반환한 수는 위와 같은 과정으로 반환되었다.

출처 : https://codechacha.com/ko/java-hashcode/
