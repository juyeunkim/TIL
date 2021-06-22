# WebTestClient

- 서버 응용 프로그램 테스트를 위해 설계된 HTTP 클라이언트
- End-to-end HTTP테스트를 수행하는데 사용
- Spring MVC 및 Spring WebFlux 애플리케이션을 테스트하는데 사용



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

