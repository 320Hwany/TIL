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

1,2번 방법은 모두 쿼리 파라미터 형식으로 전달되기 때문에 request.getParameter()를 사용할 수 있다.


3. HTTP message body에 데이터를 직접 담아서 요청

* API 메세지 바디 - 단순 텍스트 : 단순 텍스트를 HTTP 메세지 바디에 담아서 전송하는 방법이다.

```
ServletInputStream inputStream = request.getInputStream(); //message body 의 내용을 byte 코드로 바로 얻을 수 있다
String messageBody = StreamUtils.copyToString(inputStream, StandardCharsets.UTF_8); // byte -> string
```

* API 메세지 바디 - Json : 데이터 형식은 주로 Json을 사용하고 HTTP API에서 주로 사용한다. 

위와 같이 request.getInputStream(), StreamUtils.copToString()을 사용하여 message body의 내용을 바이트 코드로 바꾸고  
바이트 코드를 string으로 바꾸는 방식은 같다. 
```
private ObjectMapper objectMapper = new ObjectMapper();
```
Spring boot에서 ObjectMapper라는 Jackson 라이브러리를 제공한다. 

HelloData라는 객체가 있다고 해보자
```
@Getter @Setter
public class HelloData {

    private String username;
    private int age;
}
```
```
HelloData helloData = objectMapper.readValue(messageBody, HelloData.class);
```
이렇게 사용하여 string 형식의 message body를 Json 방식으로 파싱하여 helloData 객체를 반환한다. 

## HTTP 응답 데이터 

> HTTP 응답 메시지도 3가지 방법으로 전달한다.  

1. 단순 텍스트 응답  
2. HTML 응답  
3. HTTP API - MessageBody Json 응답

### HTTP 요청 파라미터

스프링이 제공하는 @RequestParam을 사용하면 요청 파라미터를 편리하게 사용할 수 있다.

```
    @ResponseBody
    @RequestMapping("/request-param-v2")
    public String requestParamV2(
            @RequestParam("username") String memberName,
            @RequestParam("age") int memberAge) {
        log.info("username={}, age={}", memberName, memberAge);
        return "ok";
    }
```

request-param-v2?username=kim&age=20

위와 같은 url로 들어오면 @RequestParam으로 받아서 처리할 수 있다.  
이때 return "ok"; 는 ok라는 뷰를 찾아서 반환하는 것이 아니라  
@ResponsBody가 있기 때문에 HTTP message body에 직접 해당 내용을 입력한다.

다음으로 username, age를 가지고 있는 객체 HelloData가 있다고 하자.  

```
  @ResponseBody
  @RequestMapping("/model-attribute-v1")
  public String modelAttributeV1(@ModelAttribute HelloData helloData) {
      log.info("username={}, age={}", helloData.getUsername(),  helloData.getAge());
      return "ok";
  }
```
  
이렇게 @ModelAttribute를 사용하면 놀랍게도... username, age를 알아서 넣어준다!  
단 이때 HelloData 객체는 @Setter가 있어야 한다.  


@ModelAttribute는 이렇게 요청 파라미터를 처리하는 기능과 또 한가지 기능이 있다.
```
@PostMapping("/add")
public String addItemV2(@ModelAttribute("item") Item item) {

    itemRepository.save(item);

    return "basic/item";
}
```
```
model.addAttribute("item", item);
```
이렇게 model을 넣어주지 않아도 모델이 지정한 객체를 자동으로 넣어준다..!

> @ModelAttribute의 기능 2가지

1. 요청 파라미터 처리
2. Model 추가


