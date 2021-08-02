# SpringSecurity



[@PreAuthorize @Secured 사용법](https://copycoding.tistory.com/278)

[초보자가 이해하는 SpringSecurity]([https://postitforhooney.tistory.com/entry/SpringSecurity-%EC%B4%88%EB%B3%B4%EC%9E%90%EA%B0%80-%EC%9D%B4%ED%95%B4%ED%95%98%EB%8A%94-Spring-Security-%ED%8D%BC%EC%98%B4](https://postitforhooney.tistory.com/entry/SpringSecurity-초보자가-이해하는-Spring-Security-퍼옴))

[[Spring Boot] SpringSecurity 적용하기](https://bamdule.tistory.com/53)



## 접근 권한 주기

## 세팅

1. `@EnableGlobalMethodSecurity(prePostEnabled = true)`

2. 경로에 따른 권한 체크하기

   `.antMatchers("/api/**").hasRole("USER")`

- 전체 코드

~~~java
@Configuration
@EnableWebSecurity
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class SecurityConfiguration extends WebSecurityConfigurerAdapter {
	
	@Autowired 
	private MyUserDetailsService myUserDetailsService;

	@Autowired
	private ApplicationProperties applicationProperties;

	@Override
	protected void configure(AuthenticationManagerBuilder auth) throws Exception {
		auth.authenticationProvider(authProvider());
		auth.authenticationEventPublisher(new DefaultAuthenticationEventPublisher());
	}

	@Override
	protected void configure(HttpSecurity http) throws Exception {
		var registry = http.authorizeRequests();

		// 어플리케이션 별 화이트리스트
		Optional.ofNullable(applicationProperties.getSecurity()).map(SecurityInfo::getWhitelist)
				.ifPresent(whitelist -> Arrays.stream(whitelist).forEach(e -> registry.antMatchers(e).permitAll()));

		registry.antMatchers("/public/**").permitAll()
				.antMatchers("/api/**").hasRole("USER") // USER만 접근할수있는 경로 설정 - Controller에 @PreAuthorize("hasRole('ROLE_USER')") 붙여야함
				.anyRequest().authenticated()
		;
	}


	@Override
	public void configure(WebSecurity web) throws Exception {
		web
			.ignoring()
			.antMatchers("/static/**", "/webview/**");
	}
	
	@Bean
	public DaoAuthenticationProvider authProvider() throws Exception {
		DaoAuthenticationProvider authProvider = new DaoAuthenticationProvider();
		authProvider.setUserDetailsService(myUserDetailsService);
		authProvider.setPasswordEncoder(passwordEncoder());
		return authProvider;
	}
	
	@Bean
	public PasswordEncoder passwordEncoder() throws Exception {
		return new BCryptPasswordEncoder();
	}

	@Bean
	@Override
	public AuthenticationManager authenticationManagerBean() throws Exception {
		// TODO Auto-generated method stub
		return super.authenticationManagerBean();
	}	
}
~~~



### Controller

`@PreAuthorize` 를 사용해서 사용하고자 하는 클래스 or 메소드 앞에 사용한다

- 클래스에 사용

  ~~~java
  @RestController
  @PreAuthorize("hasRole('ROLE_USER')")
  public class TestController{
      ////
  }
  ~~~

- 메소드에 사용

  ~~~java
  @RestController
  public class TestController{
      
      @PreAuthorize("hasRole('ROLE_USER')")
  	@RequestMapping(value="/api/users/games/{gameId}", method=RequestMethod.POST)
  	public Result insUserGame(@PathVariable(name = "gameId", required = true) Integer gameId
  				, @RequestBody UserGame userGame) throws Exception {
  		return gameService.insUserGame(gameId, userGame, super.getUser());
  	}
  }
  ~~~

  



## @EnableWebSecurity

> for Spring Security

~~~java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
  ------
}
~~~



## @EnableGlobalMethodSecurity

> for Spring Security

~~~java
@Configuration
@EnableWebSecurity
@EnableGlobalMethodSecurity(securedEnabled=true, prePostEnabled=true)
public class SecurityConfig extends WebSecurityConfigurerAdapter {
  ------
}
~~~

- 웹 어플리케이션 method security 담당
- `@PreAuthorize`, `@PostAuthorize` , `@Secured`  어노테이션 사용 가능



### @PreAuthorize

요청이 들어와 함수를 실행하기 전에 권한을 검사하는 어노테이션

- 설정

~~~java
@Configuration
@EnableGlobalMethodSecurity(securedEnabled = true, prePostEnabled = true)
public class ClientMethodSecurity extends GlobalMethodSecurityConfiguration {

}
~~~



- 예제

~~~java
@PreAuthorize("hasRole('ROLE_USER')")
 public void create(Contact contact)
~~~

~~~java
@PreAuthorize("hasPermission(#contact, 'admin')")
  public void deletePermission(Contact contact, Sid recipient, Permission permission);
~~~

> 설정이 추가로 필요하다
>
> 1. `PermissionEvalutor`을 구현하는 클래스 - `ClientPermissionExpression`
>
> ~~~java
> public class ClientPermissionExpression implements PermissionEvaluator {
> 	// hasPermission() 을 PermissionEvaluator 에 위임한다
>     
>     @Override
>     public boolean hasPermission(Authentication authentication, Object targetDomainObject, Object permission) {
>         return false;
>     }
> 
>     @Override
>     public boolean hasPermission(Authentication authentication, Serializable targetId, String targetType, Object permission) {
>         return false;
>     }
> }
> ~~~
>
> 2. 1번에서 구현한 클래스를 GlobalMethodSecurity 에 연결
>
> ~~~java
> @Configuration
> @EnableGlobalMethodSecurity(securedEnabled = true, prePostEnabled = true)
> public class ClientMethodSecurity extends GlobalMethodSecurityConfiguration {
> 
>     @Override
>     protected MethodSecurityExpressionHandler createExpressionHandler() {
>         DefaultMethodSecurityExpressionHandler expressionHandler =
>                 new DefaultMethodSecurityExpressionHandler();
>         expressionHandler.setPermissionEvaluator(new ClientPermissionExpression());
>         return expressionHandler;
>     }
> }
> ~~~

~~~java
@PreAuthorize("#contact.name == authentication.name")
  public void doSomething(Contact contact);
~~~

> security context에 저장된 `Authentication`을 이용하는 built-in express
>
> `principal` , `UserDetails` 에서도 직접적으로 접근 가능



[Spring Boot Security에서 hasPermission 사용](https://hodolman.com/19)

### @PostAuthorize

함수를 실행하고 클라이언트한테 응답을 하기 직전에 권한을 검사하는 어노테이션

### @Secured

~~~java
@Secured("ROLE_USER")
public void create(Contact contact)
~~~



### @PreAuthorize vs @Secured

- @PreAuthorize 
  @Secured보다 최신 기능, 더 강력함
- @PreAuthorize("hasRole('ROLE_USER')") == @Secured("ROLE_USER")

|                 | @PreAuthorize   | @Secured |
| --------------- | --------------- | -------- |
| expression      | O               | X        |
| OR, AND, NOT(!) | OR, AND, NOT(!) | OR       |

~~~java
@PreAuthorize("hasRole('ROLE_ROLE1') and hasRole('ROLE_ROLE2')") // ROLE1 and ROLE2
~~~


~~~java
@Secured({"ROLE1", "ROLE2"}) // ROLE1 or ROLE2
~~~





[@EnableGlobalMethodSecurity](https://docs.spring.io/spring-security/site/docs/3.0.x/reference/el-access.html#el-common-built-in)

## @EnableAuthorizationServer

> for OAuth 2 Security

~~~java
@SpringBootApplication
@RestController
@EnableOAuth2Client
@EnableAuthorizationServer
public class SocialApplication extends WebSecurityConfigurerAdapter {

------
}
~~~

- OAuth 2.0 Authorization 서버 매커니즘 config
- Authorization Server를 만들고 액세스 토큰을 부여 가능



## @EnableResourceServer

> for OAuth 2 Security

~~~java
@Configuration
@EnableResourceServer
protected static class ResourceServerConfiguration
    extends ResourceServerConfigurerAdapter {
  ------
}
~~~

- 액세스 토큰을 사용하려면 리소스 서버(인증 서버) 필요한데 서버를 제공한다 
- security filter를 만든다



[Spring Security 공식 문서](https://docs.spring.io/spring-boot/docs/1.4.x/reference/html/boot-features-security.html)

