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

  

