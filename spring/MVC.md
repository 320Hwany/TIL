## MVC 패턴 

서블릿, JSP로만 구현을 했을 때 JSP가 너무 많은 역할을 가진다는 단점이 있었다. 이를 MVC패턴으로 역할을 나눌 수 있다.   
Model : 데이터의 형식을 지정하고 저장하고 불러오는 작업  
View : 눈에 보이는 것 웹의 경우 html, css로 나타내는 요소   
Controller : Model의 데이터를 View에 연결해서 전반적인 제어를 하는 것

여기서 Controller는 서블릿이 담당하고 View는 JSP가 담당한다. Controller에서 Model로 데이터를 전달하고 View에서 Model로  
데이터를 참조한다. 또한 컨트롤러에 비즈니스 로직을 둘 수도 있지만 이렇게하면 컨트롤러가 너무 많은 역할을 담당하기 때문에   
서비스라는 계층을 만들어 비즈니스 로직을 담당하게 한다.

하지만 이러한 MVC 패턴의 컨트롤러에는 중복이 너무 많다.

```
 RequestDispatcher dispatcher = request.getRequestDispatcher(viewPath);// Controller 에서 view 로 이동할 때 사용
 dispatcher.forward(request, response);
```
Controller에서 View로 이동하게 해주는 dispathcher와
```
String viewPath = "/WEB-INF/views/new-form.jsp";
```
ViewPath가 중복된다.  
 
또한 이러한 설계는 공통처리가 어렵다. 이를 해결하기 위해서 컨트롤러 호출 전에 공통 기능을 처리하는 프론트 컨트롤러를 도입한다.

[프론트 컨트롤러](https://github.com/yhwjd/TIL/blob/main/spring/%ED%94%84%EB%A1%A0%ED%8A%B8%EC%BB%A8%ED%8A%B8%EB%A1%A4%EB%9F%AC.md)

## MVC 웹 프레임워크

정적웹이 아닌 동적웹에서 사용된다.

> 잠깐! 라이브러리 vs 프레임워크

라이브러리가 각각의 개별적인 기능들이라고 한다면 프레임워크는 이것들이 연결되어서 기초적인 골격을 갖춘 상태  
가져다 쓰는 것이 라이브러리 기본 틀로 삼아서 위에 덧붙여 만드는게 프레임워크 

### HTTP 요청 파라미터

1. @RequestParam    

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

required를 사용하여 이 파라미터가 필수인지 아닌지를 정할 수 있다. required = true가 기본 값이다.
```
@RequestParam(required = false) Integer age
```

defaultValue를 사용하여 데이터가 들어오지 않았을 때 기본 값으로 "guest"로 설정한다.
```
@RequestParam(defaultValue = "guest") String username
```

파라미터를 Map으로 조회할 수도 있다. 모두 paramMap안에 넣고 get으로 조회할 수 있다. 또한 하나의 key 값에 여러 개의 value가 있다면  
MultiValueMap을 사용할 수도 있다.
```
    @ResponseBody 
    @RequestMapping("/request-param-map")
    public String requestParamMap(
            @RequestParam Map<String, Object> paramMap) {
        log.info("username={}, age={}", paramMap.get("username"), paramMap.get("age"));
        return "ok";
    }
```

2. @ModelAttribute  

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

### HTTP message body

1. 단순 텍스트  

앞에서 설명한 요청 파라미터가 아닌 HTTP message body를 가져올 때는 @RequestBody를 사용한다.
```
    @ResponseBody
    @PostMapping("/request-body-string-v4")
    public String requestBodyStringV4(@RequestBody String messageBody) {
        log.info("messageBody={}", messageBody);
        return "ok";
    }
```
@RequestBody가 HTTP message body를 가져와서 String 형태의 messageBody에 넣어준다.  
이때 return "ok"; 는 ok라는 뷰를 찾아서 반환하는 것이 아니라  
@ResponsBody가 있기 때문에 HTTP message body에 직접 해당 내용을 입력한다.

2. JSON

@RequestBody를 사용하면 객체 또한 넣어줄 수 있다.  
JSON 요청 -> HTTP 메세지 컨버터 -> 객체
```
    @ResponseBody
    @PostMapping("/request-body-json-v3")
    public String requestBodyJsonV3(@RequestBody HelloData data) {
        log.info("username={}, age={}", data.getUsername(), data.getAge());
        return "ok";
    }
```

@ResponseBody로 객체를 반환할 수 있다.  
객체 -> HTTP 메세지 컨버터 -> JSON 응답
```
    @ResponseBody
    @PostMapping("/request-body-json-v5")
    public HelloData requestBodyJsonV5(@RequestBody HelloData data) {
        log.info("username={}, age={}", data.getUsername(), data.getAge());
        return data;
    }
```
### HTTP 응답 

1. 정적 리소스 : 웹 브라우저에 정적인 HTML, css, js를 제공할 때는 정적 리소스를 사용한다  
2. 뷰 템플릿 : 웹 브라우저에 동적인 HTML을 제공할 때는 뷰 템플릿을 사용한다   
return 으로 뷰의 논리 이름을 반환한다.
```
    @RequestMapping("/response-view-v2")
    public String responseViewV2(Model model) {
        model.addAttribute("data", "hello!");
         return "response/hello";
    }
```
3. HTTP API 제공 : HTML을 전달하는 것이 아니고 데이터를 HTTP message body에 JSON 같은 형식으로 보낸다  

```
   @ResponseStatus(HttpStatus.OK)
    @ResponseBody
    @GetMapping("/response-body-json-v2")
    public HelloData responseBodyJsonV2() {
        HelloData helloData = new HelloData();
        helloData.setUsername("userA");
        helloData.setAge(20);
        return helloData;
    }
```
@Controller와 @ResponseBody를 합쳐 @RestController를 사용할 수도 있다.
