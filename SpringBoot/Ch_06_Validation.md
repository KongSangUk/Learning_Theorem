# Validation

올바르지 않은 데이터를 걸러내고 보안을 유지하기 위해 데이터 검증(validation)은 여러 계층에 걸쳐서 적용됩니다.
Client의 데이터는 조작이 쉬울 뿐더러 모든 데이터가 정상적인 방식으로 들어오는 것도 아니기 때문에, `Client Side`뿐만 아니라 `Server Side`에서도 데이터 유효성을 검사해야 할 필요가 있습니다.

스프링부트 프로젝트에서는 `@validated`를 이용해 유효성을 검증할 수 있습니다.



## Bean Validation

스프링의 기본적인 `validation`인 `Bean validation`은 클래스 "필드"에 특정 annotation을 적용하여 필드가 갖는 제약 조건을 정의하는 구조로 이루어진 검사입니다.

validator가 어떠한 비즈니스적 로직에 대한 검증이 아닌, 그 클래스로 생성된 객체 자체의 필드에 대한 유효성 여부를 검증합니다.



> **❗️ @Valid, @Validated 차이**
> `@Valid`는 Java 에서 지원해주는 어노테이션이고 `@Validated`는 Spring에서 지원해주는 어노테이션입니다. `@Validated`는 `@Valid`의 기능을 포함하고, 유효성을 검토할 그룹을 지정할 수 있는 기능을 추가로 가지고 있습니다.



## Spring Boot Validation 적용하기

### 1. `spring boot 2.3 version` 이상부터는 `spring-boot-starter-web` 의존성 내부에 있던 `validation`이 사라졌기 때문에, 의존성을 따로 추가

**- Gradle 의존성 추가**

```java
implementation group: 'org.springframework.boot', name: 'spring-boot-starter-validation', version: '2.5.6'
```



**- Maven 의존성 추가**

```java
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
    <version>2.5.6</version>
</dependency>
```



### 2. `Contoller`에서 유효성 검사를 적용할 API의 Request객체 앞에 `@validated` 어노테이션을 추가

**userController.java**

```java
    @PostMapping
    public ResponseEntity<?> createUSer(@Validated @RequestBody final UserCreateRequestDto userCreateRequestDto, BindingResult bindingResult){
        if (bindingResult.hasErrors()) {
            List<String> errors = bindingResult.getAllErrors().stream().map(e -> e.getDefaultMessage()).collect(Collectors.toList());
            // 200 response with 404 status code
            return ResponseEntity.ok(new ErrorResponse("404", "Validation failure", errors));
            // or 404 request
            //  return ResponseEntity.badRequest().body(new ErrorResponse("404", "Validation failure", errors));
        }
        try {
            final User user = userService.searchUser(userCreateRequestDto.toEntity().getId());
        }catch (Exception e){
            return ResponseEntity.ok(
                    new UserResponseDto(userService.createUser(userCreateRequestDto.toEntity()))
            );
        }
        // user already exist
        return ResponseEntity.ok(
                new UserResponseDto(userService.searchUser(userCreateRequestDto.toEntity().getId()))
        );

    }
```



**ErrorResponse.java**

```java
package com.springboot.server.common;
import lombok.*;
import java.util.ArrayList;
import java.util.List;

@Getter
@Setter
public class ErrorResponse {

    private String statusCode;
    private String errorContent;
    private List<String> messages;

    public ErrorResponse(String statusCode, String errorContent, String messages) {
        this.statusCode = statusCode;
        this.errorContent = errorContent;
        this.messages = new ArrayList<>();
        this.messages.add(messages);
    }

    public ErrorResponse(String statusCode, String errorContent, List<String> messages) {
        this.statusCode = statusCode;
        this.errorContent = errorContent;
        this.messages = messages;
    }
}
```

`@Validated`로 검증한 객체가 유효하지 않은 객체라면 `Controller`의 메서드의 파라미터로 있는 `BindingResult` 인터페이스를 확장한 객체로 들어옵니다.

때문에 `bindingResult.hasError()` 메서드는 유효성 검사에 실패했을 때 true를 반환합니다.



### 3. Request를 핸들링할 객체를 정의할 때 Validation 어노테이션을 통해 필요한 유효성 검사를 적용합니다.

```java
import lombok.*;
import javax.validation.constraints.Email;
import javax.validation.constraints.NotBlank;

@Getter
@Builder
@NoArgsConstructor
public class UserCreateRequestDto {

    @NotBlank(message="NAME_IS_MANDATORY")
    private String name;
    @NotBlank(message="PASSWORD_IS_MANDATORY")
    private String password;
    @Email(message = "NOT_VALID_EMAIL")
    private String email;

    public User toEntity(){
        return User.builder()
                .user_name(name)
                .email(email)
                .password(password)
                .build();
    }
}
```

유효성 검사에 적용할 수 있는 어노테이션은 다음과 같습니다.

