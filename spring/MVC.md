## HTTP 요청 데이터

클라이언트에서 서버로 데이터를 전달하는 방법은 3가지가 있다.    
    
1. GET - 쿼리 파라미터

/url?username=hello&age=20  

다음과 같이 쿼리 파라미터로 받으면

```String username = request.getParameter("username");```  
```String age = request.getParameter("age");```  


메시지 바디없이 데이터를 전달할 수 있다.  
검색, 필터, 페이징 등에서 많이 사용하는 방식이다.  


2. POST - HTML Form  

```
<form action="/request-param" method="post">
    username: <input type="text" name="username" />
    age: <input type="text" name="age" />
    <button type="submit">전송</button>
</form>
```

username, age를 직접 입력하고 전송 버튼을 누르면 post방식으로 /request-param 에 데이터가 전달된다.   
메시지 바디에 쿼리 파라미터 형식으로 데이터를 전달한다.  
주로 회원 가입, 상품 주문 등에 사용하는 방식이다.  


HTTP message body에 데이터를 직접 담아서 요청

데이터 형식은 주로 Json을 사용하고 HTTP API에서 주로 사용한다. 
