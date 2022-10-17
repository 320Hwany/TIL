## String    

```
String a = new String("hello");
```  
위와 같이 new 키워드로 객체를 만들어서 사용할 수 있지만 아래와 같은 리터럴 방식을 더 많이 사용한다.  

```
String a = "hello";
```

* 이렇게 String 자료형은 리터럴 방식으로 표기 가능하지만 primitive 자료형이 아니다.  
* 따라서 메모리 공간이 변하지 않는다(immutable) 
* 가비지 컬렉터에 의해서 제거된다.

```
String a = "hello";
a = "hi";
``` 
위와 같은 코드에서 a의 값 자체가 변했다고 생각할 수 있지만 "hello"가 들어간 메모리 말고 또 다른 영역의 메모리에  
"hi"라는 값이 들어간다. 
