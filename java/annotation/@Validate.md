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



[참고 ](https://www.baeldung.com/javax-validation)

[참고2](https://meetup.toast.com/posts/223)



### JUnit 에서 사용하기

~~~java
public class ValidateTest {
    private static Validator validator;
    
    @BeforeAll 
	public static void init() {
		// validator 세팅
		ValidatorFactory factory = Validation.buildDefaultValidatorFactory();
		validator = factory.getValidator();
	}
    
    @Test
    @DisplayName("validation test")
    @Test
   public void manufacturerIsNull() {
      Car car = new Car( null, "DD-AB-123", 4 );

      Set<ConstraintViolation<Car>> constraintViolations =
      validator.validate( car );

      assertEquals( 1, constraintViolations.size() );
      assertEquals(
         "may not be null",
         constraintViolations.iterator().next().getMessage()
      );
   }
}
~~~

> `Set<ConstraintViolation<Object Type>> constraintViolations = validator.validate(Object Name) `

[Vaidation in JUnit](http://hibernate.org/validator/documentation/getting-started/)



### ExceptionHandler

> Validation을 어길경우 `BindException` `MethodArgumentNotValidException` 발생

- BindException
  `@ModelAttribute` 로 매핑시 발생

- MethodArgumentNotValidException

  `@RequestBody` 로 매핑시 발생

~~~java
@ExceptionHandler(BindException.class)
	@ResponseStatus(value=HttpStatus.OK)
	@ResponseBody
	public Result handleServerException(BindException e) throws Exception  {
		List<ObjectError> list = e.getAllErrors();
		HashMap<String, Object> errors = new HashMap<>(); // <field, message>
		for (ObjectError objectError : list) {
			FieldError field = (FieldError) objectError; // Object의 Field 정보
			String fieldName = field.getField(); 
			
			// field 체크
			if(errors.containsKey(fieldName)) { // errors에 fileName으로 저장된 값이 있다면 continue - 첫번째꺼만 넣어준다
				continue;
			}
			
			
			errors.put(fieldName, objectError.getDefaultMessage());
		}
		
		Result result = new Result(ServerError.ERR_COMMON_DATA_BIND);
		result.put(ResultKey.errmsgvariablemap, errors);
		
		logger.info(CommonUtil.getExceptionMessage(e));
		return result;
	}
~~~

