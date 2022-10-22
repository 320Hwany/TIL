## Overriding

상위 클래스나 인터페이스에 존재하는 메소드를 재정의하는 것을 말한다.   
이미 존재하는 메소드와 메소드명, 넣어주는 매개변수, 리턴타입이 같다.  
메소드 내부의 구현부를 재정의하는 것이다.

```
public int overridingTest(int a){
   return a;
}
```
위와 같은 메소드가 상위 클래스나 인터페이스 존재한다고 해보자.

```
@Overriding
public int overridingTest(int a){
   return a+a;
}
```
다음과 같이 메소드 명, 넣어주는 매개변수, 리턴 타입은 모두 같지만  
내부의 구현부를 다르게 재정의 할 수 있다.

## Overloading

상위 클래스나 인터페이스에 존재하는 메소드를 재정의하는데   
이미 존재하는 메소드와 메소드명은 같으나 넣어주는 매개변수는 다르고 리턴타입은 다를 수도 있다.
```
public int overloadingTest(int a){
   return a;
}
```
위와 같은 메소드가 상위 클래스나 인터페이스 존재한다고 해보자.  
```
@Overloading
public int overloadingTest(int a, int b){
   return a+b;
}
```   
다음과 같이 넣어주는 매개변수가 다른 메소드명이 같은 메소드를 재정의 할 수 있다.
