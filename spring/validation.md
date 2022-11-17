# validation  

validation은 HTTP 요청이 정상인 것인지 확인하는 것이다. 이때 오류가 발생할 때 어떤 오류인지를 사용자에게 명확하게 알려줘야 한다.  
검증 로직을 직접 구현할 수도 있지만 상당히 번거롭다. 대부분의 검증 로직은 빈 값인지 특정 크기 범위 내의 값 인지를 확인하는 단순한 로직이다.  
이런 검증 로직을 공통화 표준화 한 것이 Bean validation이다. 또한 스프링과 타임리프가 제공하는 기능을 사용하여 통합하면 훨씬 편하다.    

build.gradle에 다음 의존 관계를 추가하면 라이브러리를 사용할 수 있다. 
```
implementation 'org.springframework.boot:spring-boot-starter-validation'
```

> 참고 : jakarta.validation-api는 Bean-validation 인터페이스이고 hibernate-validator는 구현체이다.

## 자주 사용하는 검증 애노테이션

* @NotBlank : 빈 값 + 공백만 있는 경우를 허용하지 않는다.  
* @NotNull : null 값을 허용하지 않는다.  
* @Range(min = 100, max = 10000) : 범위 안의 값이어야 한다.  
* @Max(10000) : 최대 10000까지만 허용한다.  
* @Size(min = 4, max = 12) : String의 길이가 다음과 같은 크기여야 한다.

## 검증 순서

먼저 회원가입을 위한 Dto MemberSignupDto가 정의되어 있다고 하자

```
@Getter
@Setter
public class MemberSignupDto {

    @Email
    @NotBlank(message = "이메일을 입력해주세요")
    private String email;

    @Size(min = 4, max = 12, message = "최소 4글자 최대 12글자의 아이디만 생성 가능합니다.")
    private String username;

    @Size(min = 4, max = 12, message = "최소 4글자 최대 12글자의 비밀번호만 생성 가능합니다.")
    private String password;
}
```

컨트롤러에서 회원가입을 위한 PostMapping이 있다. 
```
@PostMapping("/signup")
public String join(@Validated @ModelAttribute MemberSignupDto memberSignupDto, BindingResult bindingResult)
```
입력한 email, username, password가 @ModelAttribute를 이용해서 memberSignupDto로 변경이 성공하면 Bean Validation이 적용된다.  
만약 타입이 맞지 않아 memberSignupDto로 변경하는데 실패하면 bindingResult의 typeMismatch FieldError 를 추가한다.  
변경은 되지만 Bean Validation을 적용했을 때 조건에 맞지 않으면 bindingResult에 FieldError를 추가한다.  

bindingResult에 error가 있는지는 다음과 같이 확인한다.
```
bindingResult.hasErrors();
```

## Bean validation의 메세지를 찾는 순서

Bean Validation을 적용했을 때 조건에 맞지 않으면 적절한 메세지를 출력해야 한다. 이때 메세지를 찾는 순서는 다음과 같다.    

1. 생성된 메세지 코드 순서대로 messageSource에서 메세지를 찾는다.  

errors.properties에 다음과 같이 등록하여 사용할 수 있다.
```
NotBlank.memberSignupDto.email=이메일을 입력해주세요
NotBlank.memberSignupDto.username=최소 4글자 최대 12글자의 아이디만 생성 가능합니다
NotBlank.memberSignupDto.email=최소 4글자 최대 12글자의 비밀번호만 생성 가능합니다
```

2. 애노테이션의 message 속성을 사용한다. 
```
@NotBlank(message = "이메일을 입력해주세요")
private String email;  

@Size(min = 4, max = 12, message = "최소 4글자 최대 12글자의 아이디만 생성 가능합니다.")
private String username;

@Size(min = 4, max = 12, message = "최소 4글자 최대 12글자의 비밀번호만 생성 가능합니다.")
private String password;
```

3. 라이브러리가 제공하는 기본 값을 사용한다.
