## HTTP 메소드

HTTP 메소드는 HTTP 요청 메세지에서 start line 시작 부분에 적혀있다. (응답 메세지에는 없다)   
주요 메소드에는 GET, POST, PUT, PATCH, DELETE 가 있고 기타 메소드에는 HEAD, OPTIONS, CONNNECT, TRACE 가 있다.   
이러한 메소드를 통해 어떠한 행위를 할 것인지를 알려줄 수 있다. 이번 글에서는 주요 메소드에 대해서 설명하겠다.  

### GET

GET은 리소스를 조회할 때 사용한다. HTTP 요청 메세지는 start-line, header, empty-line, message body로 나뉘었다.  

* 클라이언트 -> 서버

GET의 경우 message body를 통해 전달할 수 있지만 지원하지 않는 곳이 많기 때문에 거의 사용하지 않는다.  
어떤 것을 조회할 지는 header의 Host와 start-line에 적혀 있는 path, 쿼리 파라미터를 통해 클라이언트에서 서버로 전달한다.  
쿼리 파라미터가 없다면 header의 Host만 보고 URL 조회를 요청하면 된다.   

* 서버 -> 클라이언트

서버에서는 데이터를 보낼 방법을 header의 Content-Type에 적고 그 방법으로 message body 부에 데이터를 넣고 클라이언트로 전달한다.   

### POST 

POST는 주로 신규 리소스 등록, 프로세스 처리에 사용한다.  

* 클라이언트 -> 서버 

HTTP 요청 message body를 통해 서버로 요청 데이터를 전달한다. (어떤 방식인지는 밑에서 설명하겠다)  

* 서버 -> 클라이언트 

서버에서는 신규 리소스 식별자를 생성하고 이렇게 서버에서 생성하는 방법을 컬렉션이라고 한다.  
HTTP 응답 메세지에 생성한 정보를 header, message body에 넣어서 클라이언트로 전달한다.   

이러한 방법은 회원가입, 주문에 사용하거나 게시판 글쓰기.. 등에 활용할 수 있다.  
중요한 것은 클라이언트가 서버에 데이터를 전달하고 서버에서 이 데이터의 신규 리소스를 생성한다는 것이다.  
물론 결제완료 -> 배달시작 -> 배달완료 처럼 프로세스를 변경하기 위해 사용하는 경우에는 새로운 리소스를 생성하지 않는다.  

### PUT

PUT은 리소스를 대체할 때 사용한다. 리소스를 단순히 변경하는 것이 아니라 대체하는 것이다.

* 클라이언트 -> 서버  
   
클라이언트가 리소스를 위치를 알고 지정한 후 서버에 전달하는데 이 방법을 스토어라고 한다.  
HTTP 요청 메세지 start-line에 리소스 위치를 적은 후 대체할 데이터를 message body 부에 적는다.  

* 서버 -> 클라이언트

서버에서 리소스가 있는 경우에는 그 리소스를 완전히 대체하고 없다면 생성한다.  
이때 원래 a = 10, b = 20 두 개의 필드가 있었는데 HTTP 요청 메세지에는 a = 100만 있다고 해보자.  
이러면 원래 있던 b 필드는 사라지는 것이다. 변경이 아니라 완전히 대체!   

### PATCH

PATCH는 PUT과 비슷하지만 PUT처럼 완전히 대체가 아니라 리소스 부분 변경을 할 때 사용한다.  
원래 a = 10, b = 20 두 개의 필드가 있었는데 HTTP 요청 메세지에는 a = 100만 있다고 해보자.   
이러면 변경한 후 데이터는 a = 100 , b = 20이다.    

### DELETE 

DELETE는 리소스를 제거할 때 사용한다. 마찬가지로 클라이언트에서 리소스를 식별한 후 서버로 전달한다.   

# HTTP 메소드의 속성

1. 안전 (Safe Methods)

안전은 호출해도 리소스를 변경하지 않는다는 뜻으로 주요 메소드에는 GET만 안전하다.   

2. 멱등 (Idempotent Methods)

멱등은 몇 번을 호출하든 결과가 같다는 뜻이다. GET, PUT, DELETE 가 멱등이다.  
POST는 계속 등록을 하거나 결제를 할 수 있으므로 멱등이 아니다.  
또한 PATCH도 만약 단순히 특정 값으로 변경하는 경우에는 멱등이지만 데이터를 10씩 증가시키는  
경우에는 멱등이 아니다.  

3. 캐시 가능 (Cacheable)

캐시 가능은 응답 결과 리소스를 캐시해서 사용해도 된다는 뜻이다.  
GET, POST, PATCH가 캐시 가능하지만 POST, PATCH는 구현이 쉽지 않기 때문에 GET 정도만 캐시로 사용한다.  

## 클라이언트에서 서버로 데이터 전송

클라이언트에서 서버로 데이터를 전송하는 방법은 크게 나누면 두 가지이다.  
쿼리 파라미터를 통해 데이터를 전송하거나 메세지 바디를 통해 데이터를 전송한다.  

4가지 상황으로 분류하여 클라이언트에서 서버로 데이터를 전송하는 경우를 알아보자.  

1. 정적 데이터 조회 (GET)

이미지나 정적 텍스트 문서를 조회한다. 쿼리 파라미터 없이 header의 Host와 start-line의 path를 보고 조회한다.  

2. 동적 데이터 조회 (GET)

주로 검색, 게시판 목록에서 정렬 필터에 사용한다. HTTP 요청 메세지의 start-line의 path, 쿼리 파라미터를 보고   
서버에서 정렬 필터해서 동적으로 결과를 생성한 후 클라이언트에 전송한다.  

3. HTML Form을 통한 데이터 전송 (GET, POST)

회원 가입, 상품 주문, 데이터 변경에 사용한다. HTTP 요청 메세지의 header의 Content-Type에 application/x-www-form-urlencoded 라고 적고  
message body 부분에는 key & value 형식으로 작성해서 전송한다.  
참고로 HttpServletRequest을 사용할 때 쿼리 파라미터 방식과 동일하게 조회할 수 있다.  
```
String username = request.getParameter("username");
String age = request.getParameter("age");
```
GET 방식으로 데이터를 전송할 수 있지만 조회에만 사용해야하고 리소스 변경이 발생하는 곳에 사용하면 안된다.   

또한 파일 업로드 같은 binary 데이터를 전송해야 할 때는 HTML Form에 enctype = "multiple/form-data"를 추가하고 
HTTP 요청 메세지의 header의 Context-Type 도 "multiple/form-data"라고 적어서 보낸다.  

4. HTTP API를 통한 데이터 전송 (GET, POST, PUT, PATCH)

회원 가입, 상품 주문, 데이터 변경, 서버 to 서버, 앱 클라이언트, 웹 클라이언트(Ajax)에 사용한다.  
GET은 조회할 때 HTTP 요청 메세지의 쿼리 파라미터로 데이터를 전달하고 POST, PUT, PATCH는 message body를 통해 전달한다.   
이때 Content-Type은 최근에는 application/json을 주로 사용한다.  

출처 : 모든 개발자를 위한 HTTP 웹 기본 지식 (김영한)
