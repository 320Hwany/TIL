## Call by Value

Call by value는 값에 의한 호출이다. 다르게 표현하면 값만 전달한다고 할 수 있다.  
따라서 call by value 방식으로 값을 전달받고 수정을 해도 원본 값은 바뀌지 않는다.  
값을 전달받으면 스택에 메모리가 할당된다. 서로 같은 메모리가 아님!  

```
class CallByValue {

    @Test
    void test() {
        int a = 0;  -- (1)
        int b = 0;  -- (1)
        assertEquals(a, 0);  
        assertEquals(b, 0);

        addTest(a, b); 

        assertEquals(a, 0);
        assertEquals(b, 0);
    }

    void addTest(int a, int b) {
        a += 10;
        b += 10;
        System.out.println(a); // 여기선 a == 10
        System.out.println(b); // 여기선 b == 10
    }
}
```
위의 테스트가 성공하였다. addTest를 하여도 a, b의 값이 각각 10으로 바뀌지 않은 이유는 주소 값을 전달 받은 것이 아니라  
별도의 메모리 공간에 그 값 자체를 복사한 것이기 때문이다. 따라서 (1)의 a,b 와 addTest(a, b)의 a, b는 각각 다른 메모리에 있는 다른 값이다.


## Call by Reference

call by reference는 참조에 의한 호출이다. 값을 수정할 때 그 주소 값에서 원본 값을 바꾼다.  

<img width="770" alt="스크린샷 2022-09-27 오후 10 57 05" src="https://user-images.githubusercontent.com/84896838/192546675-e3b15f59-1f93-4500-b1b8-12caafdb9dd4.png">

위와 같은 경우 call by reference 인 것 같지만 call by value이다.  

왜냐하면 method1 에서 method2로 값을 넘겨줄 때 Person의 주소 값을 넘겨준다!!  
서로 다른 메모리에 같은 주소 값을 가지고 있는 것이다. 같은 메모리가 아님!  
따라서 call by value 이지만 값을 변경할 수 있는 것이다.

참고로 자바는 call by value 방식만 가능하다.

출처 :https://velog.io/@ahnick/Java-Call-by-Value-Call-by-Reference
