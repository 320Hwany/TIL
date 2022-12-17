## HTTP header 란

HTTP 메세지에는 start line, header, empty line, message body 4부분으로 나뉘었는데 이번 글에서는 header에 대해서 알아보겠다.   
HTTP header에는 HTTP 전송에 필요한 모든 부가 정보가 들어있다. message body의 크기, 내용, 압축, 인증, 클라이언트 정보, 서버 정보 ...  
HTTP header는 header field ":" OWS field value 형식으로 작성한다. 예를들면 Content Length: 3423으로 작성할 수 있다.       

## HTTP header 종류  

이러한 header는 4가지로 분류할 수 있다.   
1. General header : 메세지 전체에 적용되는 정보  
2. request header : 요청 정보  
3. response header : 응답 정보
4. representation(표현) header : representation 정보 

### 표현(representation)

참고로 representation(표현)은 표현 헤더와 표현 데이터를 합친 것을 말하며 표현 데이터는 메세지 바디가 전달하는 데이터이고  
표현 헤더는 표현 데이터를 해석할 수 있는 정보를 제공한다. 

Content-Type : 표현 데이터의 형식을 설명한다. text/html; charset=utf-8, application/json, image/png 등이 있다.  
Content-Encoding : 표현 데이터를 인코딩 압축하는 방식을 설명한다. gzip, deflate, identity 등이 있다.   
Content-Language : 표현 데이터의 자연 언어를 나타낸다. ko, en, en-US 등이 있다.  
Content-Length : 표현 데이터의 길이를 나타낸다.   

HTTP 요청, 응답 메세지 모두 message body에 표현 데이터를 넣어서 보낼 수 있다. 따라서 요청, 응답 메세지 모두에 사용가능한 헤더  

### 협상(negotiation)

협상은 클라이언트가 서버에게 어떠한 표현을 원하는지 요청한다. 따라서 HTTP 요청 메세지에만 사용한다.   

Accept : 클라이언트가 선호하는 미디어 타입을 전달  
Accept-Charset : 클라이언트가 선호하는 문자 인코딩  
Accept-Encoding : 클라이언트가 선호하는 압축 인코딩   
Accept-Language : 클라이언트가 선호하는 자연 언어  

이때 클라이언트가 선호하는 표현 형식을 서버에서 지원하지 못할 수도 있다. 그렇기 때문에 클라이언트에서는 선호하는 형식을 여러 개 보낸다.  
우선 순위는 'Quality Values'와 '구체적인 것이 먼저'라는 것을 적용한다.   

1. Quality Values(q)

Accept Language: ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7 라는 예시를 살펴보자.  
q는 0부터 1까지 수로 표현하며 클수록 우선 순위가 높다. 또한 ko-KR에서는 생략된 것을 확인할 수 있는데  
생략하면 1이라는 것을 의미한다.  

2. 구체적인 것이 먼저  

Accept: text/*, text/plain, text/plain;format=flowed,*/*. 
클라이언트가 구체적인 것을 먼저 요청하고 서버에 없으면 다음 우선순위로 요청을 한다.    

### 전송 방식  

전송방식에는 단순, 압축, 분할, 범위 전송이 있다.    
1. 단순 전송은 Content-Length로 데이터의 길이를 표시해서 전송해주는 것이다. (Content-Length: 3423)         
2. 압축 전송은 Content-Encoding에 압축 방식을 표시해서 전송해주는 것이다. (Content-Encoding: gzip)       
3. 분할 전송은 Transfer-Encoding에 chunked로 표시해서 데이터를 나눠서 전달한다. (Transfer-Encoding: chunked)     
이때 전체 데이터의 길이는 알 수 없으므로 Content-Length 필드는 작성해서는 안된다.   
4. 범위 전송은 전체 데이터를 보낼 필요가 없고 특정 범위의 데이터를 보낼 때 사용한다. (Content-Range: bytes 1001-2000/2000)      

### 기타 정보

Referer: 이전 웹 페이지 주소  
User-Agent: 유저 에이전트 애플리케이션 정보  
Server: 요청을 처리하는 origin server의 소프트웨어 정보  
Date: 메세지가 생성된 날짜  
Host: 요청한 domain 정보  
Location: 페이지 Redirection (status code 3xx에서 redirect되는 위치 정보)  

### 쿠키  

Set-Cookie: 응답 메세지 헤더에 있고 서버에서 클라이언트로 쿠키 전달  
Cookie: 클라이언트가 서버에서 받은 쿠키를 저장하고 요청 메세지를 보낼 때 서버로 전달한다.  

Expired, max-age가 있어 쿠키의 생명주기를 보여주며 domain, path도 있어 적용되어야 하는 domain, 경로를 알려준다.  
또한 Secure, HTTPOnly, SameSite라는 보안을 위한 내용도 존재한다.  

쿠키와 세션의 자세한 내용은 다음 링크를 참조하자. [쿠키와 세션](https://github.com/320Hwany/TIL/blob/main/spring/%EC%BF%A0%ED%82%A4%2C%20%EC%84%B8%EC%85%98.md)   

출처 : 모든 개발자를 위한 HTTP 웹 기본지식 (김영한)  
