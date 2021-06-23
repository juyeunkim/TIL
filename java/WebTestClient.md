# WebTestClient

- 서버 응용 프로그램 테스트를 위해 설계된 HTTP 클라이언트
- End-to-end HTTP테스트를 수행하는데 사용
- Spring MVC 및 Spring WebFlux 애플리케이션을 테스트하는데 사용
- spring 버전 5.3 부터 지원



## 사용방법

1. @Annotation Injection

~~~xml
		<dependency>
		    <groupId>org.springframework.boot</groupId>
		    <artifactId>spring-boot-starter-webflux</artifactId>
		</dependency>
~~~

~~~java
public class Test{
    
    @Autowired
    WebTestClient webTestClient;
    
    @Test
    public String test1(){
        webTestClient.get().uri("/hello").exchange()
                    .expectStatus().isOk()
                    .expectBody(String.class).isEqualTo("hello world");
    }
}
~~~

2. Controller에 바인딩

- WebFlux

~~~java
WebTestClient client =
        WebTestClient.bindToController(new TestController()).build();
~~~

- Spring MVC

~~~java
WebTestClient client =
        MockMvcWebTestClient.bindToController(new TestController()).build();
~~~

3. ApplicationContext 바인딩

- WebFlux

~~~java
@SpringJUnitConfig(WebConfig.class)
class MyTests {

    WebTestClient client;

    @BeforeEach
    void setUp(ApplicationContext context) {
        client = WebTestClient.bindToApplicationContext(context).build(); 
    }
}
~~~

- SpringMVC

~~~java
@ExtendWith(SpringExtension.class)
@WebAppConfiguration("classpath:META-INF/web-resources") 
@ContextHierarchy({
    @ContextConfiguration(classes = RootConfig.class),
    @ContextConfiguration(classes = WebConfig.class)
})
class MyTests {

    @Autowired
    WebApplicationContext wac; 

    WebTestClient client;

    @BeforeEach
    void setUp() {
        client = MockMvcWebTestClient.bindToApplicationContext(this.wac).build(); 
    }
}
~~~





## 사용 예시

~~~java
client.get().uri("/persons/1")
            .accept(MediaType.APPLICATION_JSON)
            .exchange()
            .expectStatus().isOk()
            .expectHeader().contentType(MediaType.APPLICATION_JSON)
~~~



## Context

