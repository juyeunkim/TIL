# SpringSecurity

보통 웹 시스템 구성시에는 세션에 인증정보는 직접 저장하고 그 정보를 가지고 권한을 확인하는 형태를 취하고 있다

시큐리티 같은 경우 기본적으로 세션을 사용하고 있지만 `SecurityContext` 객체를 통해서 전달하는 간접적으로 인증 정보를 전달한다.



## 구조

### 인증 architecture

![img](https://t1.daumcdn.net/cfile/tistory/99A7223C5B6B29F003)

> spring security 는 세션-쿠키 방식으로 인증



1. 유저가 로그인을 시도 (http request)
2. AuthenticationFilter 에서부터 user DB까지 타고 들어감
3. DB에 있는 유저라면 UserDetails로 꺼내서 유저의 session 생성
4. spring security의 인메모리 세션저장소인 `SecurityContextHolder`에 저장
5. 유저에게 session ID와 함께 응답을 내려줌
6. 이후 요청에서는 요청쿠키에서 JSESSIONID 를 까서 검증 후 유효하면 Authentication을 쥐어준다



[spring security 파헤치기](https://sjh836.tistory.com/165)

[사용자 인증](https://sungminhong.github.io/spring/security/)



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
>
> 스프링 MVC 웹 보안을 활성화

~~~java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
  ------
}
~~~

- `WebSecurityConfigurer`를 구현하거나
- 컨텍스트의 `WebSecurityConfigurerAdapter` 를 확장한 빈으로 설정 되어있어야한다
  - 확장하여 캘래스를 설정하는 것이 가장 편하고 자주 쓰이는 방법



### WebSecurityConfigurerAdapter

- `configure()` 메서도를 오버라이딩하고 동작을 설정하는것으로 웹 보안을 설정할 수 있다

~~~java
public abstract class WebSecurityConfigurerAdapter implements
		WebSecurityConfigurer<WebSecurity>{
    public void configure(WebSecurity web) throws Exception {...}
    
    protected void configure(HttpSecurity http) throws Exception {...}
    
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {...}
}
~~~

- WebSecurity
  - 스프링 시큐리티의 필터 연결을 설정하기 위한 오버라이딩
- HttpSecurity
  - 인터셉터로 요청을 안전하게 보호하는 방법을 설정하기 위한 오버라이딩
- AuthenticationManagerBuilder
  - 사용자 세부 서비를 설정하기 위한 오버라이딩



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





[@PreAuthorize @Secured 사용법](https://copycoding.tistory.com/278)

[초보자가 이해하는 SpringSecurity]([https://postitforhooney.tistory.com/entry/SpringSecurity-%EC%B4%88%EB%B3%B4%EC%9E%90%EA%B0%80-%EC%9D%B4%ED%95%B4%ED%95%98%EB%8A%94-Spring-Security-%ED%8D%BC%EC%98%B4](https://postitforhooney.tistory.com/entry/SpringSecurity-초보자가-이해하는-Spring-Security-퍼옴))

[[Spring Boot] SpringSecurity 적용하기](https://bamdule.tistory.com/53)





## DelegatingFilterProxy & FilterChainProxy

![img](https://blog.kakaocdn.net/dn/c4Ruyp/btqIBZqOTEb/Q7KNDDJk5rKakxgbAXoRE1/img.png)

- web.xml

~~~xml
<filter-mapping>
		<filter-name>springSecurityFilterChain</filter-name>
		<url-pattern>/*</url-pattern>
</filter-mapping>
<filter>
	<filter-name>springSecurityFilterChain</filter-name>
	<filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
	<async-supported>true</async-supported>
	<init-param>
		<param-name>contextAttribute</param-name>
		<param-value>org.springframework.web.servlet.FrameworkServlet.CONTEXT.appServlet</param-value>
	</init-param>
</filter>
~~~

> DelegatingFilterProxy : 모든 요청은 프록시 필터를 거친다. 스프링 시큐리티는 이를 통해 인증, 인가를 수행



### DelegatingFilterProxy

서블릿 컨테이너와 스프링 컨테이너 사이의 링크를 제공하는 ServletFilter 이다

특정한 이름을 가진 스프링 빈을 찾아 그 빈에게 요청을 위임한다.



[[스프링 시큐리티] DelegatingFilterProxy & FilterChainProxy](https://uchupura.tistory.com/24)







## 익명 사용자 인증처리 필터

### AnonymousAuthenticationFilter

- 익명 사용자 인증처리 필터

- 익명 사용자와 인증 사용자를 구분해서 처리하기 위한 용도로 사용되는 필터

  - `Security Context` 안에 인증객체를 저장 (`ROLE_ANONYMOUS`)

- `isAnonymous()` ` isAuthenticated()` 로 구분

- 인증 객체를 세션에 저장하지 않는다

  >  스프링 시큐리트는 세션에 유저 정보를 저장한다.

  

[[Spring Security] 02. 익명 사용자 인증 처리 필터 : AnonymousAuthenticationFilter](https://coder-in-war.tistory.com/entry/Spring-Security-02-익명-사용자-인증처리-필터-AnonymousAuthenticationFilter)




