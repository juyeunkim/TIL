# @Validate

> 스프링 5 버전에서 validate 체크해주는 기능



### pom.xml

~~~xml
<!-- validation-api는 생략 가능 -->
<dependency>
    <groupId>javax.validation</groupId>
    <artifactId>validation-api</artifactId>
    <version>2.0.1.Final</version>
</dependency>
<dependency>
    <groupId>org.hibernate.validator</groupId>
    <artifactId>hibernate-validator</artifactId>
    <version>6.0.7.Final</version>
</dependency>
~~~

> hibernate > validation가 포함되어있어서
>
> hibernate 사용하면 validate 생략 가능



### config

~~~java
@Configuration
@EnableWebMvc
public class SpringConfig implements WebMvcConfigurer {
    ...
}
~~~

- Bean Validation 어노테이션에 대한 검증 기능을 제공하는 OptionalValidatorFactoryBean를 글로벌 Validator로 등록



### VO

~~~java
public class FormData {
    @NotBlank
    @Email
    private String email;

    @NotBlank
    private String name;

    @NotEmpty
    private String password;

    @DateTimeFormat(pattern = "yyyyMMdd")
    @PastOrPresent
    private LocalDate birthday;
~~~



### Controller

~~~java
@PostMapping
public String submit(@ModelAttribute @Valid FormData formData, Errors errors) {
    if (errors.hasErrors()) return "form";
    return "submit";
}
~~~



## Validation Annotations

- @NotNull
- @AssertTrue
- @Size
- @Min
- @Max
- @Email
- @NotEmpty
- @NotBlank
- @Positive @PositiveOrZero
- @Negative @NegativeOrZero
- @Past @PastorPresent
- @Future @FutureOrPresent



### NotNull vs NotEmpty vs NotBlank

> NotNull < NotEmpty < NotBlank

- NotNull : `null`만 안됨
- NotEmpty : `null` `""` 안됨
- NotBlank : `null` `""` `" "` 안됨 



[참고](https://www.baeldung.com/javax-validation)