- `expectBody(Class<T>)`: 단일 객체로 디코딩
- `expectBodyList(Class<T>)`: 객체를 `List<T>`.
- `expectBody()`: [JSON 콘텐츠](https://docs.spring.io/spring-framework/docs/5.3.8/reference/html/testing.html#webtestclient-json) 또는 빈 본문 에 `byte[]`대해 디코딩





[스트링 부트 테스트](https://kkambi.tistory.com/110)

[WebTestClient - 공식 문서](https://docs.spring.io/spring-framework/docs/5.3.8/reference/html/testing.html#webtestclient-controller-config)

[Spring5 WebClient](https://www.baeldung.com/spring-5-webclient)



## WebTestClient Test Class

~~~java
@Autowired
private ApplicationContext context;

protected WebTestClient webTestClient;

@Rule
public final JUnitRestDocumentation restDocumentation = new JUnitRestDocumentation();

@Before
public void setUp() throws Exception {
    this.webTestClient = WebTestClient
            .bindToApplicationContext(context)
            .apply(springSecurity())
            .configureClient()
            .baseUrl("http://localhost:8080/")
            .filter(documentationConfiguration(restDocumentation)
                .snippets()
                .withDefaults(WebTestClientInitializer.prepareSnippets(context),
                           CliDocumentation.curlRequest(),
                           HttpDocumentation.httpRequest(),
                           HttpDocumentation.httpResponse(),
                           AutoDocumentation.requestFields(),
                           AutoDocumentation.responseFields(),
                           AutoDocumentation.pathParameters(),
                           AutoDocumentation.requestParameters(),
                           AutoDocumentation.description(),
                           AutoDocumentation.methodAndPath(),
                           AutoDocumentation.section()))
            .build();
}
~~~





[Spring Auto REST Docs](https://scacap.github.io/spring-auto-restdocs/)

## 사용하면서 발생했던 에러

### org.springframework.beans.factory.NoSuchBeanDefinitionException: No bean named 'webHandler' available

This means that the servlet stack instead if the reactive one is active. Make sure that `@EnableWebFlux` is used.



~~~
org.springframework.beans.factory.support.BeanDefinitionOverrideException: Invalid bean definition with name 'requestMappingHandlerAdapter' defined in class path resource [org/springframework/boot/autoconfigure/web/servlet/WebMvcAutoConfiguration$EnableWebMvcConfiguration.class]: Cannot register bean definition [Root bean: class [null]; scope=; abstract=false; lazyInit=null; autowireMode=3; dependencyCheck=0; autowireCandidate=true; primary=false; factoryBeanName=org.springframework.boot.autoconfigure.web.servlet.WebMvcAutoConfiguration$EnableWebMvcConfiguration; factoryMethodName=requestMappingHandlerAdapter; initMethodName=null; destroyMethodName=(inferred); defined in class path resource [org/springframework/boot/autoconfigure/web/servlet/WebMvcAutoConfiguration$EnableWebMvcConfiguration.class]] for bean 'requestMappingHandlerAdapter': There is already [Root bean: class [null]; scope=; abstract=false; lazyInit=null; autowireMode=3; dependencyCheck=0; autowireCandidate=true; primary=false; factoryBeanName=org.springframework.web.reactive.config.DelegatingWebFluxConfiguration; factoryMethodName=requestMappingHandlerAdapter; initMethodName=null; destroyMethodName=(inferred); defined in class path resource [org/springframework/web/reactive/config/DelegatingWebFluxConfiguration.class]] bound.
~~~



1.RestTestCase에서 ApplicationContext로 WebTestClient 설정
org.springframework.beans.factory.NoSuchBeanDefinitionException: No bean named 'webHandler' available

> https://github.com/spring-projects/spring-framework/issues/22544
> @EnableWebFlux 를 추가하라고 해서 추가했는데 다른 에러 발생

- 에러 내용
  java.lang.IllegalStateException: Failed to load ApplicationContext
  Caused by: org.springframework.beans.factory.support.BeanDefinitionOverrideException: Invalid bean definition with name 'requestMappingHandlerAdapter' defined in class path resource [org/springframework/boot/autoconfigure/web/servlet/WebMvcAutoConfiguration$EnableWebMvcConfiguration.class]: Cannot register bean definition [Root bean: class [null]; scope=; abstract=false; lazyInit=null; autowireMode=3; dependencyCheck=0; autowireCandidate=true; primary=false; factoryBeanName=org.springframework.boot.autoconfigure.web.servlet.WebMvcAutoConfiguration$EnableWebMvcConfiguration; factoryMethodName=requestMappingHandlerAdapter; initMethodName=null; destroyMethodName=(inferred); defined in class path resource [org/springframework/boot/autoconfigure/web/servlet/WebMvcAutoConfiguration$EnableWebMvcConfiguration.class]] for bean 'requestMappingHandlerAdapter': There is already [Root bean: class [null]; scope=; abstract=false; lazyInit=null; autowireMode=3; dependencyCheck=0; autowireCandidate=true; primary=false; factoryBeanName=org.springframework.web.reactive.config.DelegatingWebFluxConfiguration; factoryMethodName=requestMappingHandlerAdapter; initMethodName=null; destroyMethodName=(inferred); defined in class path resource [org/springframework/web/reactive/config/DelegatingWebFluxConfiguration.class]] bound.



2. ApiTest에서 Controller로 설정
   org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'net.nmfun.funbox.controller.GameController$$EnhancerBySpringCGLIB$$b7967e1d': Unsatisfied dependency expressed through field 'request'; nested exception is org.springframework.beans.factory.NoSuchBeanDefinitionException: No qualifying bean of type 'javax.servlet.http.HttpServletRequest' available: expected at least 1 bean which qualifies as autowire candidate. Dependency annotations: {@org.springframework.beans.factory.annotation.Autowired(required=true)}
   Caused by: org.springframework.beans.factory.NoSuchBeanDefinitionException: No qualifying bean of type 'javax.servlet.http.HttpServletRequest' available: expected at least 1 bean which qualifies as autowire candidate. Dependency annotations: {@org.springframework.beans.factory.annotation.Autowired(required=true)}
   at org.springframework.beans.factory.support.DefaultListableBeanFactory.raiseNoMatchingBeanFound(DefaultListableBeanFactory.java:1717)
   at org.springframework.beans.factory.support.DefaultListableBeanFactory.doResolveDependency(DefaultListableBeanFactory.java:1273)
   at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveDependency(DefaultListableBeanFactory.java:1227)
   at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor$AutowiredFieldElement.resolveFieldValue(AutowiredAnnotationBeanPostProcessor.java:657)
   ... 93 more

3. Controller로 테스트
   https://howtodoinjava.com/spring-webflux/webfluxtest-with-webtestclient/ <-- 여기보고 작성
   java.lang.IllegalStateException: Failed to load ApplicationContext
   Caused by: org.springframework.beans.factory.support.BeanDefinitionOverrideException: Invalid bean definition with name 'requestDataValueProcessor' defined in class path resource [org/springframework/security/config/annotation/web/reactive/WebFluxSecurityConfiguration.class]: Cannot register bean definition [Root bean: class [null]; scope=; abstract=false; lazyInit=null; autowireMode=3; dependencyCheck=0; autowireCandidate=true; primary=false; factoryBeanName=org.springframework.security.config.annotation.web.reactive.WebFluxSecurityConfiguration; factoryMethodName=requestDataValueProcessor; initMethodName=null; destroyMethodName=(inferred); defined in class path resource [org/springframework/security/config/annotation/web/reactive/WebFluxSecurityConfiguration.class]] for bean 'requestDataValueProcessor': There is already [Root bean: class [null]; scope=; abstract=false; lazyInit=null; autowireMode=3; dependencyCheck=0; autowireCandidate=true; primary=false; factoryBeanName=org.springframework.security.config.annotation.web.configuration.WebMvcSecurityConfiguration; factoryMethodName=requestDataValueProcessor; initMethodName=null; destroyMethodName=(inferred); defined in class path resource [org/springframework/security/config/annotation/web/configuration/WebMvcSecurityConfiguration.class]] bound.