```java
@Null  // null만 혀용합니다.
@NotNull  // null을 허용하지 않습니다. "", " "는 허용합니다.
@NotEmpty  // null, ""을 허용하지 않습니다. " "는 허용합니다.
@NotBlank  // null, "", " " 모두 허용하지 않습니다.

@Email  // 이메일 형식을 검사합니다. 다만 ""의 경우를 통과 시킵니다
@Pattern(regexp = )  // 정규식을 검사할 때 사용됩니다.
@Size(min=, max=)  // 길이를 제한할 때 사용됩니다.

@Max(value = )  // value 이하의 값을 받을 때 사용됩니다.
@Min(value = )  // value 이상의 값을 받을 때 사용됩니다.

@Positive  // 값을 양수로 제한합니다.
@PositiveOrZero  // 값을 양수와 0만 가능하도록 제한합니다.

@Negative  // 값을 음수로 제한합니다.
@NegativeOrZero  // 값을 음수와 0만 가능하도록 제한합니다.

@Future  // 현재보다 미래
@Past  // 현재보다 과거

@AssertFalse  // false 여부, null은 체크하지 않습니다.
@AssertTrue  // true 여부, null은 체크하지 않습니다.
```

해당 기능들에 대한 더 세부적인 내용은 [이 사이트](https://docs.jboss.org/hibernate/validator/6.2/reference/en-US/html_single/#section-builtin-constraints)를 참고.



## Exception Handling

위에 적용한 것 처럼 에러를 처리하는 객체를 따로 생성해 가공하는 것 외에도,
`@ExceptionHandler` 어노테이션을 이용해 예외를 처리할 수 있습니다.



**example1**

```java
@ResponseStatus(HttpStatus.BAD_REQUEST)
@ExceptionHandler(ConstraintViolationException.class)
public Object exception(Exception e) {
	return e.getMessage(); 
}
```



**example2**

```java
@ExceptionHandler(MethodArgumentNotValidException.class)
protected ResponseEntity<ErrorResponse> handleMethodArgumentNotValidException(MethodArgumentNotValidException e) {
    log.error("MethodArgumentNotValidException : " + e.getMessage());
    final ErrorResponse response = ErrorResponse.of(ErrorCode.INVALID_INPUT_VALUE, e.getBindingResult());
    return new ResponseEntity<>(response, HttpStatus.BAD_REQUEST);
}    
```



## Test Code 작성

```java
import com.springboot.server.controller.user.UserCreateRequestDto;
import org.junit.jupiter.api.*;
import javax.validation.ConstraintViolation;
import javax.validation.Validation;
import javax.validation.Validator;
import javax.validation.ValidatorFactory;
import java.util.Set;

import static org.assertj.core.api.Assertions.assertThat;


public class UserValidationTest {
    private static ValidatorFactory factory;
    private static Validator validator;

    @BeforeAll
    public static void init() {
        factory = Validation.buildDefaultValidatorFactory();
        validator = factory.getValidator();
    }

    @AfterAll
    public static void close() {
        factory.close();
    }

    @DisplayName("빈문자열 전송 시 에러 발생")
    @Test
    void blank_validation_test() {
        // given
        UserCreateRequestDto userCreateRequestDto = new UserCreateRequestDto("", "test","email@naver.com");
        // when
        Set<ConstraintViolation<UserCreateRequestDto>> violations = validator.validate(userCreateRequestDto); // 유효하지 않은 경우 violations 값을 가지고 있다.
        // then
        assertThat(violations).isNotEmpty();
        violations
                .forEach(error -> {
                    assertThat(error.getMessage()).isEqualTo("NAME_IS_MANDATORY");
                });
    }

    @DisplayName("이메일 형식 아닌 경우 에러 발생")
    @Test
    void email_validation_test() {
        // given
        UserCreateRequestDto userCreateRequestDto = new UserCreateRequestDto("name", "test","email");
        // when
        Set<ConstraintViolation<UserCreateRequestDto>> violations = validator.validate(userCreateRequestDto);
        // then
        assertThat(violations).isNotEmpty();
        violations
                .forEach(error -> {
                    assertThat(error.getMessage()).isEqualTo("NOT_VALID_EMAIL");
                });
    }

    @DisplayName("유효성 검사 성공")
    @Test
    void validation_success_test() {
        // given
        UserCreateRequestDto userCreateRequestDto = new UserCreateRequestDto("name", "test","email@naver.com");
        // when
        Set<ConstraintViolation<UserCreateRequestDto>> violations = validator.validate(userCreateRequestDto);
        // then
        assertThat(violations).isEmpty(); // 유효한 경우
    }
}
```

`hamcrest.assertThat`을 사용하면 `violations`를 찾지 못해서 `assertj.assertThat`으로 처리하긴 했는데...이유는 더 찾아봐야겠다. ->`hamcrest`는 `violationMatchers`로 따로 구현돼있는 것 같다.



## Reference

[Valid](https://jeong-pro.tistory.com/203)
[Valid2](https://wildeveloperetrain.tistory.com/25)
[Valid3](https://jaimemin.tistory.com/1884)
[Valid4](https://velog.io/@ayoung0073/springboot-annotation-valid)
[bindingresult casting error](https://stackoverflow.com/questions/54536895/cannot-be-cast-to-class-because-they-are-in-unnamed-module-of-loader-app)
[return validation error binding result with response entity](https://stackoverflow.com/questions/50006227/how-do-you-return-an-error-response-from-a-binding-result-validation-failure/50027787)
[spring validation](https://wildeveloperetrain.tistory.com/25)
[hamcrest violation matchers](https://github.com/testinfected/hamcrest-matchers/blob/master/validation-matchers/src/main/java/org/testinfected/hamcrest/validation/ViolationMatchers.java)