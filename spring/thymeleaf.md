# thymeleaf

타임리프는 백엔드 서버에서 HTML을 동적으로 렌더링 하는 용도로 사용한다. (서버 사이드 렌더링)   
타임리프는 HTML을 유지하기 때문에 직접 열어도 내용을 확인할 수 있고 뷰 템플릿을 이용해 동적으로 렌더링 한 결과도 확인할 수 있다. (네츄럴 템플릿)  

### text, utext

둘 다 텍스트를 출력한다. model.addAttribute("data", "Hello <b>Spring!</b>"); 로 넘긴 data를 출력한다.
```
<li>th:text 사용 <span th:text="${data}"></span></li>
<li>th:utext 사용 <span th:utext="${data}"></span></li>
``` 
text는 escape를 제공해 태그를 포함한 그대로 출력되고 utext는 unescaped로 Spring! 이 진하게 출력된다.

### with 

선언한 태그 안에서 지역 변수를 선언하여 사용할 수 있다. 
```
<div th:with="first=${users[0]}">
    <p>처음 사람의 이름은 <span th:text="${first.username}"></span></p>
</div>
```

### 기본 객체, 편의 객체 

타임리프는 기본 객체를 제공한다.
```
${#request}, ${#response}, ${#session}, ${#servletContext}, ${#locale}
```
또한 Http 요청 파라미터 접근, Http session 접근, 스프링 빈 접근도 제공한다.
각각 param, session, @로 접근한다.
```
${param.paramData}, ${session.sessionData}, ${@helloBean.hello('Spring!')
```

### URL 링크 
* 단순 URL
```
<li><a th:href="@{/hello}">basic url</a></li>
```
* 쿼리 파라미터  
```
<li><a th:href="@{/hello(param1=${param1}, param2=${param2})}">hello query param</a></li>
```
* 경로 변수   
```
<li><a th:href="@{/hello/{param1}/{param2}(param1=${param1}, param2=${param2})}">path variable</a></li>
```

출처 : 스프링 MVC 2편(인프런 김영한)
