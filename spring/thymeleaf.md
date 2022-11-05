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

### 리터럴 

리터럴은 소스 코드 상에서 고정된 값을 말한다. 타임리프에서 문자 리터럴은 작은 따옴표로 감싸서 나타낸다.   
공백없이 이어진다면 생략할 수 있지만 공백이 있다면 무조건 감싸야한다.
```
<li>'hello world!' = <span th:text="'hello world!'"></span></li>
```
리터럴 대체 문법을 사용할 수도 있다.
```
<li>리터럴 대체 |hello ${data}| = <span th:text="|hello ${data}|"></span></li>
```

### 연산 

산술 연산, 비교 연산을 할 수 있다.  
```
<li>10 + 2 = <span th:text="10 + 2"></span></li>
<li>1 gt 10 = <span th:text="1 gt 10"></span></li>
```
또한 Elvis 연산자로 데이터가 있으면 ${nullData}를 없으면 뒤에 '데이터가 없습니다'를 출력한다 
```
<li>${nullData}?: '데이터가 없습니다.' = <span th:text="${nullData}?:'데이터가 없습니다.'"></span></li> </ul>
```
no_operation 연산은 ${data}가 없으면 타임리프가 실행되지 않고 HTML을 그대로 사용한다.
```
<li>${data}?: _ = <span th:text="${data}?: _">데이터가 없습니다.</span></li>
```

### 속성 설정

th:* 로 속성을 설정하면 기존에 있던 속성을 대체한다. 다음 예시에서 서버에서 렌더링하면 name = userA가 된다.
```
<input type="text" name="mock" th:name="userA" />
```
또한 th:checked가 false 이면 checked 속성 자체를 제거한다.
```
- checked o <input type="checkbox" name="active" th:checked="true" /><br/>
- checked x <input type="checkbox" name="active" th:checked="false" /><br/>
```

### 반복
th:each로 ${users} 컬렉션에서 하나씩 꺼내올 수 있다.
```
<tr th:each="user : ${users}">
        <td th:text="${user.username}">username</td>
        <td th:text="${user.age}">0</td>
    </tr>
```
또한 userStat으로 index, count, size, even, odd, first, last, current 등 정보를 가져올 수 있다.  
지정한 변수명 + Stat일 경우에는 파라미터를 생략할 수 있다.  

### 조건부 평가
th:if, th:unless를 사용하여 조건에 맞으면 출력하고 조건에 맞지 않으면 출력하지 않는다.  

```
<span th:text="'미성년자'" th:if="${user.age lt 20}"></span>
<span th:text="'미성년자'" th:unless="${user.age ge 20}"></span>
```   
switch case를 사용할 수도 있다.
```
<td th:switch="${user.age}">
    <span th:case="10">10살</span>
    <span th:case="20">20살</span>
    <span th:case="*">기타</span>
</td>
```

### 주석

표준 HTML 주석, 타임리프 파서 주석, 타임리프 프로토타입 주석 3가지가 있지만 거의 대부분 타임리프 주석만 사용한다.  
```
<!--/* [[${data}]] */-->

<!--/*-->
<span th:text="${data}">html data</span>
<!--*/-->
```

### 블록

th:block은 HTML 태그가 아닌 타임리프의 유일한 자체 태그이다.  
다음과 같이 두 div에서 반복되어 출력해야 할 경우 사용한다.
```
<th:block th:each="user : ${users}">
    <div>
        사용자 이름1 <span th:text="${user.username}"></span>
        사용자 나이1 <span th:text="${user.age}"></span> </div>
    <div>
        요약 <span th:text="${user.username} + ' / ' + ${user.age}"></span>
    </div>
</th:block>
```

### 자바스크립트 인라인 

타임리프는 자바스크립트에서 타임리프를 편리하게 사용할 수 있는 인라인 기능을 제공한다.  
텍스트 렌더링, 내츄럴 템플릿 등을 지원한다.
```
<script th:inline="javascript">
```

### 템플릿 조각, 템플릿 레이아웃

상단, 하단, 좌우측 카테고리 영역과 같이 여러 페이지에서 함께 사용하는 영역은 템플릿 조각을 사용해서 처리한다.   
먼저 공통으로 처리할 내용을 th:fragment로 만든다. 
```
<footer th:fragment="copy"> 푸터 자리 입니다.
```

사용할 페이지에 insert, replace로 그 부분을 넣을 수 있다. insert는 추가하는 것이고 replace는 대체하는 것이다.   
파라미터를 전달해서 동적으로 렌더링 할 수도 있다.
```
<div th:insert="~{template/fragment/footer :: copy}"></div>
<div th:replace="~{template/fragment/footer :: copy}"></div>
<div th:replace="~{template/fragment/footer :: copyParam ('데이터1', '데이터 2')}"></div>
```
일부 코드 조각을 레이 아웃에 넘겨 사용할 수도 있다.


출처 : 스프링 MVC 2편(인프런 김영한)
