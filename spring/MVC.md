## MVC 웹 프레임워크

정적웹이 아닌 동적웹에서 사용된다.

Model : 데이터의 형식을 지정하고 저장하고 불러오는 작업  
View : 눈에 보이는 것 웹의 경우 html, css로 나타내는 요소   
Controller : Model의 데이터를 View에 연결해서 전반적인 제어를 하는 것

> 잠깐! 라이브러리 vs 프레임워크

라이브러리가 각각의 개별적인 기능들이라고 한다면 프레임워크는 이것들이 연결되어서 기초적인 골격을 갖춘 상태  
가져다 쓰는 것이 라이브러리 기본 틀로 삼아서 위에 덧붙여 만드는게 프레임워크 

## HTTP 요청 데이터

> 클라이언트에서 서버로 데이터를 전달하는 방법은 3가지가 있다.    
    
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


3. HTTP message body에 데이터를 직접 담아서 요청

데이터 형식은 주로 Json을 사용하고 HTTP API에서 주로 사용한다. 

## HTTP 응답 데이터 

> HTTP 응답 메시지도 3가지 방법으로 전달한다.  

1. 단순 텍스트 응답  
2. HTML 응답  
3. HTTP API - MessageBody Json 응답
