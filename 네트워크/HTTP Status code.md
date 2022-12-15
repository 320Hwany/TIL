## HTTP Status Code

클라이언트가 서버로 HTTP 요청 메세지를 보냈을 때 서버가 클라이언트에게 HTTP 응답 메세지를 보내준다.  
HTTP 응답 메세지의 start line에 HTTP Status Code가 있는데 요청의 처리 상태를 나타낸다.  

Status Code는 100번대부터 500번대까지 있다.  
1xx (Information) : 요청이 수신되어 처리중 (거의 사용하지 않는다)  
2xx (Successful) : 요청 정상 처리   
3xx (Redirection) : 요청을 완료하려면 추가 행동이 필요   
4xx (Client Error) : 클라이언트 오류   
5xx (Server Error) : 서버 오류   

### 2xx - 요청 정상 처리

* 200 OK 

요청을 성공했을 때 보낸다. 예를들어 조회를 하려고 할 때 클라이언트에서 서버로 GET 요청을 보내고    
서버에서는 메세지 바디에 데이터를 넣어서 보낸다.   

* 201 Created

요청을 성공했을 때 서버에서 새로운 리소스를 생성할 때 보낸다. 예를들어 회원가입을 하려고 클라이언트에 서버로 POST 요청을 보내면  
서버에서는 생성된 리소스를 응답의 Location 헤더 필드에 넣어서 보낸다.  

* 202 Acceptd 

요청이 접수되었으나 처리가 완료되지 않았을 경우이다.   

* 204 No content

서버가 요청을 성공적으로 수행했지만 응답 메세지에 데이터를 넣지 않고 보낸다. 예를들어 글을 작성하고 저장 버튼을 누르면  
서버에서는 클라이언트에게 데이터를 보내지 않고 저장되었다는 사실만 알려주면 된다.   

### 3xx - Redirection

Redirection은 3xx 응답 결과에 Location header가 있으면 Location 위치로 이동하는 것을 말한다.  
Redirection에는 영구, 일시, 캐시 3가지 종류가 있다.   

* 영구 Redirection 301(Moved Permanently), 308(Permanent Redirect)

영구 Redirection은 리소스의 URI가 영구적으로 이동한다.   
예를들어 클라이언트가 서버에 POST 요청을 했다고 해보자. 301은 요청 메소드가 GET으로 바뀌고 본문 내용이 제거될 수 있다.  
308은 요청 메소드를 POST로 유지하며 본문 내용도 유지한다. (301과 기능은 같다)    

* 일시 Redirection 302(Found), 307(Temporary Redirect), 303(See Other) 

일시 Redirection은 리소스의 URI가 일시적으로 변경된다.  
마찬가지로 클라이언트가 서버에 POST 요청을 했다고 해보자. 302는 요청 메소드가 GET으로 바뀌고 본문 내용이 제거될 수 있다.  
307은 요청 메소드를 POST로 유지하며 본문 내용도 유지한다. (302와 기능은 같다)  
303은 요청 메소드가 GET으로 변경된다.  

일시적 Redirection의 예시는 다음을 참고하자 [PRG Post/Redirect/Get](https://github.com/320Hwany/TIL/blob/main/spring/PRG.md). 

* 캐시 304 Not Modified

클라이언트가 서버로 GET 메소드로 조회를 요청했을 때 만약 리소스가 변경되지 않았다면  
서버는 클라이언트가 요청한 리소스가 변경되지 않았다고 알려줘 클라이언트의 local pc에서 저장된 캐시를 재사용하도록 한다.   

### 4xx - Client Error

클라이언트에게 오류가 있음을 알려주는 Status Code이다.  

* 400 Bad Request

클라이언트가 잘못된 요청을 해서 서버가 요청을 처리할 수 없는 경우이다.   
쿼리 파라미터가 잘못되었거나 API 스펙에 맞지 않는 경우이다.

* 401 Unauthorized

클라이언트가 해당 리소스에 대한 인증이 필요한 경우이다. 로그인과 같은 인증이 필요한 경우이다.  
> 참고  
> 인증(Authentication) : 본인이 누구인지 확인 (로그인)  
> 인가(Authorization) : 권한 부여 (로그인과 같은 인증 후 관리자 권한이 필요한 경우이다)  
> 위와 같은 경우는 인증이 필요한 경우이기 때문에 Unauthentication이 더 맞는 표현이다  

* 403 Forbidden

서버가 요청을 이해했지만 승인을 거부한 경우이다. 로그인과 같은 인증은 했지만 권한 부여인 인가가 필요하다.   

* 404 Not Found

요청 리소스를 찾을 수 없는 경우이다. 또는 클라이언트가 접근 권한이 없는 경우 정보를 숨기고 싶을 때 사용하기도 한다.  

### 5xx - Server Error

서버 문제로 오류가 발생한 경우이다.  

* 500 Internal Server Error

서버 내부 문제로 오류 발생  

* 503 Service Unavailable

서비스 이용 불가한 경우이며 서버가 일시적으로 과부화 또는 예정된 작업으로 처리할 수 없는 경우이다.   
  
  
서버 차원에서 이러한 Status Code의 예외처리를 해줘 클라이언트에게 보여주는 페이지에 어떠한 오류인지를 잘 표시해주자!   

출처 : 모든 개발자를 위한 HTTP 웹 기본 지식 (김영한)  

