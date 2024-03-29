# WebFlux



[springwebflux](https://godekdls.github.io/Reactive Spring/springwebflux)

[스프링 리액티브]([https://kouzie.github.io/spring/Spring-Boot-%EC%8A%A4%ED%94%84%EB%A7%81-%EB%A6%AC%EC%95%A1%ED%8B%B0%EB%B8%8C-%EA%B0%9C%EC%9A%94/#webflux](https://kouzie.github.io/spring/Spring-Boot-스프링-리액티브-개요/#webflux))

# Reactor

[Reactor 써보기](http://wonwoo.ml/index.php/post/category/web/spring)

## Exception Handler

스프링 부트에서 공통 에러를 처리하는 것은 MVC와 조금 다름



### 에러 처리 방법

- `onErrorReturn`

에러가 발생할 때 에러 대신에 특정 값을 발생하고 싶을때 사용

~~~java
Flux<Integer> seq = Flux.range(1, 10)
        .map(x -> {
            if (x == 5) throw new RuntimeException("exception");
            else return x;
        })
        .onErrorReturn(-1);

seq.subscribe(System.out::println);
~~~

~~~
1
2
3
4
-1
~~~

> 에러 대신에 -1 값이 리턴



- `onErrorResume`

에러가 발생하면 다른 시퀀스나 에러로 대체할 수 있다.

~~~java
Random random = new Random();
Flux<Integer> seq = Flux.range(1, 10)
        .map(x -> {
            int rand = random.nextInt(8);
            if (rand == 0) throw new IllegalArgumentException("illarg");
            if (rand == 1) throw new IllegalStateException("illstate");
            if (rand == 2) throw new RuntimeException("exception");
            return x;
        })
        .onErrorResume(error -> {
            if (error instanceof IllegalArgumentException) {
                return Flux.just(21, 22);
            }
            if (error instanceof IllegalStateException) {
                return Flux.just(31, 32);
            }
            return Flux.error(error);
        });

seq.subscribe(System.out::println);
~~~



### global Exception

- `DefaultErrorAttribute` 

  글로벌 레벨에서 오류처리를 통해 커스텀한 반환값을 설정하고 싶을때 사용

  - 기본 반환값

  ~~~json
  {
  "timestamp": "...",
  "path": "...",
  "status": 405,
  "error": "Method Not Allowed",
  "message": "",
  "requestId": "ca35d584-1"
  }
  ~~~

  - 커스텀 반환값

  ~~~java
  @Slf4j
  @Component
  @RequiredArgsConstructor
  public class GlobalErrorAttributes extends DefaultErrorAttributes {
  
      // private final MessageSource messageSource;
  
      @Override
      public Map<String, Object> getErrorAttributes(ServerRequest request, ErrorAttributeOptions options) {
          Throwable throwable = getError(request);
          String acceptLanguage = request.headers().firstHeader("accept-language");
          Locale locale = acceptLanguage != null && acceptLanguage.startsWith("ko") ? Locale.KOREA : Locale.getDefault();
          log.error("unknown server error:{}, {}" + throwable.getClass().getCanonicalName(), throwable.getMessage());
          // throwable 의 종류에 맞게 반환값을 조정
          Map<String, Object> map = new HashMap<>();
          map.put("code", UNKNOWN_ERROR_CODE);
          map.put("error", UNKNOWN_ERROR_TYPE);
          // map.put("description", messageSource.getMessage("fms.error.unknown_server_error", null, locale));
          return map;
      }
  }
  ~~~

  ~~~json
  {
  "code": "500-00",
  "error": "UnknownServerError"
  }
  ~~~

  > 핸들러에서 GlobalErrorAttributes.getErrorAttributes 호출되도록 설정하면 된다

- `AbstractErrorWebExceptionHandler` 
  에러 발생시 필터로 등록되어있는 핸들러

  ~~~java
  @Order(-2)
  @Component
  public class GlobalErrorWebExceptionHandler extends AbstractErrorWebExceptionHandler {
      // constructors
      public GlobalErrorWebExceptionHandler(
              GlobalErrorAttributes errorAttributes, WebProperties.Resources resources,
              ApplicationContext applicationContext, ServerCodecConfigurer configurer) {
          super(errorAttributes, resources, applicationContext);
          this.setMessageWriters(configurer.getWriters());
      }
  
      @Override
      protected RouterFunction<ServerResponse> getRoutingFunction(ErrorAttributes errorAttributes) {
          return RouterFunctions.route(RequestPredicates.all(), this::renderErrorResponse);
      }
  
      private Mono<ServerResponse> renderErrorResponse(ServerRequest request) {
          Map<String, Object> errorPropertiesMap = getErrorAttributes(request, ErrorAttributeOptions.defaults());
          Integer status = Integer.valueOf(((String) errorPropertiesMap.get("code")).substring(0, 3));
          return ServerResponse.status(status)
              .contentType(MediaType.APPLICATION_JSON)
              .body(BodyInserters.fromValue(errorPropertiesMap));
      }
  }
  ~~~

  > 기본에러 처리는 order 가 -1로 되어있어서 보다 빠르게 설정

| 인터페이스                    | spring-mvc              | spring-webflux                                       |
| ----------------------------- | ----------------------- | ---------------------------------------------------- |
| 설명                          | ErrorController         | ErrorWebExceptionHandler extends WebExceptionHandler |
| 편의를 위해 추상화된 클래스   | AbstractErrorController | AbstractErrorWebExceptionHandler                     |
| 기본 Bean으로 제공되는 클래스 | BasicErrorController    | DefaultErrorWebExceptionHandler                      |

[Webflux global exception handling](https://www.programmersought.com/article/6869648075/)

[Spring Boot WebFlux](https://blog.seri.la/6)

[webflux - Global Exception : **ResponseStatusException** 사용 ](https://devmingsa.tistory.com/80)

[Spring-webflux에서 Handler](https://supawer0728.github.io/2019/04/04/spring-error-handling/)

[Spring WebFlux - 공식 문서](https://docs.spring.io/spring-framework/docs/5.1.6.RELEASE/spring-framework-reference/web-reactive.html#webflux-exception-handler)

[Exception handling - Spring WebFlux](https://medium.com/@akhil.bojedla/exception-handling-spring-webflux-b11647d8608)





### 

